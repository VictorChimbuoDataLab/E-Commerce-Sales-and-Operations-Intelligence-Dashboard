# 📊 E-Commerce Sales & Operations Intelligence Dashboard

## Project Overview

This project involved the complete cleaning, transformation, modeling, and analysis of a 7,000+ row e-commerce transactions dataset sourced from multiple retail branches.

The raw dataset contained significant data quality issues including duplicate records, invalid Order IDs, inconsistent date formats, mixed currencies, spelling mistakes, missing values, embedded special characters, duplicate headers, inconsistent customer information, and operational data anomalies.

Using Power Query and Power BI, I transformed the dataset into a reliable analytical model and developed interactive dashboards capable of supporting operational and strategic business decision-making.

---

## 🎯 Business Objective

The objective of this project was to:

* Clean and standardize a highly inconsistent transactional dataset
* Combine multiple source tables into a unified analytical model
* Develop business KPIs using DAX
* Analyze revenue, profitability, fulfillment efficiency, and return performance
* Identify operational bottlenecks and growth opportunities
* Generate actionable business recommendations

---

## 🛠 Tools Used

* Power BI
* Power Query
* DAX
* Excel

---

# 🔍 Dataset Challenges

The raw dataset contained:

* Duplicate Order IDs
* Invalid Order IDs
* Duplicate headers embedded within the dataset
* Excel serial dates
* DD/MM/YYYY dates
* MM/DD/YYYY dates
* Text-based date formats
* Mixed currencies (NGN and USD)
* Multiple decimal delimiters
* Invalid product names
* Inconsistent payment methods
* Inconsistent order statuses
* Special characters embedded in text fields
* Missing values
* Inconsistent phone number formats
* Inconsistent location naming conventions
* Legacy shipment records

---

# 🧹 Power Query Data Cleaning & Transformation

## Data Quality Assessment

Before transformation began, Column Quality was enabled to evaluate data validity, completeness, and error rates across all columns.

Blank rows from both source tables were removed before further transformations.

### Power Query Applied Steps

Insert your Power Query Applied Steps screenshot below.

![Power Query Applied Steps](Power%20Query%20Cleaning%20by%20VC_DataLab.png)

---

## Order ID Cleaning

### Issues Identified

* Text values mixed with numeric IDs
* Invalid values such as:

  * OrderID
  * ORD-
  * -COPY
* Multiple Order IDs stored within a single cell
* Duplicate headers appearing within the dataset

### Actions Performed

* Removed invalid text labels
* Converted Order ID to Whole Number
* Investigated conversion failures using **Keep Errors**
* Identified records containing multiple Order IDs
* Removed unrecoverable records
* Removed null Order IDs
* Audited duplicate records using **Keep Duplicates**
* Removed duplicate Order IDs after validation

---

## Order Date Standardization

The Order Date column contained:

* Excel serial dates
* DD/MM/YYYY dates
* MM/DD/YYYY dates
* Text-based dates

A custom Power Query transformation was developed to standardize all formats into a single Date field.

This included:

* Text trimming
* Locale-aware parsing
* Date conversion
* Excel serial date transformation

---

## Customer Data Standardization

### Customer Name

* Converted names to Proper Case
* Removed special characters
* Trimmed unnecessary spaces

### Email

* Replaced "at" with "@"
* Removed HTML artifacts
* Replaced null values with N/A

### Phone Number

Standardized multiple formats including:

* +234XXXXXXXXXX
* (+234)XXXXXXXXXX
* 0XXXXXXXXXX

Transformation steps:

* Trimmed values
* Extracted valid digits
* Retained last 10 digits
* Applied standardized Nigerian format (+234)

---

## Geographic Data Cleaning

### City

* Corrected character substitutions
* Replaced 0 with O
* Trimmed whitespace

### State

Standardized naming conventions:

* Federal Capital Territory → FCT

### Country

Standardized abbreviations:

* NG → Nigeria
* US → U.S.A.
* GB → Britain

### Postal Code

* Replaced null values with N/A

---

## Product Data Cleaning

### Product ID

* Removed unnecessary spaces

### Product Names

* Reviewed unique values
* Identified naming inconsistencies
* Standardized product names
* Mapped products to appropriate categories

---

## Dataset Consolidation

After completing the initial cleaning phase, both datasets were appended into a single analytical table.

This approach ensured:

* Consistent transformations
* Reduced duplication of effort
* Simplified downstream calculations

Following the append operation:

* Order IDs were revalidated
* Duplicate transactions were removed

---

## Quantity Cleaning

