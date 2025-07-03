Pizza Sales Analysis
This repository contains SQL queries used to analyze pizza sales data. The analysis covers key performance indicators (KPIs), daily and monthly sales trends, sales distribution by category and size, and identification of top and bottom-performing pizzas.

Table of Contents
Key Performance Indicators (KPIs)

Daily and Monthly Trends

Sales Analysis by Category and Size

Top/Bottom Performing Pizzas

Filtering Examples

Key Performance Indicators (KPIs)
The following SQL queries calculate essential KPIs for pizza sales:

1. Total Revenue
Calculates the sum of all pizza sales.

SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales;

Result: 817860.05

2. Average Order Value
Determines the average value of each order.

SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value FROM pizza_sales;

Result: 38.30

3. Total Pizzas Sold
Counts the total number of pizzas sold.

SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales;

Result: 49574

4. Total Orders
Calculates the total number of unique orders.

SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales;

Result: 21350

5. Average Pizzas Per Order
Calculates the average number of pizzas in each order.

SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / 
CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2))
AS Avg_Pizzas_per_order
FROM pizza_sales;

Result: 2.32

Daily and Monthly Trends
These queries help in understanding sales patterns over time.

B. Daily Trend for Total Orders
Analyzes the total number of orders per day of the week.

SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales
GROUP BY DATENAME(DW, order_date)
ORDER BY 
    CASE DATENAME(DW, order_date)
        WHEN 'Monday' THEN 1
        WHEN 'Tuesday' THEN 2
        WHEN 'Wednesday' THEN 3
        WHEN 'Thursday' THEN 4
        WHEN 'Friday' THEN 5
        WHEN 'Saturday' THEN 6
        WHEN 'Sunday' THEN 7
    END;

Results:
| order_day | total_orders |
|-----------|--------------|
| Saturday  | 3158         |
| Wednesday | 3024         |
| Monday    | 2794         |
| Sunday    | 2624         |
| Friday    | 3538         |
| Thursday  | 3239         |
| Tuesday   | 2973         |

C. Monthly Trend for Orders
Analyzes the total number of orders per month.

SELECT DATENAME(MONTH, order_date) AS Month_Name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY DATENAME(MONTH, order_date)
ORDER BY MIN(MONTH(order_date));

Results:
| Month_Name | Total_Orders |
|------------|--------------|
| February   | 1685         |
| June       | 1773         |
| August     | 1841         |
| April      | 1799         |
| May        | 1853         |
| December   | 1680         |
| January    | 1845         |
| September  | 1661         |
| October    | 1646         |
| July       | 1935         |
| November   | 1792         |
| March      | 1840         |

Sales Analysis by Category and Size
These queries provide insights into sales distribution across different pizza categories and sizes.

D. % of Sales by Pizza Category
Calculates the percentage of total sales for each pizza category.

SELECT pizza_category, 
       CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_revenue,
       CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_category
ORDER BY PCT DESC;

Results:
| pizza_category | total_revenue | PCT   |
|----------------|---------------|-------|
| Classic        | 220053.10     | 26.91 |
| Chicken        | 195919.50     | 23.96 |
| Veggie         | 193690.45     | 23.68 |
| Supreme        | 208197.00     | 25.46 |

E. % of Sales by Pizza Size
Calculates the percentage of total sales for each pizza size.

SELECT pizza_size, 
       CAST(SUM(total_price) AS DECIMAL(10,2)) AS total_revenue,
       CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_size
ORDER BY 
    CASE pizza_size
        WHEN 'S' THEN 1
        WHEN 'M' THEN 2
        WHEN 'L' THEN 3
        WHEN 'XL' THEN 4
        WHEN 'XXL' THEN 5
    END;

Results:
| pizza_size | total_revenue | PCT   |
|------------|---------------|-------|
| L          | 375318.70     | 45.89 |
| M          | 249382.25     | 30.49 |
| S          | 178076.50     | 21.77 |
| XL         | 14076.00      | 1.72  |
| XXL        | 1006.60       | 0.12  |

