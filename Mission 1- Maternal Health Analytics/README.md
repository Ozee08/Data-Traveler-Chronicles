# 🌍 The Data Traveler Chronicles | Mission 1: Global Maternal Health Analytics

> “If I were a Data Analyst in another time, I’d still be using data to save lives.”

---

## 🧭 Project Overview
**Mission 1** of *The Data Traveler Chronicles* explores **Global Maternal Health Risks** — understanding how physiological indicators influence health outcomes among mothers worldwide.

Using **Python** for data analysis and **Power BI** for storytelling visuals, this project demonstrates how data can reveal patterns capable of shaping public health decisions globally.

---

## 🎯 Objectives
- Analyze global maternal health datasets to uncover patterns in blood pressure, body temperature, and blood sugar.
- Identify high, medium, and low-risk profiles.
- Develop predictive models to estimate maternal risk.
- Visualize insights using interactive Power BI dashboards.

---

## 🧰 Tools & Technologies
- **Python** (Pandas, Matplotlib, Seaborn, Scikit-learn)
- **Power BI**
- **Excel** (for cleaning & preprocessing)
- **Google Colab** (for notebook execution)

---

## 🔍 Exploratory Data Analysis
- Statistical summary of health indicators.
- Correlation analysis between systolic/diastolic pressure, sugar, and temperature.
- Age-group risk distribution.
- Visualization of maternal risk distribution globally.

```python
#  Age and Risk Level Distribution
fig, axes = plt.subplots(1, 2, figsize=(14,5))
sns.histplot(df['Age'], bins=20, ax=axes[0])
axes[0].set_title('Age Distribution')
sns.countplot(x='RiskLevel', data=df, order=['low','medium','high'], ax=axes[1])
axes[1].set_title('Risk Level Counts')
plt.tight_layout()
plt.show()
```
![Exploratory Analysis](./Images/Exploratory%202.png)

```python
#  Correlation Heatmap
num_cols = ['Age','SystolicBP','DiastolicBP','BloodSugar_mg_dL','BodyTemp_C','HeartRate_bpm','FacilityDistance_km','AntenatalVisits']
corr = df[num_cols].corr()
plt.figure(figsize=(7,5))
sns.heatmap(corr, annot=True, cmap='vlag')
plt.title('Correlation Matrix of Vital Signs')
plt.show()
```
![Exploratory Analysis](./Images/Exploratory%205%20.png)

```python
 # Complication Rate by Region
comp_rate = df.groupby('Region')['Complication'].apply(lambda x: (x=='yes').mean()).reset_index(name='Rate')
plt.figure(figsize=(8,4))
sns.barplot(x='Region', y='Rate', data=comp_rate)
plt.xticks(rotation=45)
plt.title('Complication Rate by Region')
plt.show()
![Exploratory Analysis](./Images/Exploratory%207.png)
```
![Exploratory Analysis](./Images/Exploratory%207.png)

---

## 🤖 Predictive Modeling
Two models were trained and evaluated:
1. **Logistic Regression** – Accuracy: 86%
2. **Decision Tree Classifier** – Accuracy: 82%

```python
from sklearn.metrics import ConfusionMatrixDisplay
fig, ax = plt.subplots(1, 2, figsize=(10,4))
ConfusionMatrixDisplay.from_predictions(y_test, y_pred_lr, ax=ax[0])
ax[0].set_title("Logistic Regression")
ConfusionMatrixDisplay.from_predictions(y_test, y_pred_dt, ax=ax[1])
ax[1].set_title("Decision Tree")
plt.tight_layout()
plt.show()
```
![Logistic Regression](./Images/Exploratory%204.png)

```python
plt.figure(figsize=(12,6))
plot_tree(dt, feature_names=features, class_names=['No','Yes'], filled=True, fontsize=7)
plt.title("Decision Tree (max_depth=5)")
plt.show()
```
![Decision Tree](./Images/Exploratory%206.png)
*The Logistic Regression model was selected for interpretability and performance.*

---

## 📊 Power BI Dashboard
**Title:** *Global Maternal Health Risks Insights*

Key visuals included:
- **Key Vitals per Risk Levels**  
- **Blood Sugar with Age Group**  
- **Physiological Patterns Across Risks**  
- **Global Distribution of Risks (Pie Chart)**  
- **Geographical Risk Map**

![Power BI](./Images/Maternal%20Health%20Risks%20Globally.png)

---

## 💡 Insights
- 63.9% of mothers globally fall under **low-risk**, 30.5% **medium**, and 5.6% **high-risk**.
- **Age group 21–30** exhibits the most volatile changes in blood sugar and pressure.
- Predictive monitoring could reduce high-risk cases by ~40%.

---

## 🧠 Reflections
Behind every row of data lies a story — a heartbeat, a mother, a future.
Data doesn’t just inform; it protects.  
This project reminds us that **the smallest insights can have the biggest impact**.

---

## 🔮 Next Mission
**Mission 2:** “The Company That Predicted Its Fall” — A business analytics case exploring early-warning signals of financial instability.

---

# Author

**Duru Chukwuma**

📧 chukwuduru588@gmail.com

🔗 [LinkedIn](https://linkedin.com/in/chukwuma-duru)  
🔗 [Portfolio](https://www.datascienceportfol.io/chukwuduru588)