### Issues Identified

* Numeric quantities stored as text

### Actions Performed

* Standardized quantity values
* Converted text values such as "Two" into numeric values

---

## Currency Standardization

### Issues Identified

The Unit Price column contained:

* NGN values
* USD values
* Currency symbols
* Mixed decimal delimiters

### Actions Performed

#### Currency Detection

Created a helper column to identify:

* NGN
* USD

#### Currency Cleanup

Removed:

* $
* USD
* ₦

#### Decimal Correction

Developed custom logic to correct inconsistent decimal separators.

#### Currency Conversion

Converted NGN values into USD using:

**1 USD = 1380 NGN**

#### Finalization

* Removed helper columns
* Standardized numeric formats

---

## Discount Standardization

* Replaced null values with 0
* Converted values into Percentage format

---

## Total Price Reconstruction

Invalid and missing Total Price values were rebuilt using:

**Total Price = Unit Price × Quantity × (1 − Discount)**

Actions performed:

* Created custom calculation column
* Converted to Decimal Number
* Rounded values
* Removed invalid source column

---

## Payment Type Standardization

Corrected inconsistent payment methods:

* cc → Credit Card
* bank → Bank Transfer

Applied:

* Proper Case formatting
* Text trimming

---

## Order Status Standardization

Corrected inconsistent status values:

* R → Returned

Applied:

* Proper Case formatting
* Text trimming

---

## Delivery Date Standardization

Challenges:

* Multiple date formats
* Mixed delimiters
* British and American date formats

Solution:

* Standardized delimiters
* Applied culture-aware date conversion
* Preserved legitimate null delivery dates

---

## Feedback Score Cleaning

Standardized text scores:

* Five → 5
* Four → 4

Handled missing values appropriately.

---

## Notes & Tags Standardization

### Notes

* Null → N/A
* Invalid values → N/A

### Tags

Converted:

```text
["Promo","Web"]
```

Into:

```text
Promo; Web
```

Improving readability and consistency.

---

## Data Cleaning Outcome

The dataset was systematically cleaned and standardized using Power Query.

Data quality issues such as inconsistent formats, invalid records, duplicate entries, mixed data types, incorrect spellings, multiple date formats, currency inconsistencies, and missing values were successfully addressed.

The resulting dataset became structured, consistent, analysis-ready, and suitable for advanced data modeling and business intelligence reporting in Power BI.
# 📐 DAX Development & KPI Framework

After successfully cleaning and transforming the dataset in Power Query, the cleaned data was loaded into Power BI for data modeling, KPI development, business analysis, and dashboard creation.

The objective of the DAX framework was not only to measure sales performance but also to evaluate profitability, customer activity, fulfillment efficiency, return behavior, operational performance, and year-over-year growth.

A total of 24 DAX measures were developed to support dashboard reporting and executive decision-making.

---

## Revenue Metrics

### Orders Total Sales

```DAX
Orders Total Sales =
SUM('Whole Part'[Total Sales])
```

### Orders Net Revenue

```DAX
Orders Net Revenue =
SUM('Whole Part'[Total Price])
```

### Actual Gross Revenue

```DAX
Actual Gross Revenue =
CALCULATE(
    SUM('Whole Part'[Total Sales]),
    'Whole Part'[OrderStatus] = "Delivered"
)
```

### Actual Net Revenue

```DAX
Actual Net Revenue =
CALCULATE(
    SUM('Whole Part'[Total Price]),
    'Whole Part'[OrderStatus] = "Delivered"
)
```

---

## Profitability Metrics

### Orders Net Profit

```DAX
Orders Net Profit =
SUMX(
    'Whole Part',
    'Whole Part'[Total Price] * 0.25
)
```

### Actual Gross Profit

```DAX
Actual Gross Profit =
CALCULATE(
    SUM('Whole Part'[Profit]),
    'Whole Part'[OrderStatus] = "Delivered"
)
```

### Actual Net Profit

```DAX
Actual Net Profit =
CALCULATE(
    SUMX(
        'Whole Part',
        'Whole Part'[Total Price] * 0.25
    ),
    'Whole Part'[OrderStatus] = "Delivered"
)
```

### Lost Profit

```DAX
Lost Profit =
[Actual Gross Profit] - [Actual Net Profit]
```

---

## Customer & Order Metrics

### Total Customers

```DAX
Total Customers =
DISTINCTCOUNT('Whole Part'[CustomerName])
```

### Total Orders

```DAX
Total Orders =
COUNT('Whole Part'[OrderID])
```

---

