# Data Cleaning Guide: Turkey University Exam Score Dataset

## 1. Initial Inspection
- Load the data and check shape (`df.shape`)
- View first/last rows (`df.head()`, `df.tail()`)
- Check data types (`df.dtypes`)
- Get summary stats (`df.describe()`, `df.info()`)

---

## 2. Handle Missing Values
- Check for nulls: `df.isnull().sum()`
- Decide per column:
  - **Numerical** (Study_Hours, GPA, etc.): impute with mean/median or drop
  - **Categorical** (Gender, Major, etc.): impute with mode or create "Unknown" category
  - **ID columns**: rows with missing Student_ID should likely be dropped

---

## 3. Remove Duplicates
- Check: `df.duplicated().sum()`
- Full duplicates: drop them
- Duplicate Student_IDs with different data: investigate (data entry error?)

---

## 4. Data Type Validation

| Variable | Expected Type | Check |
|----------|---------------|-------|
| Student_ID | string/int | Unique? |
| Enrollment_Year, Exam_Year | int | Range 2015–2021? |
| Gender, Scholarship_Status, PartTime_Job | category | Only expected values? |
| Socioeconomic_Status | ordered category | Low/Middle/High only? |
| Continuous variables | float | No strings mixed in? |

---

## 5. Logical Consistency Checks
- `Exam_Year >= Enrollment_Year` (can't take exam before enrolling)
- `Attendance_Rate` between 0 and 1 (or 0–100 if percentage)
- `Prior_GPA` between 0.00 and 4.00
- `Exam_Score` between 0 and 100
- `Sleep_Hours_per_Night` reasonable (e.g., 0–24, realistically 3–12)
- `Study_Hours_per_Week` reasonable (0–168 max, realistically 0–80)

---

## 6. Outlier Detection
For continuous variables:
- **Visual**: boxplots, histograms
- **Statistical**: IQR method or Z-score (|z| > 3)
- Variables to check:
  - Study_Hours_per_Week
  - Tutoring_Hours_per_Week
  - Sleep_Hours_per_Night
  - Prior_GPA
  - Exam_Score

---

## 7. Categorical Value Cleaning
- Check unique values: `df['column'].value_counts()`
- Look for:
  - Typos ("Femal" vs "Female")
  - Case inconsistency ("MALE" vs "Male")
  - Whitespace issues (" Male" vs "Male")
- Standardize with `.str.strip().str.title()`

---

## 8. Handle Student_ID
- Confirm uniqueness
- Decide: keep for tracking or drop before modeling (not a predictor)

---

## 9. Feature Engineering Considerations (post-cleaning)
- Create `Years_Until_Exam = Exam_Year - Enrollment_Year`
- Bin continuous variables if needed
- Encode categoricals (one-hot or label encoding)

---

## Summary Checklist

| Step | Task |
|------|------|
| [ ] | Check and handle missing values |
| [ ] | Remove duplicates |
| [ ] | Validate/convert data types |
| [ ] | Check logical constraints |
| [ ] | Detect and handle outliers |
| [ ] | Standardize categorical values |
| [ ] | Verify ID uniqueness |
