📌 Supply Chain Analytics Dashboard — Executive Summary
1. Project Overview
This dashboard delivers a complete 360° view of end‑to‑end supply chain operations — from procurement and production to inventory, sales, and final delivery.
It is designed for business analysts, operations managers, and supply chain leaders who require real‑time, actionable insights in a clean, executive‑ready interface.

2. Dashboard Structure
📊 Dashboard Pages
Page	Description
| Page            | Description                                                   |
|-----------------|---------------------------------------------------------------|
| Main / Overview | High-level KPI summary — Revenue, Profit, Margin, Orders, Shipments |
| Sales           | Revenue trends, discount analysis, product & customer-level breakdown |
| Inventory       | Stock levels, safety stock, reorder points, days of inventory |
| Procurement     | Supplier orders, lead time, unit cost, quality scores         |
| Production      | Output by facility, defect rates, batch-level tracking        |
| Shipment        | Delivery performance, delays, carrier breakdown, on-time %    |
| Supplier        | Supplier tier analysis, avg quality score, cost comparison    |
| Customer        | Customer segmentation by channel, country, size, and volume   |


3. Data Model Architecture
🗂️ Star Schema Overview
The model follows a star schema with 5 fact tables and 5 dimension tables, connected through 20 active relationships (Many‑to‑One, single‑direction cross‑filter).

📦 Fact Tables
| Table            | Key Columns                                                                 |
|------------------|------------------------------------------------------------------------------|
| fact_sales       | sales_id, date_key, product_id, customer_id, quantity_sold, gross_revenue, net_revenue, profit |
| fact_inventory   | inventory_id, date_key, product_id, facility_id, stock_level, safety_stock_level, reorder_point |
| fact_procurement | procurement_id, order_date_key, product_id, supplier_id, order_qty, total_cost, lead_time_days, quality_score |
| fact_production  | production_id, date_key, product_id, facility_id, quantity_produced, defective_units, defect_rate_pct |
| fact_shipment    | shipment_id, ship_date_key, delivery_date_key, product_id, facility_id, customer_id, status, shipping_cost, delay_reason |



📐 Dimension Tables
| Table        | Key Columns                                                                 |
|--------------|-------------------------------------------------------------------------------|
| dim_date     | date_key, date, year, quarter, month, month_name, week, day, is_weekend      |
| dim_product  | product_id, product_name, category, product_line, unit_price, unit_cost, weight_kg |
| dim_customer | customer_id, customer_name, country, channel_type, size, annual_volume_usd    |
| dim_supplier | supplier_id, supplier_name, country, specialty, tier, avg_quality_score       |
| dim_facility | facility_id, facility_name, country, city, facility_type, specialization, annual_capacity |



🔗 Relationship Map (20 Active)
Sales → Product, Customer, Date
Inventory → Product, Facility, Date
Procurement → Product, Supplier, Order Date
Production → Product, Facility, Date
Shipment → Product, Customer, Facility, Delivery Date

All relationships use Many‑to‑One with single‑direction filtering for optimal performance.

4. DAX Measure Library (39 Total)
| Measure            | Description                                  |
|--------------------|----------------------------------------------|
| Total_Revenue      | Sum of net revenue from fact_sales           |
| Profit             | Net revenue minus total cost                 |
| Profit Margin %    | Profit as a percentage of revenue            |
| Growth_Revenue     | Absolute revenue growth vs prior period      |
| Growth_Revenue %   | YoY or period-over-period growth rate        |
| discount           | Total discount amount applied                |
| discount %         | Discount as a percentage of gross revenue    |


5. 📦 Sales & Orders
| Measure               | Description |
|-----------------------|-------------|
| Total_Sales_Quantity  | Total units sold |
| Total_Sales_Cost      | Total cost of goods sold |
| Order_Qty             | Total number of purchase orders |


6. 🏭 Inventory
| Measure           | Description |
|-------------------|-------------|
| Inventory_Value   | Total value of stock on hand |
| safety_stock      | Aggregate safety stock level |
| Reorder Point     | Inventory reorder trigger threshold |
| Days of Inventory | Inventory days on hand |
| Turnover Rate     | Inventory turnover ratio |