## Quantity Metrics

### Total Quantity Sold

```DAX
Total Quantity Sold =
SUM('Whole Part'[Quantity])
```

### Actual Quantity Sold

```DAX
Actual Quantity Sold =
CALCULATE(
    SUM('Whole Part'[Quantity]),
    'Whole Part'[OrderStatus] = "Delivered"
)
```

### Lost Quantity

```DAX
Lost Quantity =
[Total Quantity Sold] - [Actual Quantity Sold]
```

---

## Order Status Metrics

### Delivered Orders

```DAX
Delivered Orders =
CALCULATE(
    COUNT('Whole Part'[OrderStatus]),
    'Whole Part'[OrderStatus] = "Delivered"
)
```

### Returned Orders

```DAX
Returned Orders =
CALCULATE(
    COUNT('Whole Part'[OrderStatus]),
    'Whole Part'[OrderStatus] = "Returned"
)
```

### Pending Orders

```DAX
Pending Orders =
CALCULATE(
    COUNT('Whole Part'[OrderStatus]),
    'Whole Part'[OrderStatus] = "Pending"
)
```

### Processing Orders

```DAX
Processing Orders =
CALCULATE(
    COUNT('Whole Part'[OrderStatus]),
    'Whole Part'[OrderStatus] = "Processing"
)
```

### Shipped Orders

```DAX
Shipped Orders =
CALCULATE(
    COUNT('Whole Part'[OrderStatus]),
    'Whole Part'[OrderStatus] = "Shipped"
)
```

### Cancelled Orders

```DAX
Cancelled Orders =
CALCULATE(
    COUNT('Whole Part'[OrderStatus]),
    'Whole Part'[OrderStatus] = "Cancelled"
)
```

---

## Fulfillment & Return Performance Metrics

### Order Delivery Rate

```DAX
Order Delivery Rate =
DIVIDE(
    [Delivered Orders],
    [Total Orders],
    0
)
```

### Return Rate

```DAX
Return Rate =
DIVIDE(
    [Returned Orders],
    ([Returned Orders] + [Delivered Orders]),
    0
)
```

### Average Discount Rate

```DAX
Average Discount Rate =
(
([Orders Total Sales] / [Orders Total Sales])
-
([Orders Net Revenue] / [Orders Total Sales])
)
```

---

## Time Intelligence Metrics

### Actual Revenue LY

```DAX
Actual Revenue LY =
CALCULATE(
    [Actual Net Revenue],
    SAMEPERIODLASTYEAR('Whole Part'[Order Date].[Date])
)
```

### Actual Revenue YoY%

```DAX
Actual Revenue YoY% =
DIVIDE(
    [Actual Net Revenue] - [Actual Revenue LY],
    [Actual Revenue LY],
    0
)
```

### Fulfillment Rate LY

```DAX
Fulfillment Rate LY =
CALCULATE(
    [Order Delivery Rate],
    SAMEPERIODLASTYEAR('Whole Part'[Order Date].[Date])
)
```

### Fulfillment Rate YoY%

```DAX
Fulfillment Rate YoY% =
DIVIDE(
    [Order Delivery Rate] - [Fulfillment Rate LY],
    [Fulfillment Rate LY],
    0
)
```

---

## Dashboard Development

After developing the KPI framework, the cleaned dataset and DAX measures were used to build interactive dashboards focused on Sales Performance, Operations & Fulfillment Analysis, and Executive Decision Support.

The dashboard was designed to answer key business questions relating to:

* Revenue Performance
* Profitability
* Customer Activity
* Product Performance
* Regional Performance
* Fulfillment Efficiency
* Return Management
* Operational Risks
* Strategic Growth Opportunities

The following slicers were added to improve interactivity:

* Year Filter
* Tags Filter
* Payment Method Filter

---

# 📊 Dashboard 1 — Order lifecycle and Revenue Leakage Audit

### Dashboard Screenshot

Replace the image path below with your actual Dashboard 1 screenshot.

```markdown
![Sales Performance Dashboard](Dashboard%20Screenshots/Dashboard1.png)
```

![Order lifecycle and Revenue Leakage Audit Dashboard](Order%20lifecycle-Revenue%20Leakage-Audit.png)

### Visualizations Included

* Total Revenue KPI
* Total Profit KPI
* Total Customers KPI
* Revenue by Product
* Revenue by State
* Monthly Profit Trend
* Product Performance Analysis

### Key Findings

* Fitness Accessories generated the highest potential revenue.
* Borno State generated the highest revenue.
* July recorded the highest monthly profit performance.
* Skincare emerged as one of the strongest performing categories.

