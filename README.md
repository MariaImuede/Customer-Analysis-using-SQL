
# Profit Maximization and Customer Analysis - Using SQL 
 

![customers](https://github.com/MariaImuede/Power-Bi/assets/159175444/29319929-a4d4-4ef6-a62a-d506e91115bf)
59175444/fe762015-3720-474e-8314-04869c815b99)


## Problem Statement

An analysis of a Microsoft SQL database is conducted to extract meaningful insights related to sales transactions, customer loyalty, employee records, and product information of the fruit company. The objective is to derive actionable insights and recommendations to improve decision-making processes and address specific business inquiries.
Data set provided by https://www.linkedin.com/company/techchak/



### Steps followed 

- Step 1 : Exploration of Database: the structure of the database is explored to understand its organization and identify potential data quality issues.

- Step 2 : Data cleaning tasks are performed to address any identified issues, including handling data types and ensuring consistency in formats.

- Step 3 : Queries are designed to extract relevant information from different tables within the database, including products, transaction reports, loyalty customers, and employees.


- Step 4 : Queries are executed to answer specific questions related to sales volume, product performance, top customers, employee records, and sales trends.
           
# see queries below

--1. What is Angie’s Berry Corner’s average daily sales volume?
-- the amount is varchar - what to do
SELECT FLOOR(AVG(Convert(FLOAT,Amount))) AS 'Average_Amount' FROM Transaction_Reports

