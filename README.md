# 🏥 Hospital ER Analytics Dashboard

An end-to-end **healthcare data analytics and business intelligence project** that transforms Hospital Emergency Room data into curated analytical datasets and an interactive Power BI dashboard using **Azure SQL Database, Microsoft Fabric, and Power BI**.

The project demonstrates the complete data journey from **CSV data preparation and cloud database storage to data ingestion, Lakehouse storage, semantic modeling, DAX calculations, dashboard visualization, and business insights**.

---

## 1. Project Overview

The objective of this project is to build an interactive analytical solution for Hospital Emergency Room data and transform raw patient and visit information into a format that supports healthcare analysis and operational decision-making.

The solution focuses on two connected requirements:

1. Load and organize Hospital ER data in a structured cloud data environment.
2. Build an analytical dashboard that helps understand ER visits, patient volume, waiting time, length of stay, admissions, demographics, and department performance.

The overall solution follows:

**CSV / Raw Hospital Data → VS Code + Python → Azure SQL Database → Microsoft Fabric Dataflow Gen2 → Fabric Lakehouse → Semantic Model → DAX → Power BI**

The final output is an interactive Hospital ER dashboard containing KPI cards, trends, department analysis, patient distribution, demographic analysis, slicers, and filters.

---

## 2. Architecture

The project follows a cloud-based analytical architecture where source data is prepared and loaded into Azure SQL Database, ingested into Microsoft Fabric, stored in a Lakehouse, modeled through a semantic model, and finally consumed through Power BI.

![Hospital ER Project Workflow](screenshots/workflow.png)

### Architecture Components

| Component | Role |
|---|---|
| CSV / Excel | Contains the source Hospital ER dataset |
| VS Code + Python | Reads the CSV and uploads data programmatically |
| Azure SQL Database | Stores the Hospital ER source table |
| Microsoft Fabric | Provides the data and analytics platform |
| Dataflow Gen2 | Ingests and prepares data inside Fabric |
| Fabric Lakehouse | Stores the data for analytical processing |
| Semantic Model | Organizes fields and relationships for reporting |
| DAX | Creates analytical measures and KPI calculations |
| Power BI | Provides dashboard visualization and business insights |

---

## 3. Technology Stack

| Category | Technology | Role |
|---|---|---|
| Cloud Platform | Microsoft Azure | Cloud data platform |
| Data Source | CSV / Excel | Hospital ER source data |
| Database | Azure SQL Database | Stores Hospital ER data |
| Development | VS Code | Python development and data upload |
| Programming | Python | Data loading and processing |
| Data Processing | Pandas | Reading and preparing CSV data |
| Database Connectivity | SQLAlchemy / PyODBC | Connects Python with Azure SQL |
| Data Platform | Microsoft Fabric | Data ingestion and analytics |
| Data Ingestion | Dataflow Gen2 | Loads data into Fabric |
| Cloud Storage | Fabric Lakehouse | Stores analytical data |
| Data Modeling | Fabric Semantic Model | Analytical modeling |
| Business Calculations | DAX | KPI and analytical calculations |
| Visualization | Power BI | Interactive dashboard |

---

## 4. Data Source

The source of the project is a Hospital Emergency Room dataset containing patient and visit information required for analytical reporting.

The dataset supports analysis across areas such as:

- Patient ID
- Patient information
- Visit date
- Age
- Gender
- Race
- Department referral
- Admission status
- Wait time
- Length of stay
- Satisfaction
- Patient and visit metrics

The source CSV is first prepared in the project environment and then uploaded to Azure SQL Database using Python from VS Code.

No database passwords, access tokens, or other sensitive credentials should be included in this repository.

---

## 5. Data Pipeline / Data Flow

The data pipeline is divided into logical stages from source preparation through analytical consumption.

### 5.1 Source Data Preparation

The Hospital ER CSV dataset is maintained as the source data and loaded using Python from VS Code.

```text
Hospital ER CSV
       ↓
Python / Pandas
       ↓
Azure SQL Database
```

Python packages used for the database upload include:

- Pandas
- SQLAlchemy
- PyODBC

### 5.2 Azure SQL Database

Azure SQL Database acts as the structured source database for the project.

The Hospital ER dataset is stored in a table that can be queried using SQL.

```sql
SELECT TOP 10 *
FROM HospitalERData;
```

Record validation can be performed using:

```sql
SELECT COUNT(*) AS TotalRows
FROM HospitalERData;
```

### 5.3 Microsoft Fabric

Microsoft Fabric is used as the analytical data platform after the source data is available in Azure SQL Database.

### 5.4 Dataflow Gen2

Dataflow Gen2 is used to bring the Hospital ER data into the Fabric environment.

```text
Azure SQL Database
       ↓
Dataflow Gen2
       ↓
Fabric Lakehouse
```

### 5.5 Fabric Lakehouse

The Lakehouse provides a centralized analytical storage layer for the Hospital ER data.

