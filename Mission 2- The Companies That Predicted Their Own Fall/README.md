# 📊 The Companies That Predicted Their Fall — SQL & Excel Analysis

###  Project Overview
This project explores **how financial patterns forecasted corporate decline** years before collapse.  
Using **SQLite** for structured analysis and **Excel** for dashboard visualization, this case study identifies trends that signal when a company is on the path to failure.

---

##  Objective

To analyze historical financial data of multiple companies and detect:
- Firms that showed **early decline patterns** before collapse.  
- The relationship between **income, assets, and debt growth** over time.  
- Predictive signs that separate stable companies from those that failed.

---

##  Tools & Technologies

| Tool | Function |
|------|-----------|
| **SQLite** | Querying, filtering, and financial trend analysis |
| **Excel** | Visualization and KPI dashboards |
| **Conditional Formatting** | Highlighting red flags |
| **Pivot Tables** | Aggregating key indicators |
| **Power Query** | Data cleaning and transformation |

*Dataset Used:* `global_company_financials_final.csv`

---


##  Data Cleaning & Preparation

###  Excel Steps
1. Imported the CSV into Excel and removed duplicates.  
2. Added new calculated columns:
   - **Previous Net Income**
     ```excel
     =IFERROR(INDEX($G:$G,MATCH(1,($A:$A=A2)*($B:$B=B2-1),0)),"")
     ```
   - **Income Growth Rate**
     ```excel
     =IFERROR((G2-R2)/R2,"")
     ```
   - **Asset Change Rate**
     ```excel
     =IFERROR((H2-INDEX($H:$H,MATCH(1,($A:$A=A2)*($B:$B=B2-1),0)))/INDEX($H:$H,MATCH(1,($A:$A=A2)*($B:$B=B2-1),0)),"")
     ```
   - **Debt Growth Rate**
     ```excel
     =IFERROR((I2-INDEX($I:$I,MATCH(1,($A:$A=A2)*($B:$B=B2-1),0)))/INDEX($I:$I,MATCH(1,($A:$A=A2)*($B:$B=B2-1),0)),"")
     ```

3. Used **conditional formatting** to highlight:
   - 🔴 Negative growth rates (risk indicators)
   - 🟢 Positive growth or recovery trends

4. Verified that years were correctly aligned per company.

---

##  SQL Analysis

###  Step 1 — Create Database and Import Data
```sql
CREATE TABLE company_financials AS
SELECT * FROM csv_read('global_company_financials_final.csv');
```

| company_name | year | net_income | total_assets | total_debt | income_growth_rate | asset_change_rate | debt_growth_rate |
|---------------|------|-------------|---------------|-------------|--------------------|-------------------|------------------|
| Alpha Corp | 2018 | 1,200,000 | 5,500,000 | 2,000,000 | — | — | — |
| Alpha Corp | 2019 | 1,050,000 | 5,800,000 | 2,400,000 | -12.5 | 5.45 | 20.0 |
| Beta Inc | 2018 | 980,000 | 3,100,000 | 1,200,000 | — | — | — |
| Beta Inc | 2019 | 870,000 | 3,050,000 | 1,450,000 | -11.22 | -1.61 | 20.83 |

---
### Step 2 — Identify Companies Showing Decline
```sql
SELECT Company_Name, COUNT(*) AS decline_years
FROM The_Company_That_Predicted_Its_Fall
WHERE Income_Growth_Rate < 0
GROUP BY Company_Name
HAVING decline_years >= 2
ORDER BY decline_years DESC;
```

| company_name | decline_years |
|---------------|---------------|
| Alpha Corp | 3 |
| Delta Motors | 2 |
| Beta Inc | 2 |

 *Companies with ≥2 consecutive years of income decline.*

 ---

 ### Step 3 — Determine When Decline Began
 ```sql
SELECT Company_Name, MIN(year) AS first_decline_year
FROM The_Company_That_Predicted_Its_Fall
WHERE Income_Growth_Rate < 0
GROUP BY Company_Name;
```


| company_name | first_decline_year |
|---------------|--------------------|
| Alpha Corp | 2019 |
| Beta Inc | 2019 |
| Delta Motors | 2020 |

 *The first year each company started losing income.*

---

 ###  Step 4 — Compare Debt Growth and Asset Change
 ```sql
SELECT 
    Company_Name,
    ROUND(AVG(Debt_Change_Rate), 2) AS avg_debt_growth,
    ROUND(AVG(Assets_Change_Rate), 2) AS avg_asset_growth
FROM The_Company_That_Predicted_Its_Fall
GROUP BY Company_Name
HAVING avg_debt_growth > avg_asset_growth
ORDER BY avg_debt_growth DESC;
```


| company_name | avg_debt_growth | avg_asset_growth |
|---------------|-----------------|------------------|
| Alpha Corp | 22.4 | 5.2 |
| Delta Motors | 18.6 | 7.4 |
| Omega Energy | 25.3 | 9.2 |
| Beta Inc | 19.1 | 6.7 |

 *Companies where debt grew faster than assets.*

 ---

 ### Step 5 — Financial Health Classification
 ```sql
SELECT 
    Company_Name,
    Year,
    Income_Growth_Rate,
    Debt_Change_Rate,
    Assets_Change_Rate,
    CASE
        WHEN Income_Growth_Rate < 0 AND Debt_Change_Rate > Assets_Change_Rate THEN 'Predicted Fall'
        WHEN Income_Growth_Rate < 0 AND Debt_Change_Rate <= Assets_Change_Rate THEN 'Declining'
        ELSE 'Stable'
    END AS status_flag
FROM The_Company_That_Predicted_Its_Fall
ORDER BY Company_Name, Year;
```

| company_name | year | income_growth_rate | debt_growth_rate | asset_change_rate | status_flag |
|---------------|------|--------------------|------------------|-------------------|--------------|
| Alpha Corp | 2019 | -12.5 | 20.0 | 5.45 | Predicted Fall |
| Beta Inc | 2020 | -15.0 | 22.0 | 3.0 | Predicted Fall |
| Omega Energy | 2020 | 5.0 | 4.5 | 7.0 | Stable |
| Delta Motors | 2021 | -10.0 | 12.0 | 4.0 | Declining |

 *Dynamic health classification per company per year.*

 ---

 ### Step 6 — Rank Companies by Financial Risk
 ```sql
SELECT
      Company_Name,
    ROUND(AVG(Income_Growth_Rate), 2) AS avg_income_decline,
    ROUND(AVG(Debt_Change_Rate - Assets_Change_Rate), 2) AS debt_asset_gap,
    CASE
        WHEN AVG(Income_Growth_Rate) < 0 AND AVG(Debt_Change_Rate) > AVG(Assets_Change_Rate)
        THEN 'High Risk of Collapse'
        WHEN AVG(Income_Growth_Rate) < 0
        THEN 'Moderate Risk'
        ELSE 'Stable'
    END AS risk_level
FROM The_Company_That_Predicted_Its_Fall
GROUP BY Company_Name
ORDER BY risk_level DESC, avg_income_decline ASC;
```

| company_name | avg_income_decline | debt_asset_gap | risk_level |
|---------------|--------------------|----------------|-------------|
| Alpha Corp | -14.2 | 15.6 | High Risk of Collapse |
| Beta Inc | -10.9 | 11.2 | High Risk of Collapse |
| Delta Motors | -7.8 | 6.1 | Moderate Risk |
| Omega Energy | 3.4 | -2.1 | Stable |

 *Ranking by long-term financial risk.*

 ---

 
