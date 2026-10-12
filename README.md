# Credit Card Customer & Transaction Analysis (Power BI)

## 1. Project Introduction

Credit card businesses generate large volumes of customer and transaction data, but the value lies in understanding who the customers are, how they use their cards, and which segments drive revenue. Without a clear view of these patterns, it is hard to decide where to focus marketing, product, and risk efforts.

This project uses Microsoft Power BI to analyse a credit card dataset covering customer demographics, card categories, spending behaviour, interest earned, and transaction activity. The analysis is split into **two interactive report views**:

- **Credit Card Customer Report:** who the customers are (gender, age, job, education, marital status, dependents, state, income group, asset ownership) and how much revenue each segment contributes.
- **Credit Card Transaction Report:** how the cards are used (card category, payment mode, expense type, quarterly revenue and transaction volume) and what each card type contributes.

The objective is to turn raw customer and transaction records into a compact visual summary that supports business decisions on customer targeting, card portfolio strategy, and spending behaviour.

## 2. Business Problem

Card issuers and banks need to answer questions such as:

- Which customer segments contribute the most revenue?
- Which card categories generate the most revenue and interest?
- How do customers prefer to pay (swipe, chip, or online), and what do they spend on?
- Are there demographic or geographic concentrations in revenue?

When this information sits in raw tables, patterns are difficult to spot and compare. This project addresses that by building two dashboards that consolidate revenue, interest, income, transaction count, transaction value, and customer satisfaction into a single, filterable view.

## 3. Project Objectives

- Analyse total revenue, interest earned, customer income, and customer satisfaction
- Analyse transaction count and transaction value across card categories
- Compare revenue across customer segments: gender, age group, job, education, marital status, dependents, and income group
- Identify the top states by revenue
- Compare revenue by card category (Blue, Silver, Gold, Platinum)
- Understand payment behaviour (swipe, chip, online) and spending by expense type
- Review quarterly revenue against transaction volume
- Build interactive dashboards with slicers for week and card category
- Demonstrate how customer and transaction data can support business and strategic decisions

## 4. Dashboard Overview

### 4.1 Credit Card Customer Report

**KPI Cards**

| KPI | Value |
|---|---|
| Revenue | 1.07M |
| Total Interest | 144.8K |
| Income | 11M |
| Avg. Customer Satisfaction Score | 2.25 |

**Visuals**

- **Revenue by Week:** line chart of revenue split by gender, using week start date.
- **Quarter, Gender, and Income Group tiles:** treemaps showing the revenue split across Q1, male/female, and High/Medium/Low income groups.
- **Customer Job Table:** revenue, income, and interest earned for each job category (Businessman, White-collar, Self-employed, Govt, Blue-collar, Retirees) with totals.
- **Top 5 States:** pie chart of revenue by state (CA, TX, FL, NY, NJ).
- **Car Owner and House Owner:** count of customers by ownership status.
- **Revenue by Income Group:** pie chart comparing High, Medium, and Low income customers.
- **Personal Loan:** count of customers with and without a personal loan.
- **Marital Status:** revenue by Married, Single, and Unknown.
- **Revenue by Education Level:** bar chart from Uneducated through Doctorate.
- **Revenue by Age Group and Gender:** stacked column chart across age bands (20-30 to 60+).
- **Revenue by Dependents:** bar chart by number of dependents.
- **Slicers:** Week Start Date and Card Category (Blue, Gold, Platinum, Silver).

### 4.2 Credit Card Transaction Report

**KPI Cards**

| KPI | Value |
|---|---|
| Revenue | 1.07M |
| Total Interest | 144.8K |
| Total Transaction Count | 13K (12,543) |
| Total Transaction Value | 869K |

**Visuals**