The project Lakehouse is:

**HospitalER_Lakehouse**

### 5.6 Semantic Model

The project uses a semantic model named:

**HospitalER_SemanticModel**

The semantic model provides the fields and calculations required by the Power BI report.

### 5.7 Analytical Consumption

```text
Source Data
    ↓
Azure SQL Database
    ↓
Microsoft Fabric
    ↓
Dataflow Gen2
    ↓
HospitalER_Lakehouse
    ↓
HospitalER_SemanticModel
    ↓
DAX Measures
    ↓
Power BI Dashboard
```

---

## 6. Cloud Services and Their Responsibilities

| Service | Purpose in the Project |
|---|---|
| Azure SQL Database | Stores the Hospital ER source data |
| Microsoft Fabric | Provides the analytical data platform |
| Dataflow Gen2 | Ingests and prepares the source data |
| Fabric Lakehouse | Stores the Hospital ER analytical data |
| Semantic Model | Provides the reporting data model |
| Power BI | Provides visualization and business insights |

---

## 7. Data Transformation / Processing

The project transforms raw Hospital ER data into an analytical dataset suitable for dashboard reporting.

The processing workflow includes:

**Source Layer** — Original Hospital ER CSV records.

**Database Layer** — Records uploaded into Azure SQL Database.

**Fabric Layer** — Data ingested using Dataflow Gen2 and stored in the Lakehouse.

**Analytical Layer** — Semantic model exposes fields and measures for reporting.

The project uses analytical calculations for:

- Total ER visits
- Total patients
- Average wait time
- Average length of stay
- Admission analysis
- Department analysis
- Gender analysis
- Patient type analysis
- Satisfaction analysis

---

## 8. Data Model

The Power BI/Fabric semantic model provides the analytical fields required for Hospital ER reporting.

### Core Data Fields

| Field | Purpose |
|---|---|
| PatientID | Identifies patients |
| PatientName | Patient information |
| VisitDate | Date-based analysis |
| Age | Age analysis |
| Age Group | Patient segmentation |
| Gender | Gender analysis |
| Race | Demographic analysis |
| DepartmentReferral | Department performance |
| Admission | Patient type / admission analysis |
| WaitTimeMinutes | Waiting-time analysis |
| Length of Stay | Length-of-stay analysis |
| Satisfaction | Patient satisfaction analysis |

The dashboard supports analysis across:

- Dates
- Departments
- Patient types
- Gender
- Age groups
- Patient visits
- Waiting time
- Length of stay
- Satisfaction

### Date Modeling

The project uses:

- Visit Date
- Month Number
- Month Year

`MonthYear` is sorted using `MonthNumber` so the monthly trend displays chronologically from January through December.

---

## 9. KPI Calculations

### Total ER Visits

```DAX
Total ER Visits =
COUNTROWS('HospitalERData')
```

Target dashboard value: **25,430**

### Total Patients

```DAX
Total Patients 1 =
DISTINCTCOUNT('HospitalERData'[PatientID])
```

Display measure:

```DAX
Total Patients =
FORMAT([Total Patients 1], "#,##0")
```

Target dashboard value: **18,920**

### Average Wait Time

```DAX
Average Wait Time =
ROUND(
    AVERAGE('HospitalERData'[WaitTimeMinutes]),
    0
)
```

Display measure:

```DAX
Average Wait Time Display =
FORMAT([Average Wait Time], "0") & " min"
```

Target dashboard value: **32 min**

### Average Length of Stay

```DAX
Average Length of Stay =
AVERAGE('HospitalERData'[Length of Stay])
```

Display measure:

```DAX
Average Length of Stay Display =
FORMAT([Average Length of Stay], "0.0") & " days"
```

Target dashboard value: **4.2 days**

---

## 10. Dashboard / Analytics

The final consumption layer is an interactive **Hospital Emergency Room Analytics Dashboard** built using Power BI.

### KPI Overview

- **Total ER Visits – 25,430**
- **Total Patients – 18,920**
- **Average Wait Time – 32 min**
- **Average Length of Stay – 4.2 days**

Reference indicators:

- ER Visits: **+12%**
- Total Patients: **+10%**
- Average Wait Time: **-8%**
- Average Length of Stay: **-6%**

### Trend Analysis

The dashboard includes:

- ER Visits Trend
- Monthly ER visit analysis
- Date-based filtering
- Chronological Jan–Dec trend analysis

### Department Analysis

The dashboard provides:

- ER Visits by Department
- Average Wait Time by Department
- Department slicer

### Patient Analysis

Patient Type is represented through the `Admission` field.

Categories include:

- Admitted
- Not Admitted
- Observation
- Other

### Gender Analysis

The dashboard contains a gender distribution visual and Gender slicer.

### Interactive Filtering

The dashboard provides:

- **Date**
- **Department**
- **Patient Type**
- **Gender**

---

## 11. Dashboard Preview

