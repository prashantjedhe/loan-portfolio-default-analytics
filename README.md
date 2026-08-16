# Loan Portfolio & Default Analytics

### Dashboard Link : [View the Interactive Power BI Report](https://app.powerbi.com/groups/433a218b-d4f2-4f2d-8702-9b4f93673912/reports/84ae4574-33fa-4aac-b718-166c1dfceb6b/8bb3f8bd44c6ed12e561?experience=power-bi&bookmarkGuid=e3a99be5f1853d559f9f)

## Problem Statement

This project analyzes a loan application portfolio to understand overall loan performance, applicant financial behavior, and credit risk.

The dashboard was designed to help users answer key business questions such as:

- How large is the overall loan portfolio?
- What is the current default rate and number of defaulted loans?
- How is loan exposure distributed across loan purpose and applicant segments?
- How do income, employment, age, education, and credit-score groups differ?
- Which applicant characteristics are associated with a higher likelihood of default?
- Which low-credit segments contribute the largest share of loan exposure?

The final Power BI report contains three interactive pages:

1. **Loan Portfolio & Default Overview**
2. **Applicant Profile & Financial Behavior**
3. **Credit Risk Intelligence**

---

## Dataset

The project uses a loan application dataset containing applicant demographic, financial, credit, and loan-related attributes.

Important fields used in the analysis include:

- Age
- Age Group
- CreditScore
- Credit score Bins
- Income
- Income Bracket
- EmploymentType
- Education
- MaritalStatus
- InterestRate
- DTIRatio
- LoanAmount
- LoanPurpose
- LoanTerm
- Default
- LoanID
- Loan date / Year

The dataset is processed and analyzed in Power BI using Power Query and DAX.

---

## Tools & Technologies

- **Power BI Desktop**
- **Power BI Service**
- **Power Query**
- **DAX**
- **SQL Server**
- **GitHub**

---

# Steps Followed

## Step 1 — Data Acquisition

The loan dataset was connected to the Power BI environment through the SQL Server / dataflow-based data pipeline.

## Step 2 — Data Preparation

Power Query was used to inspect and prepare the dataset before analysis.

Key preparation activities included:

- Reviewing data types
- Checking for missing and invalid values
- Verifying numerical fields
- Preparing date/year fields
- Ensuring categorical fields were suitable for visualization
- Preparing the dataset for DAX calculations

## Step 3 — Calculated Columns

Several calculated columns were created to support segmentation and analysis.

### Credit Score Bins

```DAX
Credit score Bins =
IF(
    'Loan+Dataset+Link'[CreditScore] <= 450,
    "Very Low",
    IF(
        'Loan+Dataset+Link'[CreditScore] <= 550,
        "Low",
        IF(
            'Loan+Dataset+Link'[CreditScore] <= 650,
            "Medium",
            "High"
        )
    )
)
```

### Age Group

```DAX
Age Group =
IF(
    'Loan+Dataset+Link'[Age] <= 19,
    "Teen",
    IF(
        'Loan+Dataset+Link'[Age] <= 39,
        "Adults",
        IF(
            'Loan+Dataset+Link'[Age] <= 59,
            "Middle Age Adults",
            "Senior Citizens"
        )
    )
)
```

### Income Bracket

```DAX
Income Bracket =
SWITCH(
    TRUE(),
    'Loan+Dataset+Link'[Income] < 30000, "Low Income",
    'Loan+Dataset+Link'[Income] < 60000, "Medium Income",
    "High Income"
)
```

### Year

```DAX
Year =
YEAR('Loan+Dataset+Link'[Loan_Date_DD_MM_YYYY])
```

---

# Step 4 — DAX Measures

The dashboard uses DAX measures to calculate portfolio, applicant, and risk metrics.

### Total Loans

```DAX
Total Loans =
COUNTROWS('Loan+Dataset+Link')
```

### Total Loan Amount

```DAX
Total Loan Amount =
SUM('Loan+Dataset+Link'[LoanAmount])
```

### Average Loan Amount

```DAX
Average Loan Amount =
AVERAGE('Loan+Dataset+Link'[LoanAmount])
```

### Defaulted Loans

```DAX
Defaulted Loans =
CALCULATE(
    [Total Loans],
    'Loan+Dataset+Link'[Default] = TRUE()
)
```

### Default Rate

```DAX
Default Rate =
DIVIDE(
    [Defaulted Loans],
    [Total Loans],
    0
)
```

### Average Income

```DAX
Average Income =
AVERAGE('Loan+Dataset+Link'[Income])
```

### Average Interest Rate

```DAX
Average Interest Rate =
AVERAGE('Loan+Dataset+Link'[InterestRate]) / 100
```

### Low Credit Exposure

```DAX
Low Credit Exposure =
CALCULATE(
    [Total Loan Amount],
    'Loan+Dataset+Link'[Credit score Bins] IN {
        "Low",
        "Very Low"
    }
)
```

### Low Credit Exposure %

```DAX
Low Credit Exposure % =
DIVIDE(
    [Low Credit Exposure],
    [Total Loan Amount],
    0
)
```

### Low Credit Default Rate

```DAX
Low Credit Default Rate =
CALCULATE(
    [Default Rate],
    'Loan+Dataset+Link'[Credit score Bins] IN {
        "Low",
        "Very Low"
    }
)
```

---

# Step 5 — Dashboard Design

