1 -- Question: Who are our most active customers by order count?

SELECT CustomerID, COUNT(OrderID) AS total\_orders

FROM Orders

GROUP BY CustomerID

ORDER BY total\_orders DESC

LIMIT 10



**Business Finding: Our most active customer is BSBEV with 210 orders followed by RICAR with 203, LILAS with 203, GOURL with 202, PRINI with 200, HUNGC with 198, TORTU with 197, FOLIG with 195, ANATR with 195 and RANCH with 194. -- wouldn't it be better to give actual customer names rather than customer ID?**



2 -- Question: Who are our highest spending customers?

WITH customer\_spending AS (

&#x20; SELECT o.CustomerID, SUM(od.UnitPrice \* od.Quantity) AS total\_spent

&#x20; FROM Orders o

&#x20; JOIN "Order Details" od ON o.OrderID = od.OrderID

&#x20; GROUP BY o.CustomerID

)

SELECT c.ContactName, cs.total\_spent

FROM Customers c

JOIN customer\_spending cs ON c.CustomerID = cs.CustomerID

ORDER BY cs.total\_spent DESC

LIMIT 10;



**Business Finding: Our highest spending customer is Victoria Ashworth with a total of 6,154,115.34, followed by Yoshi Latimer with 5,698,023.67, followed by Sergio Gutiérrez with 5,559,110.08, André Fonseca	5552597.9, Ana Trujillo	5534356.65, Janete Limeira 5524990.91, Martine Rancé 5505502.85, Jaime Yorres 5462611.57, Carlos González 5439186.8, Isabel de Castro 5437042.71.**



3 -- Question: Which products generate the most revenue?

WITH product\_math AS (

&#x20; SELECT ProductID, SUM(UnitPrice \* Quantity) AS revenue

&#x20; FROM \[Order Details]

&#x20; GROUP BY ProductID

)

SELECT 

&#x20; p.ProductName, 

&#x20; pm.revenue

FROM Products p

JOIN product\_math pm ON p.ProductID = pm.ProductID

ORDER BY pm.revenue DESC

LIMIT 10;



**Business Finding: Côte de Blaye generates the most revenue with 53,274,482.7, followed by Thüringer Rostbratwurst with 24630836.96, Mishi Kobe Niku with 19424638, Sir Rodney's Marmalade with 16654879.8, Carnarvon Tigers with 12607487.5, Raclette Courdavault with 11221551, Manjimup Dried Apples with 10667691.6, Tarte au sucre with 9955529, Ipoh Coffee with 9334927.2, Rössle Sauerkraut with 9253934.4**



4 -- Which customers placed more than one order?

SELECT c.CompanyName, COUNT(o.OrderID) AS order\_count

FROM Customers c

JOIN Orders o ON c.CustomerID = o.CustomerID

GROUP BY c.CompanyName

HAVING COUNT(o.OrderID) > 1

ORDER BY order\_count DESC



**Business Finding: IT placed 335, B's Beverages placed 210, Ricardo Adocicadoes with 203. The lowest would be Océano Atlántico Ltda. with 154.**



5 -- What is the average order value per country?

SELECT c.Country,

&#x20; ROUND(AVG(od.Quantity \* od.UnitPrice), 2) AS avg\_order\_value

FROM Customers c

JOIN Orders o ON c.CustomerID = o.CustomerID

JOIN "Order Details" od ON o.OrderID = od.OrderID

GROUP BY c.Country

ORDER BY avg\_order\_value DESC



**Business Finding: The average order value for Belgium is 748.3, Argentina with 742.84, Venezuela with 740.43, Switzerland with 739.08, Germany with 738.21, Sweden with 737.94, Austria with 737.58, the UK with 737.4, Finland with 737.05, Brazil with 736.91, France with 736.3, Ireland with 735.74, Spain with 735.27, Italy with 735.03, Poland with 734.87, Mexico with 734.18, USA with 732.58, Norway with 731.98, Canada with 731.86, Portugal with 730.7, and Denmark with 725.26.**



6 -- Which employee processed the most orders?

SELECT e.FirstName || ' ' || e.LastName AS employee\_name,

&#x20; COUNT(o.OrderID) AS orders\_handled

FROM Employees e

JOIN Orders o ON e.EmployeeID = o.EmployeeID

GROUP BY employee\_name

ORDER BY orders\_handled DESC

LIMIT 1



**Business Finding: Margaret Peacock processed the most orders with the amount of 1908.**