- **Card Category Summary Table:** revenue, transaction amount, interest earned, and transaction volume for Blue, Gold, Platinum, and Silver cards.
- **Qtr Revenue and Total Transactions:** combo chart (column plus marker) comparing revenue with transaction volume by quarter.
- **Total Revenue by Customer Job:** bar chart by job category.
- **Total Revenue by Use Chip:** revenue by Swipe, Chip, and Online.
- **Total Revenue by Expense Types:** Bills, Fuel, Entertainment, Travel, Food, and Grocery.
- **Total Revenue by Education Level:** bar chart by education level.
- **Total Revenue by Card Type:** bar chart by Blue, Silver, Gold, and Platinum.
- **Slicers:** Week Start Date and Card Category, plus quarter, gender, and income group tiles that act as filters.

## 5. Tools and Technologies

- **Microsoft Power BI Desktop:** data modelling, DAX, and interactive reporting
- **Power Query:** data cleaning and transformation
- **DAX (Data Analysis Expressions):** measures and calculated columns
- **Visualisation:** KPI cards, tables, treemaps, bar and column charts, pie charts, line chart, combo chart, slicers

## 6. Data Model

The report is built on two related tables:

| Table | Description |
|---|---|
| `credit_card` | Transaction-level data: card category, revenue, interest earned, transaction amount, transaction volume, expense type, chip/swipe/online usage, week start date |
| `customer` | Customer attributes: gender, age, job, education, marital status, dependents, state, income, car/house ownership, personal loan, satisfaction score |

The tables are joined on the customer ID (`Client_Num`) with a one-to-many relationship (one customer, many transactions).

## 7. Use of DAX

<!-- Replace table/column names below with the exact ones used in your .pbix file -->

DAX was used for the KPI measures and for derived columns used in grouping.

### 7.1 Measures

```DAX
Revenue = SUM ( credit_card[Revenue] )

Total Interest = SUM ( credit_card[Interest_Earned] )

Income = SUM ( customer[Customer_Income] )

Avg Customer Satisfaction Score =
    AVERAGE ( customer[Cust_Satisfaction_Score] )

Total Transaction Count = SUM ( credit_card[Total_Trans_Vol] )

Total Transaction Value = SUM ( credit_card[Total_Trans_Amt] )
```

### 7.2 Calculated Columns

```DAX
Age Group =
SWITCH (
    TRUE (),
    customer[Customer_Age] < 30, "20-30",
    customer[Customer_Age] < 40, "30-40",
    customer[Customer_Age] < 50, "40-50",
    customer[Customer_Age] < 60, "50-60",
    "60+"
)

Income Group =
SWITCH (
    TRUE (),
    customer[Customer_Income] < 35000, "Low",
    customer[Customer_Income] < 70000, "Med",
    "High"
)

Quarter = "Q" & ROUNDUP ( MONTH ( credit_card[Week_Start_Date] ) / 3, 0 )
```

### 7.3 Additional Measures (Optional Extensions)

```DAX
Interest % of Revenue =
DIVIDE ( [Total Interest], [Revenue] )

Card Revenue Share =
DIVIDE (
    [Revenue],
    CALCULATE ( [Revenue], ALL ( credit_card[Card_Category] ) )
)

Avg Transaction Value =
DIVIDE ( [Total Transaction Value], [Total Transaction Count] )
```

## 8. Methodology

### Step 1: Data Preparation
Loaded the customer and credit card datasets into Power BI and checked field types, missing values, and inconsistent labels.

### Step 2: Data Cleaning and Transformation
Used Power Query to clean the data, standardise categories (for example, job, education, and card type), and create the fields needed for analysis.

### Step 3: Data Modelling
Related the customer and transaction tables through the customer ID so that customer attributes can filter transaction measures.

### Step 4: DAX Development
Created measures for revenue, interest, income, satisfaction, transaction count, and transaction value, along with calculated columns for age group, income group, and quarter.

### Step 5: Visualisation
Built the Customer Report and Transaction Report pages, using consistent KPI cards, colour themes (orange for customers, blue for transactions), and chart types suited to each comparison.

### Step 6: Interactivity
Added slicers for week start date and card category, and enabled cross-filtering between visuals so users can explore specific segments.

