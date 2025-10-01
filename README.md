# Specialty Retail BI Dashboard
🔒**IMPORTANT: This project was developed and deployed for the business user under a Non-Disclosure Agreement (NDA) to protect all sensitive business, customer, and operational data. Therefore, the visuals presented are intentionally redacted and only meant to show the layouts and features.**

## Overview
This interactive BI report aims to provide the business owner with a centralized view of key performance indicators (KPIs) for his online specialty retail business. It tracks critical KPIs across sales, inventory, and product performance—down to the brand and model level—enabling leadership to make data-driven decisions that maximize business impact, for their day-to-day operations as well as long-term market planning and strategizing.


## Stakeholder Interview
The project began with an initial stakeholder meeting involving the business owner and head engineer to define project scoping, requirements and review the technical database structure. This was followed by a series of weekly meetings to review dashboard iterations and prioritize new feature requests.

**Key Discovery Questions:**

- What are the most critical metrics you need to track in this dashboard?

- Who is the primary user, and what specific business decisions should the report empower them to make?

- How are retail orders and item categories organized, and how is this structure reflected in the data schema?

**Key Findings & Business Objectives:**
From the initial interview, a core business process was identified: individual SKU parts (e.g., bulbs, clips, rollers, tires) are assembled and sold as kits designed for different brand/models. The data schema successfully captures this many-to-many relationship (see Data Model section).

A primary business objective was established: to identify bestselling brand/models for prioritization. This is especially critical as the business owner plans to expand to other online retail platforms in the near future, requiring clear focus on high-performing products.

**Iterative Development:**
The dashboard evolved iteratively based on ongoing stakeholder feedback. Subsequent meetings led to the incorporation of additional features for daily operational oversight, such as tracking the average shipping price and the number of returns over the past month.


## Data model 
A review of the source database confirms the use of a **star schema**. For analytics purposes in Power BI, the following tables were selectively imported and joined:

<img width="760" height="477" alt="491679176-09fa5aee-086d-4143-914c-08316482c33d" src="https://github.com/user-attachments/assets/997615bd-00fe-46ad-9501-f30ace68c204" />

 **Order Table (Fact Table)**:
This is the central fact table containing transactional data. Key fields include:

- Order ID, Customer Name, Brand Model

- Order DateTime (from which only the date was extracted for analysis)

- Total Cost (including fields for amounts before and after tax and shipping)

A critical ETL step was performed on the SKU IDs field, which was stored in a JSON list format (e.g., {"SKU0014": 1, "SKU0097": 1}). Using Power Query's Record.ToTable function, this column was expanded to create multiple rows per order, resulting in a normalized structure.

 **Product Table (Dimension Table)**
Joined to the Order table via SKU ID. This dimension contains product master data, including:

- Manufacturer Price

- Part Type (e.g., Bulb, Clip, Roller)

 **Date Table (Dimension Table)**: A dedicated date table joined to the Order table on the Order Date. This table enables robust time intelligence calculations and period-over-period analysis(like those used in Monthly Dashboard).

 
##  Redacted View
### Sales Overview
<img width="1495" height="833" alt="493555039-ed49d2b2-f26f-4a18-a3bc-70171d1399bb" src="https://github.com/user-attachments/assets/75b42e3e-a47b-4195-87bb-8920a6a33290" />

<img width="1497" height="830" alt="491728678-d09721cd-17d9-4aad-8f2a-96e7d681b040" src="https://github.com/user-attachments/assets/732956a6-126d-4010-b24b-f8493e0a937d" />


This page provides a high-level overview of the most important KPIs and sales trends for at-a-glance performance monitoring.

- **Sales Time Period**: : A timeline slicer that allows users to select a sales period. This filter is applied to all data on the current page and will automatically propagate to all subsequent pages in the report.

- **KPI cards(Total Sales, Total Profit, Total Number of Orders, Total ROI)**: These cards display the key performance metrics we are tracking. The values dynamically update to reflect the entire dataset or are filtered by the selected timeline and/or brand/models. During development, these KPIs were created as Power Query measures. For example, Total Profit is calculated by summing the difference between the price and manufacturer cost across all unique order IDs.