F. Total Pizzas Sold by Pizza Category (for February)
Calculates the total quantity of pizzas sold per category, specifically for February.

SELECT pizza_category, SUM(quantity) AS Total_Quantity_Sold
FROM pizza_sales
WHERE MONTH(order_date) = 2
GROUP BY pizza_category
ORDER BY Total_Quantity_Sold DESC;

Results:
| pizza_category | Total_Quantity_Sold |
|----------------|---------------------|
| Classic        | 14888               |
| Supreme        | 11987               |
| Veggie         | 11649               |
| Chicken        | 11050               |

Top/Bottom Performing Pizzas
These queries identify the best and worst-performing pizzas based on revenue, quantity, and total orders.

G. Top 5 Pizzas by Revenue
Lists the top 5 pizzas generating the most revenue.

SELECT TOP 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue DESC;

Results:
| pizza_name              | Total_Revenue |
|-------------------------|---------------|
| The Thai Chicken Pizza  | 43434.25      |
| The Barbecue Chicken Pizza | 42768.00      |
| The California Chicken Pizza | 41409.50      |
| The Classic Deluxe Pizza | 38180.50      |
| The Spicy Italian Pizza | 34831.25      |

H. Bottom 5 Pizzas by Revenue
Lists the bottom 5 pizzas generating the least revenue.

SELECT TOP 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue ASC;

Results:
| pizza_name              | Total_Revenue |
|-------------------------|---------------|
| The Brie Carre Pizza    | 11588.50      |
| The Green Garden Pizza  | 13955.75      |
| The Spinach Supreme Pizza | 15277.75      |
| The Mediterranean Pizza | 15360.50      |
| The Spinach Pesto Pizza | 15596.00      |

I. Top 5 Pizzas by Quantity
Lists the top 5 pizzas sold in the highest quantity.

SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold DESC;

Results:
| pizza_name              | Total_Pizza_Sold |
|-------------------------|------------------|
| The Classic Deluxe Pizza | 2453             |
| The Barbecue Chicken Pizza | 2432             |
| The Hawaiian Pizza      | 2422             |
| The Pepperoni Pizza     | 2418             |
| The Thai Chicken Pizza  | 2371             |

J. Bottom 5 Pizzas by Quantity
Lists the bottom 5 pizzas sold in the lowest quantity.

SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold ASC;

Results:
| pizza_name              | Total_Pizza_Sold |
|-------------------------|------------------|
| The Brie Carre Pizza    | 490              |
| The Mediterranean Pizza | 934              |
| The Calabrese Pizza     | 937              |
| The Spinach Supreme Pizza | 950              |
| The Soppressata Pizza   | 961              |

K. Top 5 Pizzas by Total Orders
Lists the top 5 pizzas that appeared in the most orders.

SELECT TOP 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders DESC;

Results:
| pizza_name              | Total_Orders |
|-------------------------|--------------|
| The Classic Deluxe Pizza | 2329         |
| The Hawaiian Pizza      | 2280         |
| The Pepperoni Pizza     | 2278         |
| The Barbecue Chicken Pizza | 2273         |
| The Thai Chicken Pizza  | 2225         |

L. Bottom 5 Pizzas by Total Orders
Lists the bottom 5 pizzas that appeared in the fewest orders.

SELECT TOP 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders ASC;

Results:
| pizza_name              | Total_Orders |
|-------------------------|--------------|
| The Brie Carre Pizza    | 480          |
| The Mediterranean Pizza | 912          |
| The Spinach Supreme Pizza | 918          |
| The Calabrese Pizza     | 918          |
| The Chicken Pesto Pizza | 938          |

Filtering Examples
You can apply filters to the above queries using a WHERE clause. Here are some examples:

Filtering by Pizza Category
To get the bottom 5 pizzas by total orders specifically for the 'Classic' category:

SELECT TOP 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
WHERE pizza_category = 'Classic'
GROUP BY pizza_name
ORDER BY Total_Orders ASC;

Filtering by Pizza Size
You can also filter by pizza_size similarly.

Feel free to explore and adapt these queries for further analysis!
