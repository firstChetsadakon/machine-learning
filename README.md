# 🚀 ETL Credit Card Dataset using Python

This repository implements an **ETL (Extract, Transform, Load) process** for a credit card dataset using **Python and Pandas**, replicating an original **Pentaho** workflow.

The process transforms and cleans applicant and credit data from:
1. **`application_record.csv`** – Applicant demographics and financial details.
2. **`credit_record.csv`** – Applicant credit history and payment status.

The final cleaned dataset is saved as **`Application_Data.csv`**, including **aggregated credit information** for each applicant.  
This dataset is then **prepared for machine learning training** to build a **credit risk prediction model**.

---

## **🔹 Steps in the ETL Process**

### **1️⃣ Extract Data**
- Load `application_record.csv` and `credit_record.csv`.
- Remove **exact duplicate rows** (where all attributes are identical).
- Sort both datasets by `ID` for consistency.

### **2️⃣ Transform Data**
#### **Processing `application_record.csv`**
- Convert `FLAG_OWN_CAR` and `FLAG_OWN_REALTY` (`"Y/N"` → `1/0`).
- Add **constant column** (`Current_Date = 2022-01-01`).
- Compute **Applicant Age** (`DAYS_BIRTH // 365`) and **Years of Working** (`DAYS_EMPLOYED // 365`).
- **Filter applicants** to remove invalid records (`AGE >= 21`).
- **Group by `ID`** to keep only **one unique record per applicant**.

#### **Processing `credit_record.csv`**
- Convert `STATUS` into two categories:
  - `"Good Debt"` → `["C", "X", "0"]`
  - `"Bad Debt"` → `["1", "2", "3", "4", "5"]`
- Create new columns: **`Good_Debt`** and **`Bad_Debt`**.
- **Aggregate credit data by `ID`**, summing `Good_Debt` and `Bad_Debt`.

#### **Merging Processed Data**
- **Join `application_record.csv` with aggregated `credit_record.csv`** on `ID`.
- Fill missing values in **Total_Bad_Debt** and **Total_Good_Debt** with `0`.
- Compute **`Status`** (`1` if Good Debt > Bad Debt, else `0`).
- **Drop rows where `Years_of_Working` is negative** (invalid employment data).

### **3️⃣ Load Data**
- Rename columns to match the expected format.
- Select the final columns for output.
- Save the processed data as **`Application_Data.csv`**.

---

## **🔹 Final Output: `Application_Data.csv`**
| Applicant_ID | Applicant_Gender | Owned_Car | Owned_Realty | Total_Children | Total_Income | Income_Type | Education_Type | Family_Status | Housing_Type | Total_Family_Members | Applicant_Age | Years_of_Working | Total_Bad_Debt | Total_Good_Debt | Status |
|-------------|-----------------|-----------|--------------|---------------|-------------|-------------|---------------|--------------|-------------|--------------------|---------------|----------------|---------------|---------------|--------|
| 123456 | M | 1 | 0 | 2 | 50000 | Working | Higher Education | Married | House | 4 | 35 | 10 | 0 | 5 | 1 |

- `Status = 1` → More **Good Debt** than Bad Debt.
- `Status = 0` → More **Bad Debt** than Good Debt.

---

## **🔹 Preparing Data for Machine Learning Model Training**

After the ETL process, we will **prepare `Application_Data.csv` for model training**.  
We will use **classification models** to predict **credit risk (`Status` column)**.

### **4️⃣ Preprocessing for ML**
- **Handle missing values** → Fill `NaN` values using mean/mode imputation.
- **Encode categorical features** (e.g., `Applicant_Gender`, `Income_Type`, `Education_Type`).
- **Normalize numerical features** (`Total_Income`, `Applicant_Age`, etc.).
- **Split into training and test sets** (80% train, 20% test).

### **5️⃣ Train Machine Learning Model**
We will train different models to **predict credit risk (`Status`)**:
- **Logistic Regression**
- **Random Forest**
- **Gradient Boosting (XGBoost)**
- **Neural Networks**

### **6️⃣ Evaluate Model Performance**
- Use **accuracy, precision, recall, and F1-score** to measure performance.
- Compare different models to find the best one.

---

## **🔹 How to Run the ETL + ML Pipeline?**

### **Prerequisites**
Ensure you have **Python 3.8+** and install the required dependencies:
```bash
pip install pandas numpy scikit-learn xgboost
````
## **🔹 How to Run the ETL Pipeline?**

### **Prerequisites**
Ensure you have **Python 3.8+** and install the required dependencies:
```bash
pip install pandas numpy
```
## **🔹 Run the Script**
```bash
python etl_credit_data.py
```