### Step 7: Insight Generation
Reviewed the visuals to identify the largest contributors to revenue and the main patterns in customer and spending behaviour.

## 9. Key Analytical Insights

All figures below are taken from the dashboards.

### Overall Performance
- Total revenue is **1.07M**, with **144.8K** in interest earned (about 13.5% of revenue).
- Total transaction value is **869K** across **12,543** transactions.
- Average customer satisfaction score is **2.25**, which may warrant a closer look at service quality.

### Card Category
- **Blue** cards dominate: about **0.91M** of revenue (roughly 84.5%) and 10,924 of 12,543 transactions.
- Silver contributes about 0.09M, Gold about 0.06M, and Platinum about 0.02M.
- Premium cards (Gold and Platinum) together account for a small share of revenue and transactions.

### Customer Segments
- **Gender:** male customers contribute about **677.72K** of revenue, noticeably more than female customers.
- **Age:** revenue peaks in the **40-50** band (0.44M), followed by **50-60** (0.38M); the 20-30 band contributes the least among working-age groups (0.02M).
- **Income group:** **High** income customers generate **55.35%** of revenue (593.27K), Medium 26.26%, Low 18.39%.
- **Job:** **Businessmen** are the top revenue segment (about 0.32M, with 44.2K interest), followed by white-collar and self-employed customers (about 0.19M each). Retirees contribute the least (about 0.09M).
- **Education:** **Graduates** lead (0.48M), followed by High School (0.25M).
- **Marital status:** Married (0.48M) and Single (0.36M) account for most revenue.
- **Dependents:** revenue is highest for customers with 3 dependents (0.36M) and 2 dependents (0.21M).

### Geography
- Revenue is concentrated in four states: **CA (255.42K), TX (232.22K), FL (227.03K), and NY (226.46K)**, each contributing 22-25%. NJ contributes about 7.7%.

### Usage Behaviour
- **Swipe** is the most-used payment mode (0.61M), followed by **Chip** (0.40M); **Online** is small (0.06M).
- **Bills** (0.30M) and **Fuel** (0.20M) are the largest expense categories, followed by Entertainment (0.19M) and Travel (0.16M).

### Customer Profile
- 122 of the customers in the dataset do not own a car and 73 do; 105 do not own a house and 90 do.
- 168 customers have no personal loan and 27 do.

## 10. Conclusion

This project shows how Power BI and DAX can turn customer and transaction data into two connected dashboards that answer practical business questions. The Customer Report explains who is generating revenue, and the Transaction Report explains how and where the cards are being used.

The analysis shows that revenue is heavily concentrated in Blue cards, high-income customers, middle-aged customers (40-60), graduates, and businessmen, and that spending is led by swipe transactions and everyday expenses like bills and fuel. Premium cards and online payments contribute little, which points to possible room for growth.

These findings are a starting point for decisions on targeted marketing, card upgrade campaigns, digital payment promotion, and customer experience improvement.

## 11. Potential Business Applications

- **Customer Targeting:** focus campaigns on the highest-revenue segments, and design offers to grow underrepresented ones (younger customers, women, premium card holders)
- **Card Portfolio Strategy:** evaluate why Gold and Platinum cards have low revenue and test upgrade or benefit strategies
- **Digital Adoption:** promote online payments, which are a small share of revenue today
- **Category Partnerships:** use top expense categories (bills, fuel, entertainment, travel) to design cashback and reward programmes
- **Regional Planning:** prioritise states with the highest revenue concentration
- **Customer Experience:** investigate the low average satisfaction score


## 13. Project Files

- `Credit_Card_Dashboard.pbix`: interactive Power BI report (Customer and Transaction pages)
- `Credit_card_customer_report.png`: Customer Report preview
- `Credit_card_transaction_report.png`: Transaction Report preview
- `README.md`: project documentation, methodology, DAX, and insights
- `data/`: source dataset or instructions for accessing it, subject to usage permissions



