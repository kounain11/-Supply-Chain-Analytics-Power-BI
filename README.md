# 📦 Supply Chain Analytics Dashboard — Power BI

An end-to-end **Supply Chain Analytics Dashboard** built with Power BI to analyze business performance across sales, inventory, procurement, production, shipments, suppliers, and customers.

The project demonstrates practical Business Intelligence development, including data transformation with Power Query (M), dimensional data modeling, DAX measures, and custom HTML-based visual enhancements.

---

## 📌 Table of Contents

- [1. Project Overview](#1-project-overview)
- [2. Dashboard Structure](#2-dashboard-structure)
- [3. Data Model Architecture](#3-data-model-architecture)
- [4. DAX Measure Library](#4-dax-measure-library)
- [5. HTML-Based UI Enhancements](#5-html-based-ui-enhancements)
- [6. Tools and Technologies](#6-tools-and-technologies)
- [7. Repository Structure](#7-repository-structure)
- [8. Getting Started](#8-getting-started)
- [9. Key Business Insights](#9-key-business-insights)
- [10. Project Highlights](#10-project-highlights)
- [11. Author](#11-author)
- [12. Acknowledgements](#12-acknowledgements)

---

## 1. Project Overview

### 🎯 Objective

The dashboard provides a comprehensive, 360-degree view of supply chain operations, covering the flow of goods and business performance from procurement and production to inventory management, sales, and final delivery.

It is designed for business analysts, operations managers, and supply chain decision-makers who need consolidated performance metrics and actionable operational insights.

### 🔍 Business Areas Covered

- **Sales & Revenue:** Revenue trends, profitability, discounts, and sales volume.
- **Inventory Management:** Stock availability, safety stock, reorder thresholds, and inventory turnover.
- **Procurement:** Purchase quantities, procurement costs, lead times, and supplier performance.
- **Production:** Production output, defective units, and quality performance.
- **Shipment & Logistics:** Delivery performance, shipment costs, delays, and order reliability.
- **Supplier Management:** Supplier quality, cost efficiency, and supplier-tier comparisons.
- **Customer Analytics:** Customer segmentation by geography, channel, business size, and volume.

### 💡 Project Goal

Transform multiple operational datasets into a unified analytical model and an executive-friendly dashboard that supports data-driven business decisions.

---

## 2. Dashboard Structure

The report is organized into eight analytical pages.

| Dashboard Page | Description |
|---|---|
| Main / Overview | Executive summary of revenue, profit, margin, orders, and shipments. |
| Sales | Revenue trends, discount analysis, and product- and customer-level performance. |
| Inventory | Stock levels, safety stock, reorder points, and days of inventory. |
| Procurement | Purchase orders, procurement costs, lead times, and quality scores. |
| Production | Production output, facility performance, and defect-rate analysis. |
| Shipment | Delivery performance, delays, carrier breakdown, and on-time delivery metrics. |
| Supplier | Supplier-tier analysis, quality scores, and procurement cost comparisons. |
| Customer | Customer segmentation by channel, country, business size, and annual volume. |

### 📊 Dashboard Screenshots

Dashboard screenshots are stored in the `screenshots/` directory.

| Page | Preview |
|---|---|
| Main / Overview | ![Main Dashboard](screenshots/overview.png) |
| Sales | ![Sales Dashboard](screenshots/sales.png) |
| Inventory | ![Inventory Dashboard](screenshots/inventory.png) |
| Shipment | ![Shipment Dashboard](screenshots/shipment.png) |
| Supplier | ![Supplier Dashboard](screenshots/supplier.png) |

> **Note:** GitHub displays these images when the referenced files exist at the specified paths and their filenames match exactly.

---

## 3. Data Model Architecture

### 🗂️ Star Schema Overview

The data model follows a **star schema** consisting of five fact tables and five dimension tables.

The fact tables store transactional or operational records, while the dimension tables provide descriptive attributes for filtering, grouping, and analyzing those records.

The model is designed around active, many-to-one relationships with single-direction filtering from dimensions to facts.

### 3.1 Fact Tables

| Table Name | Purpose | Key Columns |
|---|---|---|
| `fact_sales` | Sales transactions and financial performance. | `sales_id`, `date_key`, `product_id`, `customer_id`, `quantity_sold`, `gross_revenue`, `net_revenue`, `profit` |
| `fact_inventory` | Inventory snapshots and stock availability. | `inventory_id`, `date_key`, `product_id`, `facility_id`, `stock_level`, `safety_stock_level`, `reorder_point` |
| `fact_procurement` | Purchase orders, supplier costs, and procurement performance. | `procurement_id`, `order_date_key`, `product_id`, `supplier_id`, `order_qty`, `total_cost`, `lead_time_days`, `quality_score` |
| `fact_production` | Production quantities and manufacturing quality. | `production_id`, `date_key`, `product_id`, `facility_id`, `quantity_produced`, `defective_units`, `defect_rate_pct` |
| `fact_shipment` | Shipment records, delivery status, and logistics costs. | `shipment_id`, `ship_date_key`, `delivery_date_key`, `product_id`, `facility_id`, `customer_id`, `status`, `shipping_cost`, `delay_reason` |

### 3.2 Dimension Tables

| Table Name | Purpose | Key Columns |
|---|---|---|
| `dim_date` | Calendar attributes for time-based analysis. | `date_key`, `date`, `year`, `quarter`, `month`, `month_name`, `week`, `day`, `is_weekend` |
| `dim_product` | Product master data and product attributes. | `product_id`, `product_name`, `category`, `product_line`, `unit_price`, `unit_cost`, `weight_kg` |
| `dim_customer` | Customer master data and segmentation. | `customer_id`, `customer_name`, `country`, `channel_type`, `size`, `annual_volume_usd` |
| `dim_supplier` | Supplier master data and performance attributes. | `supplier_id`, `supplier_name`, `country`, `specialty`, `tier`, `avg_quality_score` |
| `dim_facility` | Facility locations, classifications, and capacity. | `facility_id`, `facility_name`, `country`, `city`, `facility_type`, `specialization`, `annual_capacity` |

### 3.3 Relationship Map

The model connects the fact tables to their corresponding dimensions as follows.

| Fact Table | Dimension Table | Relationship Key |
|---|---|---|
| `fact_sales` | `dim_product` | `product_id` |
| `fact_sales` | `dim_customer` | `customer_id` |
| `fact_sales` | `dim_date` | `date_key` |
| `fact_inventory` | `dim_product` | `product_id` |
| `fact_inventory` | `dim_facility` | `facility_id` |
| `fact_inventory` | `dim_date` | `date_key` |
| `fact_procurement` | `dim_product` | `product_id` |
| `fact_procurement` | `dim_supplier` | `supplier_id` |
| `fact_procurement` | `dim_date` | `order_date_key` |
| `fact_production` | `dim_product` | `product_id` |
| `fact_production` | `dim_facility` | `facility_id` |
| `fact_production` | `dim_date` | `date_key` |
| `fact_shipment` | `dim_product` | `product_id` |
| `fact_shipment` | `dim_customer` | `customer_id` |
| `fact_shipment` | `dim_facility` | `facility_id` |
| `fact_shipment` | `dim_date` | `delivery_date_key` |

**Relationship configuration:**

- Cardinality: Many-to-one (`*:1`).
- Cross-filter direction: Single.
- Dimension tables filter their related fact tables.
- The date dimension supports time-based analysis across business processes.

> **Modeling note:** The relationship map above lists 16 relationships, not 20. Additional relationships should be documented here only if they actually exist in the Power BI model. If shipment analysis also uses the ship date, configure the appropriate date relationship and document whether it is active or inactive. The model should not contain multiple active relationships between the same date dimension and fact table unless the modeling design supports them.

---

## 4. DAX Measure Library

The project includes a reported library of **39 DAX measures** covering financial performance, sales, inventory, procurement, shipments, and production.

The tables below organize the measures by business function. Measure names are retained as provided in the project documentation.

> **Important:** The measures listed here are the documented subset, not a verified inventory of all 39 measures. Add the remaining measures from the Power BI model to make this section a complete 39-measure reference.

### 4.1 Financial Performance

| Measure | Description |
|---|---|
| `Total_Revenue` | Total net revenue from sales transactions. |
| `Profit` | Profit calculated as net revenue minus total cost, according to the implemented business logic. |
| `Profit Margin %` | Profit expressed as a percentage of revenue. |
| `Growth_Revenue` | Absolute revenue growth compared with a previous period. |
| `Growth_Revenue %` | Revenue growth rate compared with the previous year or selected comparison period. |
| `discount` | Total discount amount applied to sales. |
| `discount %` | Discount as a percentage of gross revenue. |

### 4.2 Sales & Orders

| Measure | Description |
|---|---|
| `Total_Sales_Quantity` | Total quantity of units sold. |
| `Total_Sales_Cost` | Total cost of goods sold, according to the implemented calculation. |
| `Order_Qty` | Total number of purchase orders, as defined by the underlying data and measure logic. |

### 4.3 Inventory Management

| Measure | Description |
|---|---|
| `Inventory_Value` | Total value of inventory currently represented in the model. |
| `safety_stock` | Aggregate safety stock level. |
| `Reorder Point` | Inventory threshold used to identify when replenishment may be required. |
| `Days of Inventory` | Estimated number of days of inventory available. |
| `Turnover Rate` | Inventory turnover ratio. |

### 4.4 Shipment & Delivery

| Measure | Description |
|---|---|
| `Total_Shipment` | Total number of shipment records. |
| `Total-Shipment_Quantity` | Total quantity of units shipped. |
| `Total_Delivered_Quantity` | Total quantity of successfully delivered units. |
| `Total_Delay` | Count of delayed shipments. |
| `Total_Qty_Delay` | Quantity of units affected by shipment delays. |
| `Perfect_Order %` | Percentage of orders or shipments meeting the implemented perfect-order criteria. |
| `Shipment_cost` | Total shipping cost. |

### 4.5 Procurement & Supplier Performance

| Measure | Description |
|---|---|
| `Avg_Lead_Time` | Average procurement lead time in days. |
| `total_Unit_Cost` | Total unit-cost value calculated across procured items. |
| `Avg unit cost` | Average unit cost across suppliers or procured items. |
| `avg_quality_scare` | Average supplier quality score. |

> **Naming note:** `avg_quality_scare` is retained as supplied. Verify whether the actual measure is named `avg_quality_score` or something else in the Power BI file before renaming it.

### 4.6 Production Quality

| Measure | Description |
|---|---|
| `Defect_Rate` | Defective units divided by total units produced, expressed according to the implemented measure format. |

### 4.7 HTML-Based Styling Measures

These measures generate HTML-based elements for custom report styling and navigation.

| Measure | Purpose |
|---|---|
| `HTML_mainpageDashboard_Header` | Main dashboard banner. |
| `HTML_Page_Title` | Dynamic page title. |
| `HTML_Glow_Divider` | Decorative section divider. |
| `HTML_Border_KPI_Panel` | KPI panel styling. |
| `HTML_Border_Nav_Shipment` | Shipment navigation card styling. |
| `HTML_Border_Nav_Supplier` | Supplier navigation card styling. |
| `HTML_Border_Nav_Customer` | Customer navigation card styling. |
| `HTML_Border_Metric_Row` | Donut metric row styling. |
| `HTML_supplier_Border_KPI_Panel` | Supplier page KPI panel styling. |
| `HTML_inventoryBorder_KPI_Panel` | Inventory page KPI panel styling. |
| `HTML_ShipmentBorder_KPI_Panel` | Shipment page KPI panel styling. |
| `HTML_CustomerBorder_KPI_Panel` | Customer page KPI panel styling. |

---

## 5. HTML-Based UI Enhancements

The dashboard uses HTML-generating DAX measures to create custom visual elements and improve the report's presentation.

### 🎨 UI/UX Features

- Custom dashboard headers and page titles.
- Styled KPI panels and metric rows.
- Custom borders and visual separators.
- HTML-based navigation cards for selected report pages.
- Consistent styling across the supplier, inventory, shipment, and customer pages.

These enhancements help create a more polished interface while retaining Power BI's analytical capabilities.

**Implementation note:** HTML-generating measures require a visual or custom visual capable of rendering HTML. Native Power BI visuals do not automatically interpret arbitrary HTML returned by a DAX measure.

---

## 6. Tools and Technologies

| Technology | Purpose |
|---|---|
| Power BI Desktop | Report development, data modeling, and visualization. |
| DAX | Business calculations, KPIs, and analytical measures. |
| Power Query (M) | Data extraction, cleaning, transformation, and preparation. |
| Star Schema | Dimensional data modeling and relationship design. |
| HTML & CSS | Custom visual styling through HTML-generating measures and compatible visuals. |
| CSV | Source data storage for fact and dimension tables. |
| Git & GitHub | Version control, project documentation, and portfolio presentation. |

---

## 7. Repository Structure

The repository is organized to separate the Power BI report, source data, screenshots, and supporting documentation.

```text
supply-chain-powerbi/
│
├── README.md
├── supply chain.pbix
│
├── data/
│   ├── fact_sales.csv
│   ├── fact_inventory.csv
│   ├── fact_procurement.csv
│   ├── fact_production.csv
│   ├── fact_shipment.csv
│   │
│   ├── dim_product.csv
│   ├── dim_customer.csv
│   ├── dim_supplier.csv
│   ├── dim_facility.csv
│   └── dim_date.csv
│
├── screenshots/
│   ├── overview.png
│   ├── sales.png
│   ├── inventory.png
│   ├── shipment.png
│   └── supplier.png
│
└── docs/
    └── data_dictionary.md
```

> **Repository note:** This tree represents the intended structure. Ensure the folders and filenames match the files actually committed to GitHub.

---

## 8. Getting Started

### 8.1 Prerequisites

- Windows operating system.
- Microsoft Power BI Desktop.
- Access to the project repository and its source files.

### 8.2 Installation & Setup

**Step 1 — Clone the repository**

Open a terminal and run:

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
```

Replace the placeholder with the actual repository URL.

**Step 2 — Open the project**

Navigate to the cloned repository and open `supply chain.pbix` using Power BI Desktop.

**Step 3 — Verify the data sources**

If Power BI requests updated file locations, configure the source paths to match the local `data/` directory.

**Step 4 — Refresh the dataset**

In Power BI Desktop, select **Home → Refresh** to load the source data and update the report.

**Step 5 — Explore the dashboard**

Navigate through the report pages to analyze sales, inventory, procurement, production, shipment, supplier, and customer performance.

### 8.3 Troubleshooting

| Issue | Suggested Action |
|---|---|
| Data source not found | Update the source paths in Power Query to match the local repository location. |
| Refresh errors | Verify that all required CSV files exist and their column names match the expected schema. |
| Blank visuals | Check relationships, filter context, measure logic, and data availability. |
| HTML displays as text | Use a compatible HTML-rendering visual and verify the measure's output. |
| Incorrect totals | Review the fact-table grain, relationship cardinality, aggregation logic, and duplicate records. |

---

## 9. Key Business Insights

The dashboard supports analysis across six major decision-making areas.

### 💰 Revenue & Profitability

- Monitor gross revenue, net revenue, profit, and profit margin.
- Analyze revenue trends over time.
- Evaluate the impact of discounts on financial performance.
- Compare sales performance across products and customer segments.

### 📦 Inventory Health

- Monitor stock levels and safety stock requirements.
- Identify potential stock shortages and replenishment needs.
- Evaluate reorder thresholds.
- Analyze days of inventory and inventory turnover.

### 🔩 Supplier & Procurement Performance

- Compare supplier lead times and quality scores.
- Evaluate unit costs across supplier groups.
- Identify differences in supplier performance.
- Monitor procurement quantities and total purchasing costs.

### 🚚 Shipment Reliability

- Monitor shipment volumes and shipping costs.
- Analyze delivery delays and their recorded reasons.
- Evaluate delivery performance and perfect-order metrics.
- Compare logistics performance across available categories.

### 🏭 Production Quality

- Monitor production output and defective units.
- Compare defect rates across facilities and product lines.
- Identify potential manufacturing quality issues.

### 👥 Customer Segmentation

- Analyze customer contribution by country and channel.
- Compare performance across customer-size categories.
- Evaluate annual customer volume and revenue contribution.

> The dashboard enables these analyses through its data model and measures. Actual findings, trends, and quantified business outcomes should be added after they have been validated against the report data.

---

## 10. Project Highlights

- **End-to-end BI workflow:** Combines multiple operational datasets in one analytical report.
- **Dimensional modeling:** Uses fact and dimension tables to structure business data.
- **DAX-driven analytics:** Provides measures for financial and operational performance.
- **Custom report presentation:** Uses HTML-generating measures for enhanced visual styling.
- **Multi-functional analysis:** Covers sales, inventory, procurement, production, logistics, suppliers, and customers.
- **Reproducible project structure:** Separates report files, data, screenshots, and documentation.
- **Portfolio-ready documentation:** Provides an organized reference for the model, metrics, and setup process.

---

## 11. Author

**Konen Chougle**

Business Analyst | Data Analyst

- **Email:** [konenchougle1@gmail.com](mailto:konenchougle1@gmail.com)

---

## 12. Acknowledgements

- Data model design inspired by real-world supply chain and ERP data structures.
- HTML-based visual styling inspired by custom UI patterns used in Power BI reporting.
- Developed as a portfolio project demonstrating business intelligence, data modeling, analytical reporting, and dashboard development skills.

---

⭐ **If you find this project useful, consider starring the repository.**
