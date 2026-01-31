# Blinkit-Grocery-Data
Power BI dashboard analyzing Blinkit sales performance across outlets, locations, and product categories. Includes KPIs, sales trends, item analysis, and interactive filters to derive business insights and support data-driven decisions.

Blinkit Sales Performance Analysis (SQL)
📌 Project Overview
This project involves a comprehensive analysis of Blinkit's (formerly Grofers) sales data to extract actionable insights. Using SQL (T-SQL), I performed data cleaning, established key performance indicators (KPIs), and analyzed sales distribution across various parameters like outlet size, location, and item types.

The primary goal was to transform raw data into a structured format that helps in understanding customer preferences and outlet performance.

📊 Key Metrics & KPIs
To measure the business health, the following KPIs were calculated:

Total Sales: The overall revenue generated (expressed in millions).

Average Sales: The mean revenue per order.

Number of Items: Total count of unique products/orders in the system.

Average Rating: Customer satisfaction score across all products.

🛠️ Technical Stack
Database: Microsoft SQL Server (T-SQL)

Key SQL Concepts: * Data Cleaning (CASE, UPDATE)

Aggregations (SUM, AVG, COUNT)

Window Functions (OVER())

Data Transformation (PIVOT, CAST)

Conditional Formatting (ISNULL)

📂 Project Structure
1. Data Cleaning
The initial dataset contained inconsistent labels for fat content (e.g., 'LF', 'low fat', and 'Low Fat'). Standardizing these was crucial for accurate reporting.

SQL
UPDATE blinkit_data
SET Item_Fat_Content = 
    CASE 
        WHEN Item_Fat_Content IN ('LF', 'low fat') THEN 'Low Fat'
        WHEN Item_Fat_Content = 'reg' THEN 'Regular'
        ELSE Item_Fat_Content
    END;
2. Business Insights Extracted
I developed several queries to answer specific business questions:

Sales by Fat Content: Understanding if customers prefer Low Fat vs. Regular items.

Outlet Performance by Size: Analyzing which outlet sizes (Small, Medium, High) contribute the most to the bottom line using Percentage of Sales window functions.

Location Analysis: Comparing sales across Tier 1, Tier 2, and Tier 3 cities.

Pivot Analysis: A detailed matrix view of Fat Content performance across different outlet locations.

🚀 How to Use
Clone the Repository:

Bash
git clone https://github.com/your-username/blinkit-sql-analysis.git
Import the Data: Use the provided .csv file (if applicable) and import it into your SQL Server instance as blinkit_data.

Run the Scripts: Execute the analysis_queries.sql file to see the results of the cleaning and KPI generation.

📈 Key Findings
Standardization Impact: Cleaning the Item_Fat_Content reduced categorical noise by 40%, leading to more accurate group-by analysis.

Tier Performance: Tier 3 locations showed the highest volume of orders, while Tier 1 locations had the highest average order value.

Outlet Size: Medium-sized outlets contribute the largest percentage to total sales.
