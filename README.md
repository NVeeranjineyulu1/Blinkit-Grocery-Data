# Blinkit-Grocery-Data
Power BI dashboard analyzing Blinkit sales performance across outlets, locations, and product categories. Includes KPIs, sales trends, item analysis, and interactive filters to derive business insights and support data-driven decisions.

-- 1. DATABASE SETUP
-- Create the table structure to hold Blinkit data
CREATE TABLE blinkit_data (
    Item_Fat_Content VARCHAR(50),
    Item_Identifier VARCHAR(50),
    Item_Type VARCHAR(100),
    Outlet_Establishment_Year INT,
    Outlet_Identifier VARCHAR(50),
    Outlet_Location_Type VARCHAR(50),
    Outlet_Size VARCHAR(50),
    Outlet_Type VARCHAR(100),
    Item_Visibility DECIMAL(10,4),
    Item_Weight DECIMAL(10,2),
    Total_Sales DECIMAL(18,2),
    Rating DECIMAL(3,1)
);

-- 2. DATA CLEANING
-- Standardizing 'Item_Fat_Content' to ensure consistency
UPDATE blinkit_data
SET Item_Fat_Content = 
    CASE 
        WHEN Item_Fat_Content IN ('LF', 'low fat') THEN 'Low Fat'
        WHEN Item_Fat_Content = 'reg' THEN 'Regular'
        ELSE Item_Fat_Content
    END;

-- Verification Check
SELECT DISTINCT Item_Fat_Content FROM blinkit_data;

---

-- 3. KEY PERFORMANCE INDICATORS (KPIs)

-- Total Sales (in Millions)
SELECT CAST(SUM(Total_Sales) / 1000000.0 AS DECIMAL(10,2)) AS Total_Sales_Million
FROM blinkit_data;

-- Average Sales per Item
SELECT CAST(AVG(Total_Sales) AS INT) AS Avg_Sales
FROM blinkit_data;

-- Total Number of Items/Orders
SELECT COUNT(*) AS No_of_Orders
FROM blinkit_data;

-- Average Customer Rating
SELECT CAST(AVG(Rating) AS DECIMAL(10,1)) AS Avg_Rating
FROM blinkit_data;

---

-- 4. BUSINESS INSIGHTS & AGGREGATIONS

-- A. Sales Distribution by Fat Content
SELECT Item_Fat_Content, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Item_Fat_Content;

-- B. Top Selling Item Types
SELECT Item_Type, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Item_Type
ORDER BY Total_Sales DESC;

-- C. Fat Content Analysis by Outlet Location (Pivoted)
SELECT 
    Outlet_Location_Type, 
    ISNULL([Low Fat], 0) AS Low_Fat, 
    ISNULL([Regular], 0) AS Regular
FROM (
    SELECT Outlet_Location_Type, Item_Fat_Content, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
    FROM blinkit_data
    GROUP BY Outlet_Location_Type, Item_Fat_Content
) AS SourceTable
PIVOT (
    SUM(Total_Sales) 
    FOR Item_Fat_Content IN ([Low Fat], [Regular])
) AS PivotTable
ORDER BY Outlet_Location_Type;

-- D. Sales Growth by Outlet Establishment Year
SELECT Outlet_Establishment_Year, CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales
FROM blinkit_data
GROUP BY Outlet_Establishment_Year
ORDER BY Outlet_Establishment_Year;

-- E. Percentage Contribution of Sales by Outlet Size
SELECT 
    Outlet_Size, 
    CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales,
    CAST((SUM(Total_Sales) * 100.0 / SUM(SUM(Total_Sales)) OVER()) AS DECIMAL(10,2)) AS Sales_Percentage
FROM blinkit_data
GROUP BY Outlet_Size
ORDER BY Total_Sales DESC;

-- F. Comprehensive Metrics by Outlet Type
SELECT 
    Outlet_Type, 
    CAST(SUM(Total_Sales) AS DECIMAL(10,2)) AS Total_Sales,
    CAST(AVG(Total_Sales) AS DECIMAL(10,0)) AS Avg_Sales,
    COUNT(*) AS No_Of_Items,
    CAST(AVG(Rating) AS DECIMAL(10,2)) AS Avg_Rating,
    CAST(AVG(Item_Visibility) AS DECIMAL(10,2)) AS Item_Visibility
FROM blinkit_data
GROUP BY Outlet_Type
ORDER BY Total_Sales DESC;