---

# 🚚 Dashboard 2 — Operations & Fulfillment Analysis

### Dashboard Screenshot

Replace the image path below with your actual Dashboard 2 screenshot.

```markdown
![Operations Dashboard](Dashboard%20Screenshots/Dashboard2.png)
```

![Fulfillment Rate, Margin, and Discount Impact Review](Fulfillment%20Rate-Margin-Discount%20Impact%20Review.png)

### Visualizations Included

* Returned Orders by Product
* Delivered Orders by Product
* Order Status Distribution
* Revenue vs Returns by State
* Fulfillment Performance Metrics

### Key Findings

* Returned orders represented the largest order status category.
* Fitness Accessories recorded the highest return volume.
* Kano State recorded the highest return volume.
* Fulfillment performance remained significantly below acceptable levels.

---

# 🧠 Executive Insights & Recommendations

### Dashboard Screenshot

Replace the image path below with your Executive Insights page screenshot.

```markdown
![Executive Insights Dashboard](Dashboard%20Screenshots/ExecutiveInsights.png)
```

![Intelligent Insight and Recommendation](Intelligent%20Insight%20and%20Recommendation.png)

This dashboard page was created specifically for management reporting and strategic decision-making.

The page summarized key discoveries, operational risks, growth opportunities, and actionable recommendations derived from the analysis.

---

# 🔍 Strategic Executive Discoveries

## 1. Fulfillment Collapse

The business recorded an extremely low delivery rate of 9.93%, resulting in the loss of approximately 34,103 ordered units.

This indicates a major breakdown in the fulfillment process.

---

## 2. Return Crisis

Approximately 75% of shipped products resulted in returns.

This significantly impacts profitability and increases logistics costs.

---

## 3. Data Integrity Threat

Several orders from previous years remained in "Shipped" status, distorting operational reporting and fulfillment KPIs.

---

## 4. High-Risk Revenue Category

Fitness Accessories generated the highest revenue potential but also recorded the highest return volume.

This makes it one of the highest-risk product categories.

---

## 5. Revenue Growth vs Operational Capacity Gap

Actual Net Revenue increased by 29.25% Year-over-Year.

However, fulfillment performance improved by only 0.25%.

This indicates customer demand is growing significantly faster than operational capacity.

---

## 6. Regional Risk Area

Kano State recorded the highest return volume.

This suggests possible localization, logistics, or customer expectation challenges.

---

## 7. Safe Profit Anchor

Beauty & Personal Care → Skincare generated the highest actual revenue and actual profit while maintaining stronger operational performance.

---

## 8. Strongest Regional Market

Borno State generated the highest Gross Sales, Net Revenue, and Actual Revenue.

---

# 🚀 Executive Recommendations

### 1. Resolve the Fulfillment Crisis

Conduct a comprehensive operational audit across warehouses and fulfillment centers to identify bottlenecks affecting delivery performance.

### 2. Reduce Product Returns

Improve product descriptions, sizing information, product images, and quality assurance procedures.

### 3. Clean Legacy Shipment Records

Implement automated workflows to close outdated shipment records and improve reporting accuracy.

### 4. Stabilize the Fitness Accessories Category

Introduce better product specifications, demonstration videos, and customer education materials.

### 5. Expand Logistics Partnerships

Increase logistics capacity through strategic third-party partnerships.

### 6. Regionalize Inventory Allocation

Move inventory closer to high-performing markets such as Borno State.

### 7. Improve Localization Strategies

Enhance product communication and customer engagement strategies in northern regions, particularly Kano State.

### 8. Scale High-Performing Categories

Increase investment and marketing support for the Skincare category.

### 9. Strengthen the Borno Market

Prioritize inventory allocation and fulfillment resources to maintain leadership in the region.

---

# 📈 Project Outcome

This project demonstrated how extensive data cleaning, transformation, modeling, and business intelligence analysis can convert raw, inconsistent data into actionable business insights.

The final solution provided visibility into:

* Revenue Performance
* Profitability
* Fulfillment Efficiency
* Return Management
* Product Performance
* Regional Performance
* Operational Risks
* Strategic Growth Opportunities

The dashboard equips decision-makers with the information required to improve operational efficiency, reduce losses, optimize logistics, and support long-term business growth.

---

## 👨‍💻 Author

**Victor Chimbuo**

Data Analytics | Power BI | Power Query | DAX | Business Intelligence

Passionate about transforming raw data into actionable insights through analytics, visualization, and business intelligence.