A consistent dark financial-analytics theme was used across all three pages.

### Main Colors

| Element | HEX |
|---|---|
| Background | `#071225` |
| Visual Background | `#0E1A33` |
| KPI Cards | `#111C35` |
| Border | `#263B63` |
| Primary Accent | `#00D9C0` |
| Purple Accent | `#7C3AED` |
| Blue Accent | `#2196F3` |
| Warning | `#FFAA00` |
| Risk | `#FF3B4E` |
| Main Text | `#FFFFFF` |
| Secondary Text | `#94A3B8` |

The dashboard also uses:

- Custom header branding
- Loan-themed dashboard icon
- Bookmark-based filter panel
- Consistent KPI cards
- Interactive page navigation
- Dark visual containers
- Risk-based color semantics

---

# Page 1 — Loan Portfolio & Default Overview

## Purpose

The first page provides an executive-level overview of overall portfolio performance and default risk.

### Main KPIs

- Total Loan Amount
- Average Loan Amount
- Default Rate
- Average Interest Rate
- Defaulted Loans

### Main visuals

- Loan exposure by purpose
- Loan growth vs. default rate
- Average income by employment
- Loan exposure by age group
- Key insights panel

### Interactivity

- Dynamic filters
- Bookmark-based filter panel
- Cross-filtering between visuals
- Tooltips
- Page navigation

---

# Page 2 — Applicant Profile & Financial Behavior

## Purpose

The second page focuses on understanding who the applicants are and how financial characteristics differ across applicant segments.

### Main analysis areas

- Applicant demographics
- Income profile
- Credit-score segmentation
- Education profile
- Employment characteristics
- Loan exposure across applicant segments

### Advanced Power BI visuals

- Decomposition Tree
- Matrix / conditional-formatting risk analysis
- Treemap
- Interactive segmentation
- KPI summary cards

The page is designed to move from simple descriptive analysis toward interactive applicant segmentation.

---

# Page 3 — Credit Risk Intelligence

## Purpose

The third page focuses on identifying risk concentration and factors associated with loan default.

### Main KPIs

- Low Credit Exposure
- Low Credit Exposure %
- Overall Default Rate
- Low Credit Default Rate
- Defaulted Loans

### Main visuals

- Default Rate Trend
- Key Influencers
- Default Risk by Credit Segment
- Key Risk Insights

### Key Influencers

The Key Influencers visual is used to explore factors associated with default.

Current dashboard results identify examples such as:

- **Low Income → 2.22× higher likelihood of default**
- **Teen Age Group → 2.17× higher likelihood of default**

These are associations identified by the Power BI analysis and should not be interpreted as causal relationships.

---

# Step 6 — Interactivity

The report includes interactive features designed to improve exploration:

- Year filter
- Credit score filter
- Employment type filter
- Age group filter
- Bookmark-based filter panel
- Reset filters button
- Page navigation
- Cross-filtering
- Tooltips
- Decomposition Tree exploration
- Key Influencers analysis

---

# Key Insights

Based on the current dashboard state, several portfolio-level observations can be highlighted.

### 1. Overall Default Risk

The current dashboard shows an overall default rate of approximately **11.61%**, with approximately **29.65K defaulted loans**.

### 2. Low-Credit Exposure

Low and Very Low credit-score segments represent approximately **45.7% of total loan exposure**, equivalent to about **₹14.9bn** in the current dashboard state.

### 3. Low-Credit Default Rate

The default rate for Low and Very Low credit-score segments is approximately **12.58%**, which is above the overall portfolio default rate.

### 4. Key Influencers

The Key Influencers analysis currently highlights:

- Low Income applicants: **2.22× higher likelihood of default**
- Teen applicants: **2.17× higher likelihood of default**

These findings represent statistical associations identified by the model.

> **Note:** Values may change when report filters are applied.

---

# Power BI Service

The report was published to Power BI Service and configured as a multi-page interactive report.

The final portfolio report is:

**Loan Portfolio & Default Analytics**

The report contains:

- 3 analytical pages
- Interactive filters
- DAX-based KPIs
- Advanced Power BI visuals
- Risk insights
- Interactive exploration

---

# Repository Structure

```text
loan-portfolio-default-analytics/
│
├── Data/
│   └── README.md
│
├── Documentation/
│   └── project-documentation.pdf
│
├── PowerBI/
│   └── Loan Portfolio & Default Analytics.pbix
│
├── Screenshots/
│   ├── page-1-overview.png
│   ├── page-2-applicant-profile.png
│   └── page-3-credit-risk.png
│
└── README.md
```

---

# Project Files

### Power BI Report

`PowerBI/Loan Portfolio & Default Analytics.pbix`

### Documentation

`Documentation/project-documentation.pdf`

### Dashboard Screenshots

Available in the `Screenshots/` folder.

---

# Future Improvements

Potential future enhancements include:

- Direct SQL Server connection for simpler refresh architecture
- Scheduled refresh after the required gateway/capacity configuration
- Additional risk segmentation
- More dynamic narrative insights
- Additional drill-through pages
- Portfolio risk scoring
- Automated monitoring of default trends

---

# Author

**Prashant Jedhe**

Data Science Engineering | Aspiring Data Analyst

---

## Project Status

**Completed — Portfolio Project**

Built with **Power BI, DAX, Power Query and SQL Server**.