- **Sales by brandmodel**: A bar chart displaying the top brand/models by sales volume, sorted in descending order. Users can interact with this chart; clicking on a specific brand/model will apply it as a filter to all other visuals on the page.

- **Sales and Orders over time**: A line graph illustrating the monthly trends for both sales and the number of orders over the selected time period. By default, it shows data for all brand/models but can be filtered via interaction with the "Sales by Brand/Model" chart.

- **Insights**: This section utilizes an experimental Power BI AI feature to automatically generate data insights. The generated insights are context-aware and will update to be relevant to the currently selected timeline and/or brand/model filters.


### Brands and Types
<img width="1488" height="828" alt="493555879-efe80af7-3e60-4245-b95f-d0d3b639d6af" src="https://github.com/user-attachments/assets/88510889-92d6-4298-8600-87abe2b3cbb0" />

<img width="1491" height="832" alt="491758472-a0abb0b8-aab0-4d5d-97bc-bbac20685b53" src="https://github.com/user-attachments/assets/fb5f6fca-e417-48da-a34b-f9996fa0a495" />

This page is designed to visualize the relationships between brand/models and individual SKU parts, enabling users to track their sales performance within specified time periods.

- **Sales Time Period**: This timeline slicer is consistent with the previous page and retains the user's filtered time period. Any adjustment made here will globally affect the data context for all relevant pages in the report.

- **Parts Sold by Brandmodel**:  A stacked bar chart displaying the quantity of SKU parts sold per brand/model, categorized by part type (as indicated by a color legend). This chart is interactive; selecting a specific brand/model or part type will filter the "Part Sold by SKU" chart to show the corresponding breakdown.

- **Part Sold by SKU**: A stacked bar chart displaying the quantity of SKU parts sold per SKU ID, categorized by part type. This chart is interactive; selecting a specific SKU ID will filter the "Parts Sold by Brand/Model" chart to show which brand/model accounts for those sales. This two-way interaction facilitates detailed analysis of sales composition and product relationships.


### Inventory
<img width="1486" height="832" alt="491729732-60057db6-03a4-402d-b2f2-9fba19a412d3" src="https://github.com/user-attachments/assets/2ae90a5c-54dd-444d-b3c9-f193b9029924" />

Interactive inventory report for parts and their relationships to brandmodels.

- **Sales Time Period**: This timeline slicer is consistent with the previous page and retains the user's filtered time period. Any adjustment made here will globally affect the data context for all relevant pages in the report.
  
- **Parts Sold by Brandmodel** A table displaying the quantity of SKU parts sold, categorized by type (as indicated by a color legend) for each brand/model. This table is interactive; selecting specific brand/models or part types will filter the subsequent inventory details to show the stock level for the selected item.

- **Availabiltity** This section dynamically updates to display the current inventory status and stock levels for all parts relevanat to the brand/model selected in the table above.

### Monthly Dashboard
<img width="1483" height="830" alt="491760035-bff71e9c-eaac-4b86-ba23-24a8031077a9" src="https://github.com/user-attachments/assets/bd91ad6a-f169-4150-a58a-37edf40418eb" />

 Month-to-month KPIs for operational reporting, created per user request.

- **KPI cards(Orders, Sales, Customers, Profit)**: Track the number of orders, number of unique customers, total sales, and total profit over the last 30 days ("This Month") compared to the previous 30-day period ("Previous Month"). Each card will display net change and percentage change metrics.
 
- **Monthly Profit Trend Breakdown**: Tracks the positive and negative profit changes for each brand/model, ranking them from most positive (green) to most negative (red). This element is interactive; clicking on a brand/model will update the KPI cards to reflect that selection's month-to-month changes.

- **Full refunds**: The number of full refunds issued on orders, comparing the last 30 days (This Month) to the previous 30-day period (Previous Month).
  
- **Average Shipping Cost**: Tracks the trend of the average shipping cost per order over each monthly period.