![angie - averge 1](https://github.com/MariaImuede/Power-Bi/assets/159175444/b63e2635-2e77-47c3-98c7-e094049c90c8)

-- 2. Which products sell best?
SELECT TOP 1 Name,FLOOR(SUM(Convert(FLOAT,Amount))) AS SumSales FROM products, Transaction_Reports
WHERE products.id = Transaction_Reports.product_id
GROUP BY name
ORDER BY SumSales DESC

![angie - 2](https://github.com/MariaImuede/Power-Bi/assets/159175444/72fe6722-2b17-4bdb-8ba3-8ee3e2eba3e5)

-- 3. Top 5 Angie loyalty customers
SELECT TOP(5) first_name,last_name,FLOOR(SUM(Convert(FLOAT,Amount))) AS SumSales 
FROM loyalty_customers, Transaction_Reports
WHERE loyalty_customers.id = Transaction_Reports.customer_id
GROUP BY first_name,last_name

![angie - 3](https://github.com/MariaImuede/Power-Bi/assets/159175444/30d584ef-b0f6-4516-a1f6-6b24e68c4cd5)

--4. What is the full name of their current staff?
SELECT first_name,last_name, start_date
FROM employees WHERE start_date = (SELECT MAX(start_date) FROM employees)

![angie - 4](https://github.com/MariaImuede/Power-Bi/assets/159175444/8e3225de-6565-41ec-a5a0-2ad31abbab47)


--5. Which product generates the least income and how much is it?
SELECT TOP 1 Name,FLOOR(MIN(Convert(FLOAT,Amount))) AS MinimumSale
FROM products, Transaction_Reports
WHERE products.id = Transaction_Reports.product_id
GROUP BY name
ORDER BY MinimumSale ASC

![angie - 5](https://github.com/MariaImuede/Power-Bi/assets/159175444/7a22a58b-3e0c-494a-997a-289799049f4e)


--6. The organization wants to ascertain the income realized from sales
SELECT FLOOR(SUM(Convert(FLOAT,Amount))) AS TotalSales 
FROM Transaction_Reports

![angie - 6](https://github.com/MariaImuede/Power-Bi/assets/159175444/6092eabf-2dfd-408c-8683-a9998f8065d4)

--7. The product that generates the highest income and how much the organization is looking at identifying the customer that patronizes them the most in order for them to encourage them with a gift
SELECT TOP 1 T.product_id, FLOOR(MAX(Convert(FLOAT,Amount))) AS Highest_income, P.name
FROM Transaction_Reports AS T
JOIN products AS P
ON T.product_id = P.id
GROUP BY T.product_id, P.name
ORDER BY  Highest_income DESC

![angie - 7](https://github.com/MariaImuede/Power-Bi/assets/159175444/0cec866e-7d8d-4195-9341-c0733e8ee224)

--8. identifying the customer that patronizes them the most in order for them to encourage them with a gift
SELECT DISTINCT T.customer_id, FLOOR(MAX(Convert(FLOAT,Amount))) AS Highest_income, L.first_name, L.last_name
FROM Transaction_Reports AS T
JOIN loyalty_customers AS L
ON T.customer_id = L.id
GROUP BY T.customer_id, L.first_name, L.last_name 
ORDER BY  Highest_income DESC

![angie - 8is8](https://github.com/MariaImuede/Power-Bi/assets/159175444/458a1b5b-2554-439b-b391-3e9302128ec2)

--9. Which customer generates the least income and by how much?
SELECT TOP 1 T.customer_id, FLOOR(MIN(Convert(FLOAT,Amount))) AS least_income, L.first_name, L.last_name
FROM Transaction_Reports AS T 
JOIN loyalty_customers AS L
ON T.customer_id = L.id
GROUP BY T.customer_id, L.first_name, L.last_name 
ORDER BY  least_income ASC

![angie - 9](https://github.com/MariaImuede/Power-Bi/assets/159175444/14543de6-5394-4921-88f2-17b03c1d3756)

--10. What is the organization's busiest hour?
SELECT TOP 1 paid_at AS busiest_hour, COUNT(*) AS event_count
FROM Transaction_Reports
GROUP BY paid_at 
ORDER BY event_count DESC 

![angie - 10](https://github.com/MariaImuede/Power-Bi/assets/159175444/40903a9c-75e4-4a46-acdc-b578c3bc7f73)

-- 11. Which day of the week does the organization sell the most
SELECT TOP 1 paid_at AS day_of_the_week, FLOOR(SUM(Convert(FLOAT,Amount))) AS TotalSales
FROM Transaction_Reports
GROUP BY paid_at 
ORDER BY TotalSales DESC 

![angie - 11](https://github.com/MariaImuede/Power-Bi/assets/159175444/c9ce2da1-b934-40b6-a9e3-bf5822b8b67f)

--12. Which month of the year does the organization make the most sales
SELECT paid_at AS month_of_the_year, FLOOR(SUM(Convert(FLOAT,Amount))) AS TotalSales
FROM Transaction_Reports
GROUP BY paid_at 
ORDER BY TotalSales DESC

![angie - 12](https://github.com/MariaImuede/Power-Bi/assets/159175444/36a7cdd8-e213-4b17-8a23-434e605c2296)


# Insights

1. Angie's Berry Corner's average daily sales volume is calculated by averaging the sales amounts from transaction reports. The average amount is calculated using the AVG function after converting the Amount column to a numeric data type.
2. The best-selling products are identified by summing the sales amounts for each product and sorting them in descending order to find the top-performing product.
3. The top five loyalty customers are determined based on their total purchase amounts, highlighting the most valuable customers.
4. The full name of the current staff is retrieved from the employees table by selecting records with the maximum start date.
5. The product generating the least income and its corresponding sales amount are identified by aggregating sales data from transaction reports and products.
6. The total income realized from sales transactions is calculated by summing the sales amounts from transaction reports.
7. The income generated from each product is ascertained by joining transaction reports and products tables to analyze sales data for each product.
8. The product generating the highest income and the corresponding sales amount are determined, along with identifying the customer who patronizes this product the most.
9. The customer generating the least income and the corresponding sales amount are identified, highlighting customers with low spending.
10. The employee who spent the last day at Angie's is determined based on the end date recorded in the employees table.
11 The busiest hour, busiest day of the week, and busiest month of the year for sales transactions are identified based on the frequency and total sales amounts recorded in transaction reports.

# Recommendations:

1. Implement measures to improve data consistency and accuracy, particularly in recording sales amounts and employee records.
2. Identify and prioritize top-performing products to optimize marketing strategies and inventory management.
3. Develop targeted loyalty programs or incentives to retain and reward top customers identified through transaction analysis.
4. Monitor employee turnover rates and implement strategies to improve employee retention and satisfaction.
5. Analyze sales trends by hour, day, and month to optimize staffing schedules and promotional activities.
6. Consider expanding product offerings or adjusting pricing strategies based on insights into sales performance.

