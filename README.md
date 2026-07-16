# Power BI Business Intelligence Reporting Solution
An end-to-end Business Intelligence project demonstrating semantic data modeling, reusable business logic, and interactive executive dashboards using Microsoft Power BI.

> **Phase 1**
>
> Semantic Data Modeling
>
> ![Final Semantic Model](https://github.com/TejalK-Hub/Power-BI-Business-Intelligence-Reporting-Solution/blob/main/Phase-1-Semantic-Data-Modeling/data_model.png)

> **Phase 2**
>
> Executive Business Dashboards
>

# Project Summary

This project demonstrates an end-to-end Business Intelligence solution built using Microsoft Power BI. It covers the complete BI workflow from transforming raw operational data into an enterprise-ready semantic model to delivering interactive executive dashboards for business reporting and decision support.

The project is divided into two phases:

- **Phase 1:** Design and development of a scalable Power BI semantic model using dimensional modeling, Galaxy Schema, DAX, Power Query, and Row-Level Security (RLS).
- **Phase 2:** Development of six interactive executive dashboards built on the semantic model to deliver business insights across sales, products, customers, inventory, campaigns, and order operations.

The final solution transforms **23 raw operational tables** into a centralized Business Intelligence model consisting of **6 Dimension tables, 6 Fact tables, 2 Support tables, reusable DAX measures, and 6 executive dashboards**, following enterprise BI best practices.
---

# Phase 1 : Semantic Data Modeling

**phase 1 Summary**

| Item | Details |
|------|---------|
| Tool | Microsoft Power BI |
| Modeling Approach | Star Schema (Galaxy Schema with Multiple Fact Tables) |
| Development Method | Business-first Data Modeling |
| Raw Tables Analyzed | 23 |
| Final Dimension Tables | 6 |
| Final Fact Tables | 6 |
| Support Tables | 2 |
| Total Production Tables | 14 |
| Reusable Measures | 15+ |
| Security | Row-Level Security (RLS) |
| Relationship Type | One-to-Many , One-to-One|
| Filter Direction | Single Direction |
| Fact-to-Fact Relationships | None |
| Date Dimension | Shared Across Multiple Facts |
| Naming Convention | snake_case |
| Table Prefixes | dim_, fact_ |
| Keys | Surrogate Keys |

---

# Architecture

The final semantic model follows a **Galaxy Schema**, consisting of multiple Fact tables connected through shared Dimensions.

```
                Dimensions
                     │
     ┌───────────────┼───────────────┐
     │               │               │
Customer        Product        Geography
     │               │               │
     └──────┬────────┴────────┬──────┘
            │                 │
      Multiple Fact Tables (Star Schema)
```

---

# Final Data Model

## Dimension Tables

- dim_customer
- dim_product
- dim_geo
- dim_campaign
- dim_order_flags
- dim_date

**Total Dimensions:** 6

---

## Fact Tables

- fact_sales
- fact_inventory
- fact_campaign_spend
- fact_sales_target
- fact_order_process
- fact_promotion_coverage (Factless Fact)

**Total Facts:** 6

---

## Support Tables

- security
- _measures

**Total Support Tables:** 2

---

#  Objectives

- Understand business processes before modeling.
- Build a scalable Star Schema.
- Create reusable Dimension tables.
- Build normalized Fact tables.
- Improve data quality.
- Protect business metrics during transformations.
- Create reusable DAX measures.
- Implement Row-Level Security.
- Deliver a production-ready semantic model.

---

#  Workflow

## Step 1 - Business Understanding

Before transforming data:

- Explored all 23 source tables.
- Understood the business processes.
- Identified entities.
- Classified tables into Facts and Dimensions.
- Determined the grain of every table.

Questions answered:

- What does this table represent?
- Is it transactional or descriptive?
- Should it become a Fact or Dimension?
- What does one row represent?

---

## Step 2 - Define Modeling Standards

Established project standards before development.

Implemented:

- Star Schema design
- snake_case naming convention
- Table prefixes (`fact_`, `dim_`)
- Surrogate key naming (`*_key`)
- Business-friendly column names
- Standardized date formatting

---

## Step 3 - Organize Power Query

Created four development folders.

```
Stage
Dimensions
Facts
Support
```

Purpose:

- Better organization
- Easier maintenance
- Cleaner ETL process

---

## Step 4 - Explore Raw Data

Reviewed every source table individually.

Tables analyzed included:

- Customers
- Products
- Orders
- Inventory
- Campaigns
- Payments
- Shipments
- Cities
- Regions
- Sales Targets
- Security
- User Details
- Addresses
- Invoice Data

For each table:

- Identified business purpose.
- Determined grain.
- Checked data quality.
- Planned relationships.

---

## Step 5 - Build Dimension Tables

### Customer Dimension

Merged:

- Customer Master
- Customer Contacts
- User Details
- Addresses
- Cities

Performed:

- Removed dummy records.
- Resolved duplicate customers.
- Merged customer information.
- Removed technical IDs.
- Applied naming standards.
- Created `dim_customer`.

---

### Product Dimension

Merged:

- Products
- Subcategories

Performed:

- Removed duplicate products.
- Split category hierarchy.
- Standardized text.
- Created Product Key.
- Removed unnecessary columns.
- Created `dim_product`.

---

## Step 6 - Transition to Fact Modeling

Used a business-process approach.

Workflow:

```
Business Process

↓

Extract Dimensions

↓

Build Fact

↓

Repeat
```

This reduced complexity while maintaining business context.

---

## Step 7 - Analyze Order Process

Analyzed:

- Orders 2025
- Orders 2026
- Order Line Items

Learned the Header vs Detail pattern.

### Header

One row = One Order

Contains:

- Customer
- Address
- Status
- Order Date

### Detail

One row = One Product within an Order

Contains:

- Product
- Quantity
- Sales
- Cost

Selected the Detail table as the Fact because it represents the lowest grain.

---

## Step 8 - Build Fact Sales

Created `fact_sales` using Order Line Items.

Performed:

- Appended yearly order tables.
- Removed unused columns.
- Merged order information.
- Added Customer Key.
- Added Product Key.
- Added Order Flag Key.
- Added Geography Keys.
- Removed descriptive columns.
- Retained only keys, dates, and measures.

---

## Step 9 - Create Junk Dimension

Created:

```
dim_order_flags
```

Instead of separate dimensions for:

- Order Status
- Priority
- Channel

Stored unique combinations with a surrogate key.

---

## Step 10 - Data Enrichment

Mapped business codes to meaningful values.

Example:

```
10 → Online Store

20 → Retail Partner

30 → Wholesale

40 → Field Sales
```

Improved readability for reporting.

---

## Step 11 - Data Validation

Validated business numbers after every major transformation.

Validation process:

```
Merge

↓

Refresh

↓

Compare Total Sales

↓

Continue only if numbers remain unchanged
```

Detected and fixed:

- Duplicate products
- Incorrect joins
- Inflated totals

---

## Step 12 - Build Geography Dimension

Created:

```
dim_geo
```

Performed:

- Created Geography Key.
- Replaced Ship-To City with key.
- Replaced Bill-To City with key.

Implemented a Role Playing Dimension.

---

## Step 13 - Build Star Schema

Connected all Dimensions to Facts.

Implemented:

- One-to-Many relationships
- Single-direction filtering
- Shared Dimensions

Avoided:

- Fact-to-Fact joins
- Bidirectional filtering
- Circular dependencies

---

## Step 14 - Build Inventory Fact

Original data:

```
Product

Jan

Feb

Mar

Apr
```

Applied:

- Unpivot transformation
- Product lookup
- Product Key
- Removed descriptive fields

Created a normalized inventory fact.

---

## Step 15 - Build Campaign Model

Separated campaign data into:

### Dimension

```
dim_campaign
```

Contains:

- Campaign Name
- Channel
- Budget
- Start Date
- End Date

### Fact

```
fact_campaign_spend
```

Contains:

- Daily Spend
- Clicks
- Impressions

---

## Step 16 - Build Factless Fact

Created:

```
fact_promotion_coverage
```

Contains:

- Campaign Key
- Product Key

No numeric measures.

Used to analyze campaign coverage.

---

## Step 17 - Build Order Process Fact

Modeled the complete order lifecycle.

Business flow:

```
Order

↓

Shipment

↓

Delivery

↓

Invoice

↓

Payment
```

Created an Accumulating Snapshot Fact for process analysis.

Calculated:

- Order to Payment Duration

---

## Step 18 - Standardize the Semantic Model

Performed:

- Standardized date formats.
- Standardized column formatting.
- Removed automatic aggregation from IDs and Keys.
- Applied consistent naming conventions.

---

## Step 19 - Build Date Dimension

Created:

```DAX
dim_date = CALENDARAUTO()
```

Added:

- Year
- Month
- Day

Connected the Date Dimension to multiple Fact tables.

Enabled consistent time-based analysis.

---

## Step 20 - Create Reusable Business Measures

Created a dedicated table:

```
_measures
```

Implemented reusable DAX measures including:

- Total Sales
- Total Orders
- Active Customers
- Total Customers
- Average Order to Payment Days

Purpose:

- Centralized KPI definitions.
- Reduced duplicate calculations.
- Ensured consistent business metrics.

---

## Step 21 - Implement Row-Level Security

Configured security using:

- Security table
- User Email
- Region

Implemented:

- One-to-Many relationship
- USERPRINCIPALNAME()
- Dynamic region-based filtering

Validated access using multiple user accounts.

---

## Step 22 - Final Validation

Performed a complete review of the semantic model.

Verified:

- Star Schema integrity
- Relationship cardinality
- Filter directions
- Data quality
- Business totals
- Naming standards
- Date formatting
- Aggregation behavior
- Security configuration

---

# Data Modeling Concepts Applied

- Star Schema
- Galaxy Schema
- Fact & Dimension Modeling
- Grain Analysis
- Header vs Detail Modeling
- Surrogate Keys
- Lookup Keys
- Conformed Dimensions
- Shared Dimensions
- Role Playing Dimensions
- Junk Dimension
- Factless Fact
- Accumulating Snapshot Fact
- Single Source of Truth
- Data Enrichment
- Data Validation
- Data Quality Checks
- Unpivot Transformation
- CALENDARAUTO()
- DAX Measures
- Row-Level Security (RLS)

---

# Key Learnings

- Understand the business before designing the model.
- Identify the grain before creating relationships.
- Build reusable Dimensions instead of duplicating data.
- Keep Facts at the lowest possible grain.
- Replace descriptive values with surrogate keys.
- Validate business totals after every transformation.
- Never connect Fact tables directly.
- Use shared Dimensions to relate multiple Facts.
- Remove unnecessary columns and tables.
- Follow consistent naming standards.
- Centralize business calculations using reusable measures.
- Use a shared Date Dimension for cross-fact analysis.
- Secure the model using Row-Level Security.
- Validate the semantic model before report development.

---

# Outcome

Successfully transformed **23 raw operational tables** into a **production-ready semantic model** consisting of **6 Dimension tables, 6 Fact tables, and 2 Support tables**, following **Star Schema (Galaxy Schema)** principles.

The final model provides:

- Clean and maintainable architecture
- Accurate and validated business metrics
- Reusable DAX measures
- Improved query performance
- Secure role-based data access
- Scalable foundation for enterprise Power BI reporting

  ---

# Phase 2 - Business Intelligence Dashboards

After completing the semantic model, the next phase focused on transforming the centralized business model into interactive executive dashboards for business reporting and decision support.

All dashboards are built on top of the same semantic model, ensuring consistent business logic, reusable DAX measures, and a single source of truth across the reporting layer.

The reporting solution consists of six domain-focused dashboards, each designed to answer specific business questions through interactive KPIs, visual analytics, and cross-filtering.

---

# Dashboard Overview

| Dashboard | Business Focus |
|-----------|----------------|
| Executive Sales Overview | Executive sales performance and target tracking |
| Product Performance Dashboard| Product, brand, and category analysis |
| Customer Analysis Dashboard| Customer behavior and regional performance |
| Inventory Management | Inventory health and stock planning |
| Campaign Performance Dashboard| Marketing spend and campaign effectiveness |
| Order Process Dashboard| Operational efficiency and order lifecycle analysis |

---

# Business Questions Addressed

## Executive Sales Overview

Provides an executive summary of overall business performance.

**Business Questions**

- How is the business performing over time?
- Are sales meeting business targets?
- Which products contribute the highest revenue?
- Which regions generate the strongest sales performance?

---

## Product Performance Dashboard

Analyzes product portfolio performance across brands and categories.

**Business Questions**

- Which products generate the highest sales?
- Which brands contribute the most revenue?
- How do product categories compare in sales performance?
- Which products consistently outperform others?

---

## Customer Analysis Dashboard

Provides customer-centric insights to support business growth and retention.

**Business Questions**

- How many customers are currently active?
- Which customer segments generate the highest revenue?
- Which regions contribute the most sales and order volume?
- Who are the highest-value customers?
- How long does the average customer payment cycle take?

---

## Inventory Dashboard

Supports inventory planning and stock optimization.

**Business Questions**

- What is the current inventory value and available stock?
- Which products require immediate replenishment?
- Which brands and categories hold the highest inventory?
- What is the current stock distribution?
- Which products require reorder based on minimum stock levels?

---

## Campaign Performance Dashboard

Evaluates marketing effectiveness and budget utilization.

**Business Questions**

- How much has been invested in marketing campaigns?
- Is campaign spending within the allocated budget?
- Which marketing channels consume the highest spend?
- How is campaign expenditure changing over time?
- Which campaigns require further optimization?

---

## Order Process Dashboard

Monitors operational efficiency across the order lifecycle.

**Business Questions**

- How is order volume changing over time?
- Which stage of the order lifecycle takes the longest?
- What is the current distribution of order statuses?
- Which regions experience longer processing cycles?
- Which orders require operational attention based on processing performance?

---

# Dashboard Features

Implemented across all dashboards:

- Interactive slicers and cross-filtering
- Executive KPI cards
- Reusable DAX measures
- Dynamic business KPIs
- Consistent enterprise dashboard theme
- Business-focused visualizations
- Executive reporting layout
- Centralized semantic model integration
- Row-Level Security (RLS) support

---

# Outcome

Phase 1: Successfully transformed the semantic model into a complete Business Intelligence reporting solution consisting of six interactive executive dashboards.

Phase 2: The reporting layer enables business users to monitor performance, analyze trends, identify operational bottlenecks, and make data-driven decisions through a centralized, scalable, and reusable semantic model.
