1 -- Question: Who are our most active customers by order count?

SELECT CustomerID, COUNT(OrderID) AS total_orders
FROM Orders
GROUP BY CustomerID
ORDER BY total_orders DESC
LIMIT 10;

**Business Finding: Our most active customer is B'Beverages with 210 orders, followed closely by Ricardo Adocicados and Laughing Bacchus Wine Cellars (203 each). High order frequency from these top 10 clients suggets strong brand loyalty and consistent demand.**


2 -- Question: Who are our highest spending customers?

WITH customer_spending AS (
 SELECT o.CustomerID, SUM(od.UnitPrice * od.Quantity) AS total_spent
      FROM Orders o
      JOIN "Order Details" od ON o.OrderID = od.OrderID
      GROUP BY o.CustomerID
)
SELECT c.ContactName, cs.total_spent
FROM Customers c
JOIN customer_spending cs ON c.CustomerID = cs.CustomerID
ORDER BY cs.total_spent DESC
LIMIT 10;

**Business Finding: Victoria Ashworth is our top spender ($6.15M), leading the next highest customer, Yoshi Latimer, by nearly $450K. The top 10 customers all exceed $5.4M, representing our most critical "High-Value" segment.**


3 -- Question: Which products generate the most revenue?

WITH product_math AS (
  SELECT ProductID, SUM(UnitPrice * Quantity) AS revenue
  FROM [Order Details]
  GROUP BY ProductID
)
SELECT 
  p.ProductName, 
  pm.revenue
FROM Products p
JOIN product_math pm ON p.ProductID = pm.ProductID
ORDER BY pm.revenue DESC
LIMIT 10;


**Business Finding: Côte de Blaye is the product that generates the most revenue of over $53M, which is more than double the revenue of the second-place product, Thüringer Rostbratwurst ($24.6M).**


4 -- Which customers placed more than one order?

SELECT c.CompanyName, COUNT(o.OrderID) AS order_count
FROM Customers c
JOIN Orders o ON c.CustomerID = o.CustomerID
GROUP BY c.CompanyName
HAVING COUNT(o.OrderID) > 1
ORDER BY order_count DESC;


**Business Finding: Almost all customers are repeat buyers. B's Beverages leads with 210 orders and our lowest customer Océano Atlántico Ltda has placed 154 orders. This shows that there is a very high retention rate across the board.**


5 -- What is the average order value per country?

SELECT c.Country,
  ROUND(AVG(od.Quantity * od.UnitPrice), 2) AS avg_order_value
FROM Customers c
JOIN Orders o ON c.CustomerID = o.CustomerID
JOIN "Order Details" od ON o.OrderID = od.OrderID
GROUP BY c.Country
ORDER BY avg_order_value DESC


**Business Finding: The AOV is exceptionally consistent globally, staying within the $725-748 range. Belgium leads with the highest average at $748.30, while Denmark sits at the bottom with $725.26. This stability suggests that our pricing strategy is well-balanced across different international markets.**


6 -- Which employee processed the most orders?

SELECT e.FirstName || ' ' || e.LastName AS employee_name,
   COUNT(o.OrderID) AS orders_handled
FROM Employees e
JOIN Orders o ON e.EmployeeID = o.EmployeeID
GROUP BY employee_name
ORDER BY orders_handled DESC
LIMIT 1;

**Business Finding: Margaret Peacock is our most productive team member, successfully processing 1,908 orders. Her volume suggests that she is a key driver for fulfillment and handles the highest workload within the sales team.**























