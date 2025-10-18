#  The Companies That Predicted Their Fall — SQL & Excel Analysis

### 📊 Project Overview
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

