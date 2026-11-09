# Medical Cost Personal - Insurance Charges Prediction

**Type:** Regression

## Goal
Predict the **yearly medical charges** of an insured person from age, BMI, smoking, health conditions, etc.

**Target:** `annual_medical_charges` (USD per year)

## Data
The data is **synthetic** (fake, created with Python) so it can be big and have all the problems we want to practice on.

- **Generator:** `generate_data.py` (random seed = 123, so you always get the same data)
- **File:** `data/medical_insurance_members.csv` - about 100,000 rows (+0.5% duplicate rows) and 52 columns
- **Data types in the file:** int, float, text (object) and dates saved as text

Re-create (or change) the data:
```bash
python generate_data.py
```
Change `N_ROWS` (100,000 - 300,000) or `SEED` at the top of `generate_data.py` to get a bigger / different dataset.

### Problems added on purpose
- **ID / personal columns** (IDs, names, e-mail, phone, date of birth)
- **Messy column names** (`Annual Income ($)`, `CreditScore`, `No. of Dependents`, ...)
- **Short codes** that should be full values (`M`/`F`, `Y`/`N`, `T`/`F`, `FT`/`PT`, ...)
- **Numbers saved as text** (`$1,250.00`, `12,500`, `34 yrs`, `12.5 km`, `15%`)
- **Missing values**, also hidden as text (`N/A`, `unknown`, `?`, `-`, a space) and one **mostly empty** column
- **Outliers** (extreme values) and **impossible values** (age = 150, negative mileage, 9999 codes, ...)
- **Dates saved as text** in different formats
- **Messy text** (`New York`, `NEW YORK`, `  new york`)
- **Categories saved as numbers** (`region_code`)
- **Useless columns**: a constant column, an almost-constant column, a duplicate column
- **Duplicate rows** (~0.5%)

### Columns
| Column in the file | Clean name | Problems |
|---|---|---|
| `Member ID` | `member_id` | ID / personal detail -> drop |
| `Full Name` | `full_name` | ID / personal detail -> drop |
| `E-mail` | `email` | ID / personal detail -> drop |
| `Phone #` | `phone_number` | ID / personal detail -> drop |
| `DOB` | `date_of_birth` | ID / personal detail -> drop |
| `Age (Years)` | `age` | number saved as text (`12 yrs`); 1% missing; wrong values: `0`, `-1`, `140` |
| `Gender` | `gender` | short codes: `M`, `F` |
| `Marital Status` | `marital_status` | short codes: `S`, `M`, `D`, `W` |
| `Education` | `education` | short codes: `HS`, `BSC`, `MSC`, `PHD` |
| `City` | `city` | 1% missing; mixed upper / lower case, extra spaces |
| `Region Code` | `region_code` | category saved as a number |
| `Preferred Language` | `preferred_language` | - |
| `Marketing Opt-In` | `marketing_opt_in` | short codes: `Y`, `N` |
| `Policy Start Date` | `policy_start_date` | date saved as text (`%Y-%m-%d`) |
| `Height Cm` | `height_cm` | number saved as text (`12 cm`) |
| `Weight Kg` | `weight_kg` | number saved as text (`12 kg`); wrong values: `0`, `900` |
| `BMI` | `bmi` | 3% missing; wrong values: `0`, `99` |
| `children` | `num_children` | - |
| `Smoker?` | `smoker` | short codes: `Y`, `N` |
| `Alcohol Use` | `alcohol_use` | short codes: `N`, `M`, `H` |
| `Exercise Frequency` | `exercise_frequency` | 4% missing; mixed upper / lower case, extra spaces |
| `BP Systolic` | `blood_pressure_systolic` | 3% missing; wrong values: `0`, `400` |
| `BP Diastolic` | `blood_pressure_diastolic` | 3% missing |
| `Cholesterol (mg/dL)` | `cholesterol_mg_dl` | 5% missing; extreme outliers |
| `Glucose Mg Dl` | `glucose_mg_dl` | 5% missing |
| `Has Diabetes` | `has_diabetes` | short codes: `Y`, `N` |
| `Has Hypertension` | `has_hypertension` | - |
| `Has Heart Disease` | `has_heart_disease` | - |
| `Has Asthma` | `has_asthma` | - |
| `Num Chronic Conditions` | `num_chronic_conditions` | - |
| `Doctor Visits Last Year` | `doctor_visits_last_year` | - |
| `Hospital Admissions Last Year` | `hospital_admissions_last_year` | - |
| `Er Visits Last Year` | `er_visits_last_year` | - |
| `Prescription Count` | `prescription_count` | - |
| `Occupation Risk` | `occupation_risk` | short codes: `L`, `M`, `H` |
| `Plan Type` | `plan_type` | - |
| `Deductible` | `deductible` | number saved as text (`$1,250.00`) |
| `Coverage Level` | `coverage_level` | number saved as text (`12.5%`) |
| `Annual Income` | `annual_income` | number saved as text (`$1,250.00`); 4% missing |
| `Employment Status` | `employment_status` | - |
| `Has Dental` | `has_dental` | - |
| `Has Vision` | `has_vision` | - |
| `Family History Heart` | `family_history_heart` | short codes: `T`, `F` |
| `Sleep Hours` | `sleep_hours` | 6% missing |
| `Steps Per Day` | `steps_per_day` | 8% missing; wrong values: `-100`, `150,000` |
| `Diet Quality` | `diet_quality` | mixed upper / lower case, extra spaces |
| `Genetic Test Score` | `genetic_test_score` | 80% missing (mostly empty) |
| `Insurer` | `insurer` | only one value |
| `Policy Currency` | `policy_currency` | only one value |
| `Is Staff Member` | `is_staff_member` | almost only one value |
| `Dependents Count` | `dependents_count` | exact copy of `num_children` |
| `Charges` | `annual_medical_charges` | number saved as text (`$1,250.00`); **TARGET** |

