# Banking Risk & Loan Performance Analytics

## Project Overview

**Banking Risk & Loan Performance Analytics** is an end-to-end banking analytics project focused on understanding lending, deposits, client banking relationships, and portfolio-level financial patterns.

### Tools
- MySQL
- Python
- Pandas
- Power BI
- DAX

### Objective
Understand how banking data can be used to analyze lending and deposit patterns and identify areas that may require further risk investigation.

> **Important:** This is a descriptive and exploratory analytics project. It does **not** build a machine-learning model to predict whether an applicant will repay a loan.

---

## Business Problem

Banks manage customer, loan, deposit, relationship, and fee information across multiple data sources. The project focuses on questions such as:

- How large is the lending portfolio?
- How large is the deposit portfolio?
- How is lending distributed across banking relationships?
- How are deposits distributed across customer segments?
- How do income, nationality, and engagement relate to banking activity?
- Which portfolio areas should be investigated further from a risk perspective?

---

## Project Workflow

```text
Raw Banking Data
       ↓
CSV / Excel Data
       ↓
MySQL Database
       ↓
Python / Pandas
       ↓
Exploratory Data Analysis
       ↓
Power BI Data Model
       ↓
DAX Measures & Calculated Columns
       ↓
Interactive Banking Dashboard
       ↓
Business Insights
```

---

## Data Sources

The project uses the following tables:

```text
customer_table
banking-realtionships
gender
investment-advisiors
```

These tables contain customer, banking relationship, gender, and investment advisor information and are linked through relevant keys.

---

## MySQL Database

Database:

```text
banking_risk
```

Main customer table:

```text
customer_table
```

Additional tables:

```text
banking-realtionships
gender
investment-advisiors
```

MySQL is used as the database layer before analysis in Python and reporting in Power BI.

---

# Exploratory Data Analysis

Python and Pandas were used to explore the banking data before dashboard development.

The EDA focused on:

- Numerical and categorical variables
- Customer characteristics
- Loan and deposit distributions
- Banking relationships
- Income groups
- Customer engagement duration
- Portfolio-level patterns

The purpose of EDA was to understand the data and identify useful business dimensions for the Power BI report.

---

# Power BI Calculated Columns

## Engagement Days

```DAX
Engagment Days =
DATEDIFF(
    'banking_risk customer_table'[Joined Bank],
    TODAY(),
    DAY
)
```

Calculates the number of days since the customer joined the bank.

## Income Band

```DAX
Income Band =
SWITCH(
    TRUE(),
    'banking_risk customer_table'[Estimated Income] < 100000, "Low",
    'banking_risk customer_table'[Estimated Income] < 300000, "Mid",
    "High"
)
```

Groups customers into Low, Mid, and High income bands.

## Processing Fees

```DAX
Processing Fees =
SWITCH(
    'banking_risk customer_table'[Fee Structure],
    "High", 0.05,
    "Mid", 0.03,
    "Low", 0.01,
    0
)
```

Assigns a processing-fee rate based on the customer's fee structure.

## Engagement Timeframe

```DAX
Engagement Timeframe =
SWITCH(
    TRUE(),
    'banking_risk customer_table'[Engagment Days] < 365, "< 1 Years",
    'banking_risk customer_table'[Engagment Days] < 1825, "< 5 Years",
    'banking_risk customer_table'[Engagment Days] < 3650, "< 10 Years",
    'banking_risk customer_table'[Engagment Days] < 7300, "< 20 Years",
    "> 20 Years"
)
```

Groups customers according to their engagement duration with the bank.

---

# DAX Measures

## Bank Deposit

```DAX
Bank Deposit =
SUM('banking_risk customer_table'[Bank Deposits])
```

## Bank Loan

```DAX
Bank Loan =
SUM('banking_risk customer_table'[Bank Loans])
```

## Business Lending

```DAX
Business Lending =
SUM('banking_risk customer_table'[Business Lending])
```

## Checking Accounts

```DAX
Checking Accounts =
SUM('banking_risk customer_table'[Checking Accounts])
```

## Credit Cards Balance

```DAX
Credit Cards Balance =
SUM('banking_risk customer_table'[Credit Card Balance])
```

## Engagement Length

```DAX
Engagment Length =
SUM('banking_risk customer_table'[Engagment Days])
```

## Foreign Currency Account