![Hospital ER Dashboard](screenshots/dashboard.png)

---

## 12. Project Workflow

```text
📄 Hospital ER CSV
        ↓
💻 VS Code + Python
        ↓
🗄️ Azure SQL Database
        ↓
🔄 Microsoft Fabric Dataflow Gen2
        ↓
🏠 Fabric Lakehouse
        ↓
📊 Semantic Model
        ↓
📐 DAX Measures
        ↓
📈 Power BI Dashboard
        ↓
💡 Hospital ER Insights
```

---

## 13. Validation / Data Quality

Validation was performed during development to check:

- Total ER visit count
- Unique patient count
- Average wait time
- Average length of stay
- Department filtering
- Patient type filtering
- Gender filtering
- Date filtering
- Monthly trend ordering
- KPI display formatting

The dataset used for the dashboard contains **25,430 ER visit records** and the dashboard target KPI for unique patients is **18,920 patients**.

This section documents dashboard and analytical validation rather than claiming a separate automated data-quality framework.

---

## 14. Key Business Insights

### ER Visit Monitoring

**Business Question:** How many emergency room visits are being handled?

The Total ER Visits KPI provides an overall view of emergency room activity.

### Patient Volume

**Business Question:** How many unique patients are using the emergency department?

The Total Patients KPI provides a unique-patient view of ER activity.

### Waiting Time

**Business Question:** Which departments have higher average patient waiting times?

The Average Wait Time KPI and department-level visual help identify waiting-time patterns.

### Length of Stay

**Business Question:** What is the average amount of time patients remain in the ER?

The Average Length of Stay KPI provides an operational indicator for patient flow.

### Department Performance

**Business Question:** Which departments receive the highest number of ER referrals?

The ER Visits by Department visual helps compare department activity.

### Patient Demographics

**Business Question:** How are ER patients distributed by gender and other demographic attributes?

The dashboard provides demographic analysis using Gender, Age, Age Group, and Race fields.

### Admission Analysis

**Business Question:** What types of patient admission outcomes are represented in the ER data?

The Patient Type analysis uses the Admission field to compare admitted, not admitted, observation, and other categories.

---

## 15. Key Learnings / Skills Demonstrated

### Data Engineering / Data Preparation

- CSV data handling
- Python data loading
- Pandas
- SQL database integration
- Azure SQL Database
- Microsoft Fabric
- Dataflow Gen2
- Fabric Lakehouse

### Data Modeling & Analytics

- Semantic data modeling
- DAX measures
- KPI calculations
- Distinct patient counting
- Average calculations
- Date-based analysis
- Department analysis
- Patient segmentation
- Data filtering

### Business Intelligence

- Power BI dashboard development
- KPI card design
- Interactive slicers
- Trend analysis
- Department analysis
- Patient distribution
- Gender analysis
- Operational analytics
- Dashboard UI design

### Visualization

- KPI cards
- Line charts
- Department charts
- Patient distribution charts
- Gender visuals
- Slicers
- Dynamic Last Updated date
- Dashboard layout and styling

---

## 16. Project Outcome

The project transforms Hospital Emergency Room data into a **structured cloud-based analytical solution and interactive business intelligence dashboard**.

```text
CSV / Raw Hospital Data
        ↓
VS Code + Python
        ↓
Azure SQL Database
        ↓
Microsoft Fabric
        ↓
Dataflow Gen2
        ↓
Fabric Lakehouse
        ↓
Semantic Model
        ↓
DAX Measures
        ↓
Power BI Dashboard
        ↓
Hospital ER Insights
```

The final solution provides a centralized analytical view of:

- ER visits
- Patient volume
- Average waiting time
- Length of stay
- Department performance
- Admission / patient type
- Gender distribution
- Demographics
- Patient satisfaction

The project demonstrates the connection between **cloud data storage, data ingestion, analytical modeling, and business intelligence consumption**.

---

## 17. Project Files

```text
hospital-data-analysis-powerbi/
│
├── README.md
├── Hospital ER Dashboard.pbix
├── Hospital_ER_Dashboard_Data_Matched.csv
│
└── screenshots/
    ├── dashboard.png
    └── workflow.png
```

| File | Description |
|---|---|
| `README.md` | Project documentation |
| `Hospital ER Dashboard.pbix` | Power BI dashboard file |
| `Hospital_ER_Dashboard_Data_Matched.csv` | Hospital ER source dataset |
| `screenshots/dashboard.png` | Dashboard preview |
| `screenshots/workflow.png` | Project workflow diagram |

---

## 18. Author

# Param Shah

### Data Analytics | Business Intelligence | Power BI

This project was developed as a portfolio project to demonstrate practical experience in building an end-to-end **healthcare data analytics and business intelligence solution**, covering data preparation, Azure SQL Database, Microsoft Fabric, Lakehouse storage, semantic modeling, DAX calculations, and interactive Power BI visualization.