## Files
| File | Description |
|---|---|
| `generate_data.py` | Creates the dataset |
| `medical_cost_prediction.ipynb` | The full project notebook |
| `data/medical_insurance_members.csv` | The dataset |
| `data/medical_insurance_members_metadata.xlsx` | Metadata (data dictionary): description, role, data types, missing values, statistics, codes and problems of every column |
| `models/` | Created when you run the notebook (best model, scaler, selected features) |

## Steps in the notebook
**A. Data cleaning**
- A0 Drop columns that are not useful (IDs, names, ...)
- A1 Rename columns
- A2 Data types: numbers saved as text, messy text, short codes, dates, categories saved as numbers
- A3 Missing values - % per column, drop columns > 50% missing, fill the rest (mean / mode)
- A4 Outliers - box plots, IQR table, wrong values -> median, extreme values -> capping
- A5 Duplicate rows and duplicate columns
- A6 Columns with only one value

**B. EDA**
- B1 Target histogram and box plot
- B2 Correlation heatmap and correlation with the target
- B3 Charts for the independent features (and features vs target)

**C. Feature engineering**
- C1 Binning / new features
- C2 Label encoding and one-hot encoding
- C3 Data splitting: 80% train / 15% test / 5% unseen (random_state = 123)
- C4 Feature scaling (StandardScaler)
- C5 Feature selection (SelectKBest + Random Forest importance)
- C6 20,000-row training sample for comparing / tuning the models

**D. Feature extraction**
- D1 PCA
- D2 Factor Analysis
- D3 Cluster analysis (K-Means with elbow and silhouette)

**E. Machine learning (20 models)**
- E1 Define the models
- E2 Evaluation function (all metrics)
- E3 Cross validation (5 folds)
- E4 Training and testing evaluation
- E5 Hyperparameter dictionary
- E6 Grid search tuning
- E7 Compare the models, select the best, re-train it on the full training data
- E8 Save the best model (`joblib`)
- E9 Load the model
- E10 Predict and evaluate on the 5% unseen data

## How to run
```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost joblib jupyter
python generate_data.py
```
Then open `medical_cost_prediction.ipynb` **from this folder** and run all cells (the full notebook takes about 10-20 minutes).