7. 🚚 Shipment & Delivery
| Measure                  | Description |
|--------------------------|-------------|
| Total_Shipment           | Total shipment records |
| Total-Shipment_Quantity  | Total units shipped |
| Total_Delivered_Quantity | Successfully delivered units |
| Total_Delay              | Count of delayed shipments |
| Total_Qty_Delay          | Quantity affected by delays |
| Perfect_Order %          | % of shipments with no issues |
| Shipment_cost            | Total shipping cost |


8. 🔩 Procurement & Suppliers
| Measure           | Description |
|-------------------|-------------|
| Avg_Lead_Time     | Average procurement lead time in days |
| total_Unit_Cost   | Total unit cost across procured items |
| Avg unit cost     | Average unit cost from suppliers |
| avg_quality_scare | Average supplier quality score |


9. ⚙️ Production
| Measure      | Description |
|--------------|-------------|
| Defect_Rate  | Defective units ÷ Total produced |


10. 🎨 HTML Styling Measures (UI/UX Enhancements)
| Measure                               | Purpose |
|----------------------------------------|---------|
| HTML_mainpageDashboard_Header          | Main banner |
| HTML_Page_Title                        | Dynamic page title |
| HTML_Glow_Divider                      | Visual section separator |
| HTML_Border_KPI_Panel                  | KPI card styling |
| HTML_Border_Nav_Shipment               | Navigation card — Shipment page |
| HTML_Border_Nav_Supplier               | Navigation card — Supplier page |
| HTML_Border_Nav_Customer               | Navigation card — Customer page |
| HTML_Border_Metric_Row                 | Donut metric row styling |
| HTML_supplier_Border_KPI_Panel         | Supplier page KPI panel styling |
| HTML_inventoryBorder_KPI_Panel         | Inventory page KPI panel styling |
| HTML_ShipmentBorder_KPI_Panel          | Shipment page KPI panel styling |
| HTML_CustomerBorder_KPI_Panel          | Customer page KPI panel styling |

---

11. 🛠️ Tools & Technologies
| Tool              | Purpose |
|-------------------|---------|
| Power BI Desktop  | Report development & modeling |
| DAX               | KPI logic and business calculations |
| Power Query (M)   | Data transformation |
| Star Schema       | Data modeling pattern |
| HTML Measures     | Custom UI styling |


📁 Repository Structure
supply-chain-powerbi/
│
├── README.md                    # Project documentation (this file)
├── supply chain.pbix            # Power BI report file
├── data/
│   ├── fact_sales.csv           # Sales transactions
│   ├── fact_inventory.csv       # Inventory snapshots
│   ├── fact_procurement.csv     # Purchase orders
│   ├── fact_production.csv      # Production batches
│   ├── fact_shipment.csv        # Shipment records
│   ├── dim_product.csv          # Product master
│   ├── dim_customer.csv         # Customer master
│   ├── dim_supplier.csv         # Supplier master
│   ├── dim_facility.csv         # Facility master
│   └── dim_date.csv             # Date dimension
├── screenshots/
│   ├── overview.png             # Main dashboard screenshot
│   ├── sales.png
│   ├── inventory.png
│   ├── shipment.png
│   └── supplier.png
└── docs/
    └── data_dictionary.md       # Column-level descriptions

🚀 Getting Started
Prerequisites
> Power BI Desktop (latest version recommended)
> Windows OS

🔑 Key Business Insights Enabled
Revenue & Profitability Tracking — Monitor gross/net revenue, profit margin, and discount impact over time
Inventory Health — Identify stockouts, safety stock breaches, and slow-moving inventory
Supplier Performance — Compare lead times, quality scores, and unit costs across supplier tiers
Shipment Reliability — Track Perfect Order %, on-time delivery, delay reasons, and carrier performance
Production Quality — Defect rate monitoring by facility and product line
Customer Segmentation — Analyze revenue contribution by customer channel, size, and country

👤 Author
Konen Chougle Business Analyst | Data Analyst 📧 konenchougle1@gmail.com

⭐ Acknowledgements
Data model design inspired by real-world supply chain ERP structures
HTML visual measures adapted for Power BI custom UI patterns
Built as a portfolio project demonstrating end-to-end BI development skills