```DAX
Foreign Currency Account =
SUM('banking_risk customer_table'[Foreign Currency Account])
```

## Savings Account

```DAX
Savings Account =
SUM('banking_risk customer_table'[Saving Accounts])
```

## Total Credit Card Amount

```DAX
Total CC Amount =
SUM('banking_risk customer_table'[Amount of Credit Cards])
```

## Total Clients

```DAX
Total Clients =
DISTINCTCOUNT('banking_risk customer_table'[Client ID])
```

## Total Deposit

```DAX
Total Deposit =
[Bank Deposit]
    + [Savings Account]
    + [Foreign Currency Account]
    + [Checking Accounts]
```

## Total Fees

```DAX
Total Fees =
SUMX(
    'banking_risk customer_table',
    [Total Loan] *
    'banking_risk customer_table'[Processing Fees]
)
```

## Total Loan

```DAX
Total Loan =
[Bank Loan]
    + [Business Lending]
    + [Credit Cards Balance]
```

---

# Dashboard Structure

The report contains five pages:

1. **Executive Overview**
2. **Lending & Loan Analysis**
3. **Deposits & Funding Analysis**
4. **Banking Portfolio Summary**
5. **Client Details**

---

# 1. Executive Overview

### Filter
- Gender

### KPI Cards
- Total Clients
- Total Loan
- Total Deposit
- Total Fees
- Total Credit Card Amount
- Savings Account Amount

### Purpose
Provides a high-level view of client volume, lending, deposits, fees, and selected account balances.

---

# 2. Lending & Loan Analysis

### Filters
- Banking Relationship
- Gender
- Investment Advisor

### KPI Cards
- Total Loan
- Bank Loan
- Business Lending
- Credit Cards

### Visuals

**Bank Loan by Banking Relationship**

Compares bank loan values across Private Bank, Retail, Commercial, and Institutional relationships.

**Lending Portfolio by Engagement**

Compares Bank Loan, Total Loan, Business Lending, and Credit Card Balance across engagement timeframes.

**Bank Loan by Income Band**

Shows lending across Low, Mid, and High income groups.

**Bank Loan by Nationality**

Compares bank loan values across customer nationality groups.

### Business Questions
- Which banking relationship contributes the most bank loan value?
- How does lending vary across income bands?
- How is loan exposure distributed across nationalities?
- How does lending differ across engagement timeframes?

---

# 3. Deposits & Funding Analysis

### Filters
- Banking Relationship
- Gender
- Investment Advisor

### KPI Cards
- Total Deposit
- Bank Deposit
- Foreign Currency Amount
- Savings Account Amount
- Checking Account Amount

### Visuals

**Bank Deposit by Engagement Timeframe**

Shows bank deposits across customer engagement groups.

**Deposit Portfolio by Nationality**

Compares Total Deposit, Bank Deposit, Savings Account, Checking Accounts, and Foreign Currency Account across nationality groups.

**Bank Deposits by Income Band**

Shows bank deposit values across Low, Mid, and High income groups.

### Business Questions
- Which engagement groups hold more bank deposits?
- How are deposits distributed across nationalities?
- Which income groups contribute most to bank deposits?
- How does deposit composition vary across customer segments?

---

# 4. Banking Portfolio Summary

### Filters
- Banking Relationship
- Gender
- Investment Advisor

### KPI Cards
- Total Clients
- Total Loan
- Bank Loan
- Business Lending
- Total Deposit
- Total Fees
- Bank Deposit
- Checking Account Amount
- Credit Card Amount
- Savings Account Amount
- Foreign Currency Amount
- Client Engagement

### Purpose
Provides a consolidated view of lending, deposits, fees, account balances, and client engagement.

---

# 5. Client Details

### Filters
- Banking Relationship
- Gender
- Investment Advisor

### Client Banking Details

The table includes:

- Name
- Investment Advisor
- Engagement Length
- Total Fees
- Total Credit Card Amount
- Credit Card Balance

### Visuals

**Total Fees by Loyalty Classification**

Compares fees across:
- Jade
- Silver
- Gold
- Platinum

**Fees & Client Engagement by Nationality**

Compares fees and client engagement across nationality groups.

### Purpose
Provides a detailed client-level view for further investigation.

---

# Key Dashboard Metrics

The current dashboard reports:

| Metric | Value |
|---|---:|
| Total Clients | 2,940 |
| Total Loan | $4.38bn |
| Total Deposit | $3.77bn |
| Total Fees | $158.19M |
| Bank Loan | $1.77bn |
| Business Lending | $2.60bn |
| Savings Accounts | $698.73M |
| Checking Accounts | $963.28M |
| Foreign Currency | $89.65M |

These are portfolio values produced from the project dataset and should not be presented as real-world banking benchmarks.

---

# Business Questions

## Lending
- What is the overall lending portfolio?
- What share comes from bank loans, business lending, and credit-card balances?
- Which banking relationships have higher loan values?
- How does loan activity vary by income band?
- How is bank lending distributed across nationalities?
- How does engagement timeframe relate to lending?

## Deposits
- What is the overall deposit portfolio?
- How are deposits split across bank, savings, checking, and foreign-currency accounts?
- How do deposits vary by income band?
- How are deposits distributed across nationalities?
- Does engagement timeframe relate to bank deposits?

## Client Relationships
- How many clients are represented?
- How long have clients been with the bank?
- How do fees vary across loyalty classifications?
- How do fees and engagement vary by nationality?
- How do banking relationships and investment advisors relate to portfolio activity?

---

# Technical Skills Demonstrated

### MySQL
- Database creation
- Multiple related tables
- Data storage
- SQL-based data access

### Python / Pandas
- Exploratory Data Analysis
- Numerical analysis
- Categorical analysis
- Distribution analysis

### Power BI
- Interactive dashboard development
- KPI cards
- Slicers
- Bar charts
- Treemaps
- Tables
- Page navigation
- Drill-through / detail reporting

### DAX
- `SUM`
- `DISTINCTCOUNT`
- `SUMX`
- `SWITCH`
- `TRUE`
- `DATEDIFF`
- Measure composition
- Calculated columns

---

# Analytical Scope and Limitations

This project provides **descriptive and exploratory banking analysis**.

It does not:

- Predict loan repayment
- Automatically approve or reject applicants
- Build a credit-risk classification model
- Prove that a customer segment causes lending risk

The dashboard instead identifies **portfolio patterns and areas that could require further investigation**.

Additional credit-history, repayment, delinquency/default, policy, and regulatory data would be required for a full lending-risk model.

Also, `Engagment Days` uses `TODAY()`, so its value changes as time passes.

---

# Potential Future Extensions

If suitable data is available, the project could be extended with:

- Loan-to-deposit ratio
- Credit-risk segmentation
- Delinquency/default analysis
- Customer profitability
- Portfolio concentration
- Risk indicators by customer segment
- Time-based lending and deposit trends
- Statistical comparison of customer groups

Only implement these when the required data and methodology are available.

---

# Resume Description

## Banking Risk & Loan Performance Analytics

**MySQL, Python, Pandas, Power BI, DAX**

- Analyzed banking customer data to evaluate **$4.38bn in total lending, $3.77bn in deposits, and $158.19M in fees** across customer segments and banking relationships.
- Examined **bank loans, business lending, credit-card balances, and deposit products** by income band, nationality, engagement timeframe, and banking relationship to identify portfolio patterns.
- Built a **5-page Power BI dashboard** using DAX measures and interactive filters for lending, deposits, portfolio summary, and client-level analysis.

---

# GitHub Repository Structure

```text
Banking-Risk-and-Loan-Performance-Analytics/
│
├── data/
│   ├── Banking.csv
│   ├── banking-realtionships.csv
│   ├── gender.csv
│   └── investment-advisiors.csv
│
├── EDA.ipynb
│
├── sql/
│   └── banking_risk_queries.sql
│
├── powerbi/
│   └── banking_risk_dashboard.pbix
│
├── screenshots/
│   ├── executive_overview.png
│   ├── lending_analysis.png
│   ├── deposit_analysis.png
│   ├── portfolio_summary.png
│   └── client_details.png
│
└── README.md
```

---

# Final Project Summary

```text
MySQL
   ↓
Data Preparation
   ↓
Python / Pandas EDA
   ↓
Business Analysis
   ↓
Power BI
   ↓
DAX
   ↓
Interactive Banking Dashboard
   ↓
Portfolio Insights
```

The project demonstrates how a banking dataset can be transformed into a structured analytical report covering **lending, deposits, fees, client relationships, and portfolio patterns**.
