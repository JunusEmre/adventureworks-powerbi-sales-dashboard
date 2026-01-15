# adventureworks-powerbi-sales-dashboard
Power BI dashboard (AdventureWorks/SQL Server): executive overview, product drivers, and customer concentration analysis.

AdventureWorks Sales Dashboard (Power BI)
Overview

This project is a Power BI sales analytics report built on the AdventureWorks dataset. The goal is to present an executive-level overview of sales performance, drill into product drivers, and assess customer revenue concentration (dependency risk) using clear KPIs and interactive visuals.

Pages & Insights

1) Executive Sales Overview

Headline KPIs: Total Sales, Total Orders, Total Customers, Avg Order Value (AOV)

Monthly Sales Trend for high-level performance tracking

Sales breakdown by Product Category and Sales Territory

2) Product Performance

Top 10 products by sales

Sales by product subcategory

Category Sales Trend used as an interactive filter for the product-level scatter plot (Revenue vs Units Sold)

3) Customer Revenue Concentration

Active Customers vs Returning Customers (≥2 orders)

Pareto-style “Top Customers” view to visualize concentration

Customer distribution by revenue bands

Dynamic donut chart showing revenue share of Top N customers (TopN selection via helper table)

Data Source & Extraction

The dataset was stored in Microsoft SQL Server. Only the tables and columns needed for the analysis were loaded into Power BI to keep the model lightweight and improve refresh performance. Data was imported into Power BI and modeled using a star-like schema (fact + dimension tables).

Data Model

The model follows a star-like schema:

Fact tables: SalesOrderHeader, SalesOrderDetail

Dimensions: DimDate, Customer, Product, ProductSubcategory, ProductCategory, SalesTerritory

Helper tables (disconnected): RevenueBands, TopN Customers, RankAxis

Screenshot: 
<img width="1568" height="1348" alt="image" src="https://github.com/user-attachments/assets/3af944b4-e97a-4b7f-ad3d-0bad0562a0ed" />

Tools Used

SQL Server – source database for AdventureWorks tables

Power BI Desktop – data modeling, DAX measures, and report design

Power Query – table/column selection and shaping during import

Key Measures (examples)

Returning Customers (2+ Orders)
Counts customers who placed at least two orders within the selected filter context (e.g., date range).

Returning Customers (2+ Orders) = 
VAR CustOrders =
    SUMMARIZE(
        'Sales SalesOrderHeader',
        'Sales SalesOrderHeader'[CustomerID],
        "OrderCount", DISTINCTCOUNT('Sales SalesOrderHeader'[SalesOrderID])
    )
RETURN
COUNTROWS(
    FILTER(CustOrders, [OrderCount] >= 2)
)

How to Use the Report

Use the Date range slicer to adjust the analysis period.

On the Product page, click a category in Category Sales Trend to filter the product-level scatter chart.

On the Customer Concentration page, adjust TopN selection to see how much revenue depends on the top customers.

What This Report Answers

What is the overall sales performance and how does it trend over time?

Which product categories/subcategories and specific products drive revenue?

How concentrated is revenue across customers (dependency risk)?

What portion of revenue comes from the Top N customers?

Notes / Future Enhancements

Add profitability metrics (margin) if the dataset includes cost or margin fields

Add customer retention/cohort analysis if repeat purchasing becomes a key business question

Add “last complete month” logic to avoid partial-month drop in trends (if needed)
