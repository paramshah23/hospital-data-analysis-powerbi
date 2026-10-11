# DAX Measures — Hospital Data Analysis Dashboard

This document describes the DAX measures used in the Hospital Data Analysis Power BI project.

> **Important:** The formulas below are templates. Replace table and column names with the exact names in your Power BI model. Check whether your dataset contains the required fields before using a measure.

## 1. Total Patients

Counts unique patients. Use this when the dataset has a patient identifier.

```DAX
Total Patients =
DISTINCTCOUNT ( 'Patients'[PatientID] )
```

If each row represents exactly one patient and there is no patient ID, you may be able to use:

```DAX
Total Patient Records =
COUNTROWS ( 'Patients' )
```

## 2. Total ER Visits

Counts emergency-room visit records. Replace the table name with the table containing visit records.

```DAX
Total ER Visits =
COUNTROWS ( 'ER Visits' )
```

If your model stores ER visits in a general visits table, use a filter based on the relevant visit-type column:

```DAX
Total ER Visits =
CALCULATE (
    COUNTROWS ( 'Visits' ),
    'Visits'[VisitType] = "ER"
)
```

## 3. Average Length of Stay

Use this only if the dataset has a numeric `LengthOfStay` column.

```DAX
Average Length of Stay =
AVERAGE ( 'Admissions'[LengthOfStay] )
```

If admission and discharge dates exist, calculate the stay in a calculated column first:

```DAX
Length of Stay (Days) =
DATEDIFF ( 'Admissions'[AdmissionDate], 'Admissions'[DischargeDate], DAY )
```

Then create the measure:

```DAX
Average Length of Stay =
AVERAGE ( 'Admissions'[Length of Stay (Days)] )
```

If neither a length-of-stay field nor admission/discharge dates exist, this KPI cannot be calculated accurately from the available data.

## 4. Total Admissions

```DAX
Total Admissions =
COUNTROWS ( 'Admissions' )
```

## 5. Average Waiting Time

Use this only if the dataset contains a numeric waiting-time column.

```DAX
Average Waiting Time (Minutes) =
AVERAGE ( 'ER Visits'[WaitTimeMinutes] )
```

## 6. Total Departments

Counts distinct departments in the department table.

```DAX
Total Departments =
DISTINCTCOUNT ( 'Departments'[DepartmentID] )
```

If there is no department ID, use the department name column:

```DAX
Total Departments =
DISTINCTCOUNT ( 'Departments'[DepartmentName] )
```

## 7. Patients by Gender

Create this measure to count patient records; put the Gender field on the visual axis or legend.

```DAX
Patient Count =
DISTINCTCOUNT ( 'Patients'[PatientID] )
```

## 8. Patients by Department

```DAX
Patients by Department =
DISTINCTCOUNT ( 'Visits'[PatientID] )
```

Place `Departments[DepartmentName]` on the visual axis, provided the relationships in the model allow department filters to reach the visits table.

## 9. Emergency Visit Percentage

```DAX
ER Visit Percentage =
DIVIDE ( [Total ER Visits], [Total Visits], 0 )
```

This requires a `[Total Visits]` measure:

```DAX
Total Visits =
COUNTROWS ( 'Visits' )
```

Format `ER Visit Percentage` as **Percentage** in Power BI.

## 10. Average Length of Stay by Department

Use `[Average Length of Stay]` in a visual with department on the axis. No separate measure is required if relationships are configured correctly.

---

## How to add a measure in Power BI

1. Open the Power BI Desktop report.
2. In the **Data** or **Fields** pane, select the table where you want to store the measure.
3. Select **Modeling → New measure** (or right-click a table and choose **New measure**).
4. Paste one DAX formula into the formula bar.
5. Replace example table/column names with the names in your model.
6. Press **Enter** and confirm there are no errors.
7. Select the measure and set its format (Whole number, Decimal number, or Percentage) as appropriate.
8. Add it to a Card, chart, or table visual and test it with your slicers.

## Suggested KPI cards

- **Total Patients** — `[Total Patients]`
- **Total ER Visits** — `[Total ER Visits]`
- **Average Length of Stay** — `[Average Length of Stay]`
- **Total Admissions** — `[Total Admissions]`
- **Average Waiting Time** — `[Average Waiting Time (Minutes)]`

## Model checks

- Confirm that patient, visit, admission, and department tables have the correct relationships.
- Prefer a unique patient identifier for patient counts; counting rows can overcount patients with multiple visits.
- Check blanks, duplicate records, date types, and numeric types before trusting results.
- Use the actual names from your model. The examples above use illustrative names and may need adjustment.
