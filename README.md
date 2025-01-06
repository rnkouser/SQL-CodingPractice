## SQL Coding Practice
---

```sql
-- Syntax to retrieve selected columns from a table

SELECT EmployeeID, EmployeeNo, FirstName, LastName, MaritalStatus
FROM Employee;


-- Syntax to retrieve all the columns from a single table

SELECT * FROM Employee;

-- Syntax to retrieve columns from a single table with a condition using the WHERE clause

SELECT EmployeeNo, FirstName
FROM Employee
WHERE EmployeeID = 10;

-- Syntax to retrieve all columns from a single table with a condition using the WHERE clause

SELECT *
FROM Employee
WHERE MaritalStatus = 3;


-- Syntax to retrieve all columns from a single table with condition using WHERE clause with LIKE
-- Where and LIKE retrieve specific patterns in a column. Always used with % sign.

SELECT FirstName, LastName
FROM Employee
WHERE FirstName LIKE 'a%' AND LastName LIKE 'a%';

-- Syntax to retrieve selected columns with condition using WHERE clause and order by specific columns in Ascending (ASC) or Descending (DESC) order.
-- Default order is ASC.
-- Order By Syntax = used for sorting the result set by one or more columns, used with (ASC)/DESC

/*
SELECT column1, column2 FROM Table_name
WHERE Column1 = 'Value'  
ORDER BY column2, column3, Column4 ASC;
*/

SELECT *
FROM [dbo].[SalesTransaction]
WHERE [ChannelID] = 10
ORDER BY [CustomerID] DESC;

-- Syntax to retrieve DISTINCT values on selected columns with condition using WHERE clause

-- DISTINCT = used for removing duplicate rows from the result set of a query, ensuring each row is unique.
/*
SELECT DISTINCT column1, column2 FROM Table_name
WHERE Column1 = 'Value';
*/

SELECT DISTINCT [EmployeeID], [EmployeeNo], [FirstName], [LastName], [DoB], [MaritalStatus]
FROM [dbo].[Employee];
-- WHERE DoB LIKE '1986%';

-- Important interview question
-- 1. From 
-- 2. Join
-- 3. Where 
-- 4. Group By
-- 5. Having
-- 6. Select
-- 7. Order by
-- 8. Limit e.g. Top 

-- Fine Juice With Great Health Surely Optimizes Life

-- Practice No 2: Retrieve dataset of AccountNumber, suppliers from Canada

SELECT AccountNumber, Supplier
FROM [dbo].[PurchaseTrans]
WHERE Country = 'Canada';

-- Practice No 3: Retrieve dataset of DISTINCT AccountNumber, suppliers from Canada

SELECT DISTINCT AccountNumber, Supplier
FROM [dbo].[PurchaseTrans];

-- Logical Operators (AND, OR, ALL, ANY, BETWEEN, EXISTS, IN, LIKE, NOT)

-- Practice No 4: Retrieve all dataset of unit price more than 100 for Burnaby City in Canada

SELECT *
FROM [dbo].[PurchaseTrans]
WHERE Country = 'Canada' AND City = 'Burnaby' AND UnitPrice > 100;

-- Practice No 5: Retrieve all dataset of Canada where CarrierTrackingNumber starts from 4B, derived using WildCard

SELECT *
FROM [dbo].[PurchaseTrans]
WHERE Country = 'Canada' AND CarrierTrackingNumber LIKE '4B%';

--Practice No 6 /* Derive Special Offer description based on Special Offer ID using CASE statement expression */

Select OrderID, SpecialOfferID,ProductID,OrderQty,
	CASE SpecialOfferID
		WHEN 1 THEN 'BlackFriday'
		WHEN 2 THEN 'Halloween Promotion'
		WHEN 3 THEN 'Winter Offer'
		WHEN 4 THEN 'ack to School'
		WHEN 6 THEN 'Liberty Anniversary'
		WHEN 7 THEN 'Summer Sales'
		WHEN 9 THEN 'Canada 150th years'
		WHEN 13 THEN 'Civil Holiday'
		WHEN 14 THEN 'Good Friday'
		ELSE 'Unknown'
	END as SpecialOffer
From [dbo].[PurchaseTrans]

--=========================================
--  Comparison Operators ( >,<,= )
--=========================================

--Practice No 7 /* Derive Purchase Key Performance Indicator using CASE statement expression on UnitPrice and Order Quantity*/

Select TransID,OrderID, UnitPrice*OrderQty as Amount,
	Case 
		WHEN UnitPrice*OrderQty < 500 THEN 'Low'    -- (1 - 499)
		WHEN UnitPrice*OrderQty >500 AND UnitPrice*OrderQty <=1000   THEN 'Medium'  --(500-999)
		Else 'High' --(1000 and above)

	End as KPI
	
From [dbo].[PurchaseTrans]


--Practice No 8 /* Derive Purchase Key Performance Indicator using CASE statement expression on UnitPrice and Order Quantity based on Country*/

Select TransID,OrderID,Supplier,StateProvince,Country,UnitPrice*OrderQty as Amount,
	Case Country
		When 'Canada' THEN
			CASE
			 When UnitPrice*OrderQty < 500 Then 'Low'
			 WHEN UnitPrice*OrderQty >500 AND UnitPrice*OrderQty <=1000   THEN 'Medium'
			 Else 'High'
		END 
		
		When 'Australia' THEN
			CASE
			 When UnitPrice*OrderQty < 500 Then 'Low'
			 WHEN UnitPrice*OrderQty >500 AND UnitPrice*OrderQty <=1000   THEN 'Medium'
			 Else 'High'
		END 
	
		When 'France' THEN
			CASE
			 When UnitPrice*OrderQty < 500 Then 'Low'
			 WHEN UnitPrice*OrderQty >500 AND UnitPrice*OrderQty <=1000   THEN 'Medium'
			 Else 'High'
		END 

	END as KPI
From PurchaseTrans



Select OrderID,ProductID,UnitPrice,OrderQty,UnitPrice*OrderQty as TotalAmount,
Case SpecialOfferID
	When 1 then 'BlackFriday'
	When 2 then 'Halloween'
	When 3 then 'Winter Break'
	When 4 then 'Back to School'
	When 5 then 'Summer Break'
	Else 'Undefined'
End  SpecialOffer, SpecialOfferID, UnitPriceDiscount,
Case
	When OrderQty<5 then 'Low Order'
	When OrderQty>=5 and OrderQty<=6 then 'Medium Order'
	Else 'High Order'
End OrderLevel,
UnitPrice*OrderQty*(1-UnitPriceDiscount) as TotalDiscountAmount
From PurchaseTrans


--=====================================================================================
---- Aggregate Functions (Count,MAX,MIN,AVG,SUM). They are always used with Group By
--======================================================================================

--Practice No 10 /* Count total number of records in the PurchaseTrans dataset*/

Select Count(*) as TotalRecords
From [dbo].[PurchaseTrans]


--Practice No 11 /* Count total number of employee records in the PurchaseTrans dataset*/
Select Count(Employee) as TotalEmployees From [dbo].[PurchaseTrans]


--Practice No 12  -- (Distince=Unique) /* Count total number of UNIQUE employee in the PurchaseTrans dataset*/
Select Count ( Distinct Employee )  as TotalUniueEmployees From [dbo].[PurchaseTrans]



--Practice No 13 /* Return the average of the total order quantity of the PurchaseTrans dataset*/

Select Country, Round (AVG(OrderQty),3) as AverageOrderQuantity From [dbo].[PurchaseTrans]
Group by Country 
Order By AverageOrderQuantity DESC


--Practice No 14 /* Return the average of the unique order quantity value of the PurchaseTrans dataset*/

Select AVG(Distinct OrderQty) as AverageofUniqueOrderQty From [dbo].[PurchaseTrans]


Select Round(AVG(Distinct OrderQty),4) as AverageofUniqueOrderQty From [dbo].[PurchaseTrans]


--Practice No 15 /* Return the Maximum of all order quantity value of the PurchaseTrans dataset*/
Select Country,  MAX(OrderQty) as MaxOrderQty From [dbo].[PurchaseTrans]
Group by Country
Order by MaxOrderQty


--Practice No 16 /* Return the maximum of the unique order quantity value of the PurchaseTrans dataset*/

Select MAX( Distinct OrderQty)  as MaxUniqueOrderQty From [dbo].[PurchaseTrans]


--Practice No 17 /* Return the minimum of all order quantity value of the PurchaseTrans dataset*/

Select MIN(OrderQty)  as MinOrderQty From [dbo].[PurchaseTrans]


--Practice No 18 /* Return the minimum of the unique order quantity value of the PurchaseTrans dataset*/

Select Min( Distinct OrderQty)  as MinUniqueOrderQty From [dbo].[PurchaseTrans]


--Practice No 19 /* Return the sum of all order quantity value of the PurchaseTrans dataset*/

Select Sum(OrderQty) as SumOrderQty From [dbo].[PurchaseTrans]


--Practice No 20 /* Return the sum of the unique order quantity value of the PurchaseTrans dataset*/

Select Country, Sum(Distinct OrderQty) as SumofuniqueOrderQty From [dbo].[PurchaseTrans]
Group By Country
Order by sumofuniqueOrderQty 


--Practice No 21 /* Return the SUM of all order quantity value GROUP BY Country in the PurchaseTrans dataset*/

Select Country, Sum(OrderQty) as SumOrderQty From [dbo].[PurchaseTrans]
Group By Country


--Practice No 22 /* Return the SUM of the unique order quantity value GROUP BY Country in the PurchaseTrans dataset*/

Select Country, Sum(Distinct OrderQty) as SumOrderQty From [dbo].[PurchaseTrans]
Group By Country


--Practice No 23 --/* Return the rollup (GrandTotal) of subtotal and total quantity by Country and class of purchaseTrans dataset*/

Select Country, Class,Sum(OrderQty) as TotalQuantity From PurchaseTrans 
Group by Rollup (Country, Class) 
Order By Country


--Practice No 24 /* Return the dataset group by country and class of purchaseTrans dataset*/

/*A GROUPING SET in SQL lets you create several groupings in one query. 
This helps you get summaries and subtotals for different column combinations all at once, 
making it easier to create detailed reports. It's like using multiple GROUP BY commands in a single query*/

Select Country,Class, Sum(OrderQty) as TotalOrderQty  From PurchaseTrans
Group by Grouping sets (Country,Class)
Order by Country


--Practice No 25 /* Return the dataset by country with Total Quantity of purchaseTrans dataset*/

Select Country,Sum(OrderQty) as TotalQuantity  From PurchaseTrans
Group by Grouping sets (Country,())
Order By Country


/* Select Country,Sum(OrderQty) as TotalQuantity  From PurchaseTrans
Group by Grouping sets (Country)
Order By Country */

--Practice No 26 /* Return the Grouping sets, rollup of subtotal and total quantity by Country and class of PurchaseTrans dataset*/

Select Country,Class,Sum(OrderQty) as TotalQty From PurchaseTrans
Group by Grouping sets (Rollup(country,Class)) 
Order By Country


--Practice No 27 /* Return the SUM of DISTINCT order quantity value Having Sum of Distinct Order Quantity greater than 100, 
                           --GROUP BY Country in the PurchaseTrans dataset*/

Select Country, Sum (Distinct OrderQty) as SumofDistinctOrderQty From PurchaseTrans
Group by Country
Having Sum(Distinct OrderQty) > 100


--Practice No 28 /* Return the SUM of DISTINCT order quantity value Having Sum of order quantity greater than 100, 
								--GROUP BY Country in the PurchaseTrans dataset*/

Select Country, Sum (Distinct OrderQty) as SumOfDistinctOrderQty, Sum(OrderQty)as SumofOrderQty From PurchaseTrans 
Group by Country
Having Sum(OrderQty) > 100


--Practice No 29 /* Return the SUM of DISTINCT order quantity value, count of order quantity, Having Sum of Distinct order quantity multiples by
								--unitprice greater than 10, GROUP BY Country in the PurchaseTrans dataset*/

Select Country, Sum(Distinct OrderQty) as SumOfDistinctOrderQty, Count(*) OrderQty From PurchaseTrans
Group By Country
Having Sum(Distinct OrderQty*UnitPrice) > 10 


--Practice 30
/* Return the SUM of DISTINCT order quantity value, count of order quantity and sum of order quantity multiplies by unit price ,
Having Count of Distinct order quantity multiples by UnitPrice greater than 10 AND sum of order quantity multiples by unit price
greater than 2000 GROUP BY Country in the PurchaseTrans dataset*/

Select Country, Sum(Distinct OrderQty) as SumOfDistinctOrderQty, 
Count(*) OrderQty, Sum(OrderQty*UnitPrice) as TotalAmount
From PurchaseTrans 
Group By Country
Having Count (Distinct OrderQty * UnitPrice) > 10 AND Sum (OrderQty*UnitPrice) > 2000


--Practice 31
/* Return the SUM of DISTINCT order quantity value, count of order quantity, Having Sum of Distinct order quantity multiples by
UnitPrice greater than 10, GROUP BY Country in the PurchaseTrans dataset*/

Select Country,Sum (Distinct OrderQty) as SumOfDistinctOrderQty, Count(*) OrderQty From PurchaseTrans
Group by Country
Having Sum(Distinct OrderQty * UnitPrice)  > 10 


--Practice 32:
/* Return the SUM of DISTINCT order quantity value, count of order quantity and sum of order quantity multiplies by unit price ,
Having Count of Distinct order quantity multiples by UnitPrice greater than 10 AND sum of order quantity multiples by unit price
greater than 2000 GROUP BY Country in the PurchaseTrans dataset*/

Select Country, Sum(Distinct OrderQty) as SumOfUniqueOrderQty, Count(*) OrderQty, Sum(OrderQty*UnitPrice) as TotalAmount
From PurchaseTrans
Group by Country
Having Count(Distinct OrderQty * UnitPrice ) > 10 AND
Sum(OrderQty * UnitPrice) > 2000


--Practice 33
/* Return the SUM of DISTINCT order quantity value, count of dataset, Purchase amount( sum of order quantity multiplies by unit
price), Having count of Distinct order quantity multiples by unit price greater than 10 AND sum of order quantity multiples by unit
price greater than 2000 GROUP BY Country and city and Order by Country in the PurchaseTrans dataset*/

Select Country,City, Sum(Distinct OrderQty) as SumOfUniqueOrderQty, Count(*) CountOfOrderQty, 
Sum(OrderQty * UnitPrice) as PurchaseAmount 
From PurchaseTrans
Group by Country, City
Having Count(Distinct OrderQty * UnitPrice) > 10 AND Sum(OrderQty * UnitPrice) > 2000
Order By Country 


--Practice 34
/* Return the SUM of DISTINCT order quantity value, count of dataset, Purchase amount( sum of order quantity multiplies by unit
price), Having count of Distinct order quantity multiples by UnitPrice greater than 10 AND sum of order quantity multiples by unit
price greater than 2000 GROUP BY Country and city and Order by Country in the PurchaseTrans dataset for Country =‘Canada’*/

Select Country, City, Sum (Distinct OrderQty) as SumOfUniqueOrderQty, Count(*) CountOfOrderQty, Sum(OrderQty * UnitPrice) as PurchaseAmount 
From PurchaseTrans
Where Country = 'Canada'
Group By Country, City
Having Count (Distinct OrderQty * UnitPrice) > 10 AND Sum(OrderQty * UnitPrice) > 2000 
Order By Country


---============================================
----	 SubQuery (Uncorrelated , Correlated)
--=============================================

Select * From PurchaseTrans t Where t.ProductID IN (Select p.ProductID From Product p where p.ProductID<10)    --Uncorrelated
Select * From PurchaseTrans t Where t.ProductID IN (Select p.ProductID From Product p where p.ProductID=t.ProductID) --Correlated
Select * From PurchaseTrans t Where t.ProductID IN (Select p.ProductID From Product p where t.OrderID>1000 AND t.StateProvince = 'Ontario')  --Correlated



Select * From PurchaseTrans
Select * From Product

--Another Example of Correlated query
Select t.*,
(Select p.ProductName From Product p Where t.ProductID=p.ProductID)as ProductName
From PurchaseTrans t

/*
Generate a report to show the count of Order Qty greater than 5 in these countries: Canada,Australia,France
against the overall countries of OrderQty > 5 in a tabular form
*/
Select  ----Main query
	(Select Count(*) from PurchaseTrans where country = 'Canada' and OrderQty>5) as Canada,       ---Uncorrelated Subquery to derive column
	(Select Count(*) from PurchaseTrans where country = 'France' and OrderQty>5) as France,        ---Uncorrelated Subquery to derive column
	(Select Count(*) from PurchaseTrans where country = 'Australia' and OrderQty>5) as Australia,  ---Uncorrelated Subquery to derive column
	(Select Count(*) from PurchaseTrans where OrderQty>5) as AllCountries



--Another Example of Correlated Query (We are referencing the columns of main query into subquery)
Select * From CustomerShipment
Select * FRom PurchaseTrans

Select c.OrderID,c.Supplier,c.Country,c.City, Sum(c.OrderQty) as TotalQty From CustomerShipment as c
Where c.orderID IN (Select p.OrderID from PurchaseTrans p where p.OrderID=c.OrderID AND p.OrderQty>5 AND p.Class='Medium' AND c.Customerclass = p.Color)
Group by c.OrderID,c.Supplier,c.Country,c.City


	-----------------------------------------------------

	--systax: Select column1, column2, column3 From ( )table  Where column...=(condition)

Select s.OrderID,s.ProductID,s.OrderQty,s.TotalAmount,s.SpecialOffer,s.OrderLevel,s.TotalDiscountAmount
From 
(
		Select OrderID,ProductID,UnitPrice,OrderQty,UnitPrice*OrderQty as TotalAmount,
		Case SpecialOfferID
			When 1 then 'BlackFriday'
			When 2 then 'Halloween'
			When 3 then 'Winter Break'
			When 4 then 'Back to School'
			When 5 then 'Summer Break'
			Else 'Undefined'
		End  SpecialOffer, SpecialOfferID, UnitPriceDiscount,
		Case
			When OrderQty<5 then 'Low Order'
			When OrderQty>=5 and OrderQty<=6 then 'Medium Order'
			Else 'High Order'
		End OrderLevel,
		UnitPrice*OrderQty*(1-UnitPriceDiscount) as TotalDiscountAmount
		From PurchaseTrans
) as s  --(s is an alias)

Where s.OrderQty>5

and s.OrderLevel = 'Low Order'

---------------------------------------------------------------------------

--Practice 35  (Uncorrelated Sub Query using IN)

/* Display dataset in PurchaseTransaction of productid in product dataset with productid less than 10*/

Select TransID,ProductID,OrderID,AccountNumber,Supplier,City, (OrderQty*UnitPrice) as PurchaseAmount From PurchaseTrans
Where ProductID IN (Select ProductID From Product Where ProductID < 10)


--Practice 36  (Uncorrelated Sub Query using NOT IN)
/* Display dataset in PurchaseTransaction of productid NOT IN product dataset with productid less than
10*/


Select TransId,ProductID,OrderID,AccountNumber, Supplier, City, (OrderQty*UnitPrice) as PurchaseAmount 
From PurchaseTrans
Where ProductID NOT IN (select ProductID from Product Where ProductId<10)



--Practice 37   ( Correlated)
/* Display dataset in PurchaseTransaction and derives ProductName in product dataset using ProductID as
Correlated column*/

Select TransID,ProductID,OrderID,AccountNumber, Supplier, City, (OrderQty*UnitPrice) as PurchaseAmount,
( Select ProductName From Product Where PurchaseTrans.ProductID = Product.ProductID) as ProductName
From PurchaseTrans



--Practice 38  (Correlated Sub Query IN COLUMN AND EXISTS)
/* Display dataset in PurchaseTransaction and derives ProductName in product dataset Where the Special
OfferId exists in SpecialOffer dataset*/

Select TransID,ProductID,OrderID,AccountNumber, Supplier, City, (OrderQty*UnitPrice) as PurchaseAmount,
(Select ProductName From Product where PurchaseTrans.ProductID = Product.ProductID) as ProductName
From PurchaseTrans
Where Exists ( select 1 From SpecialOffer Where PurchaseTrans.SpecialOfferID=SpecialOffer.SpecialOfferID) 


--Practice 39 (Correlated Sub Query and Query in Column and using NOT EXISTS)
/* Display dataset in PurchaseTransaction and derives ProductName in product dataset Where the Special
OfferId exists in SpecialOffer dataset*/

Select  TransID,ProductID,OrderID,AccountNumber, Supplier, City, (OrderQty*UnitPrice) as PurchaseAmount,
(Select ProductName From Product Where PurchaseTrans.ProductID=Product.ProductID) as ProductName
From PurchaseTrans
Where NOt EXISTS (Select 1 From SpecialOffer Where PurchaseTrans.SpecialOfferID=SpecialOffer.SpecialOfferID) 


--Practice 40 (Uncorrelated Query using ALL)
/* Display dataset in PurchaseTransaction dataset Where ProductID is greater that ALL the product(dataset with ProductID less than 10) in the
sub query dataset*/

Select TransID,ProductID,OrderID,AccountNumber, Supplier, City, (OrderQty*UnitPrice) as PurchaseAmount
From PurchaseTrans
Where ProductID > ALL (Select ProductId From Product Where ProductID<10 ) 


				
--Practice 41 (Correlated Query and Aggregate Function)
/* Display dataset in PurchaseTransaction where Order Quantity multiples Unitprice is greater than Average
of Order Quantity multiples UnitPrice in the sub query dataset*/


Select TransID,OrderID,AccountNumber, Supplier, City, (OrderQty*UnitPrice) as PurchaseAmount
From PurchaseTrans
Where (OrderQty*UnitPrice) > (Select AVG(OrderQty*UnitPrice) From PurchaseTrans)



--Practice 42
/* Display dataset in PurchaseTransaction where Order Quantity multiples Unitprice is greater than Average of
Order Quantity multiples UnitPrice in the Sub Query dataset where Country is Canada*/

Select TransID,OrderID,AccountNumber, Supplier, City, (OrderQty*UnitPrice) as PurchaseAmount
From PurchaseTrans
Where (OrderQty*UnitPrice) > (Select AVG(OrderQty*UnitPrice) From PurchaseTrans)
AND Country = 'Canada'


--============================================
----		 JOINS
--============================================

--INNER JOIN

--Practice 43  (Joining 2 tables)
/* Display dataset in PurchaseTransaction with the matching product on product dataset*/

Select * FRom PurchaseTrans
Select * FRom Product
Select * FRom SpecialOffer
Select * From Category

Select t.TransID,t.OrderID,t.AccountNumber,p.ProductName,p.ProductNumber, t.Supplier, t.City, (t.OrderQty*t.UnitPrice) as PurchaseAmount 
From PurchaseTrans t
INNER JOIN Product p ON t.ProductID=p.ProductID


--Practice 44  (Joining 3 tables)
/* Display dataset in PurchaseTransaction with the matching product and special offer dataset*/

Select  t.TransID,t.OrderID,t.AccountNumber,p.ProductName,p.ProductNumber,s.SpecialOffer, t.Supplier, t.City, (t.OrderQty*t.UnitPrice) as PurchaseAmount 
From PurchaseTrans t
INNER JOIN Product p ON t.ProductID=p.ProductID
INNER JOIN SpecialOffer s ON t.SpecialOfferID=s.SpecialOfferID 


--Practice 45
/* Display dataset in PurchaseTransaction with the matching product and special offer on Back to School
dataset*/
Select  t.TransID,t.OrderID,t.AccountNumber,p.ProductName,p.ProductNumber,s.SpecialOffer, t.Supplier, t.City, (t.OrderQty*t.UnitPrice) as PurchaseAmount 
From PurchaseTrans t
INNER JOIN Product p ON t.ProductID=p.ProductID
INNER JOIN SpecialOffer s ON t.SpecialOfferID=s.SpecialOfferID 
Where s.SpecialOfferID=4

SELECT t.OrderID, t.Supplier, p.CategoryName, p.ProductName, p.ProductNumber, s.SpecialOffer, t.Address,
t.City, t.Country, t.OrderQty
FROM PurchaseTrans t

INNER JOIN
	( 
	SELECT pd.ProductID, c.CategoryName, pd.ProductName, pd.ProductNumber
	FROM Product pd		---ProductTable
	INNER JOIN ProductCategory pc on pc.ProductID=pd.ProductID ---ProductCategory Table
	INNER JOIN Category c on c.CategoryID =pc.CategoryID       ----Category Table
	) p  ON p.ProductID = t.ProductID 
INNER JOIN SpecialOffer s on t.SpecialOfferID =s.SpecialOfferID
WHERE s.SpecialOfferID=4



--Practice 48:  
/* Display dataset in PurchaseTransaction with the matching product on product dataset*/


SELECT t.OrderID, t.Supplier, t.Address, t.City, t.StateProvince, t.Country, t.OrderQty,p.ProductName,p.ProductNumber
FROM PurchaseTrans t
LEFT JOIN Product p ON t.ProductID=p.ProductID


--Practice 49:
/* Display dataset in PurchaseTransaction with the matching Product, Category and special offer on Back to
School dataset*/

SELECT t.OrderID, t.Supplier, p.CategoryName, p.ProductName, p.ProductNumber, s.SpecialOffer, t.Address,
t.City, t.Country, t.OrderQty
FROM PurchaseTrans t

LEFT JOIN
	( 
	SELECT pd.ProductID, c.CategoryName, pd.ProductName, pd.ProductNumber
	FROM Product pd		---ProductTable
	LEFT JOIN ProductCategory pc on pc.ProductID=pd.ProductID ---ProductCategory Table
	LEFT JOIN Category c on c.CategoryID =pc.CategoryID       ----Category Table
	) p  ON p.ProductID = t.ProductID 
LEFT JOIN SpecialOffer s on t.SpecialOfferID =s.SpecialOfferID
WHERE s.SpecialOfferID=4

/*

INNER JOIN:  Returns only the rows where there is a match in both tables.
LEFT JOIN:   Returns all the rows from the left table, and the matched rows from the right table.
               If no match is found, NULL values are returned for columns from the right table
*/

------------ Cross Join (combines each row of one table with each row of another table)


----Practice 51:
/* Display ALL dataset Special Offer and with the matching in PurchaseTrans dataset*/

Select s.SpecialOffer,c.CategoryName From SpecialOffer s
CROSS JOIN Category c


--Practice 52  (FULL OUTER JOIN)
/* Display ALL dataset Special Offer and with the matching in PurchaseTrans dataset*/

SELECT t.OrderID, t.Supplier, p.CategoryName, p.ProductName, p.ProductNumber, s.SpecialOffer, t.Address, t.City, t.StateProvince, t.Country, 
t.OrderQty FROM PurchaseTrans t
FULL OUTER JOIN
	(
	SELECT pd.ProductID, c.CategoryName, pd.ProductName, pd.ProductNumber
	FROM Product pd
	INNER JOIN ProductCategory pc on pc.ProductID =pd.ProductID
	INNER JOIN Category c on c.CategoryID =pc.CategoryID
	)P ON T.ProductID =P.ProductID 
FULL JOIN SpecialOffer s on t.SpecialOfferID =s.SpecialOfferID

--Practice 53
/* Display each row in Special Offer for all rows in Category dataset*/

SELECT
 A.SpecialOffer , B.CategoryName FROM SpecialOffer A CROSS JOIN Category B



 --===============================================
 --			UNION / UNION ALL
 --===============================================


 --UNION:	   Combines the result set of two or more data set WITHOUT DUPLICATE or WITH DISTINCT data
 --UNION ALL : Combines the result set of two or more data set WITH DUPLICATE data.

--Practice 54
/* Display combination of both Customer Shipment and Purchase Transaction datasets with duplicate
data */

Select * From CustomerShipment
Select * From PurchaseTrans

Select OrderId,Supplier, Employee,Country From CustomerShipment
UNION ALL
Select OrderID,Supplier,Employee,Country From PurchaseTrans 


--Practice 55
/* Display Distinct combination of both Customer Shipment and Purchase Transaction datasets without
duplicate data */

Select OrderId,Supplier, Employee,Country From CustomerShipment
UNION 
Select OrderID,Supplier,Employee,Country From PurchaseTrans 
/*
--Interview Question
--What is the difference between Join and Union? 
 JOIN:  (Merge)     Joining is the process of linking tables to produce required data sets.
					The Joins allow extraction of data from multiple tables based on associated key(s) and business requirement(s)

 UNION: (Append)   Combines the result set of two or more data set WITHOUT DUPLICATE or WITH DISTINCT data.

 Key Differences:
 Join adds columns side-by-side
 Union adds rows on top of each other
*/

--===================================
--		INSERT
--===================================
--Practice 56
/* Insert a record into a Product table*/

Insert into Product(ProductId, ProductName,ProductNumber)
Values (105, 'Touring-3000 Yellow, 62 -- Test' , 'BK-T18Y-62 --Test')



--Practice 57
/* Insert multiple records to a table*/
Insert into Product(ProductId, ProductName,ProductNumber)
Values (106, 'Touring-3000 Yellow, 62 -- Test' , 'BK-T18Y-62 --Test'), (107, 'Touring-3000 Yellow, 62 -- Test' , 'BK-T18Y-62 --Test')

--Practice 58
/* Insert a record into a table Using Select Statement*/

Insert into Product(ProductId, ProductName,ProductNumber)
Select 109 as ProductID, 'Touring-3000 Yellow, 63 -- Test' as ProductName, 'BK-T18Y-63 --Test' as ProductNumber

--Practice 59
/* Insert a record into a table Using Select Statement*/

INSERT INTO Product (ProductID, ProductName, ProductNumber)
SELECT ProductID + 1 as ProductID, ProductName +'110' as ProductName, ProductNumber +'110'as ProductNumber
FROM Product
WHERE ProductID =( SELECT MAX (ProductID) FROM Product)

--==========================
--		UPDATE 
--==========================
--Practice 60  /* Update all product number in the product table*/
UPDATE Product SET ProductNumber='HB-M9180' Where ProductId=1


Insert into Product(ProductNumber)
Values 1 'HB-T92800'
/*

 */
 
 --Practice 61 /* Update product number only for productid is 110 in product table*/
  UPDATE Product SET Product.ProductNumber='1' where ProductID='1'

  --Practice 62
/* Update all product number in the product table using product_temp table with the matching product Id = 1*/

UPDATE p
SET p.ProductNumber=t.ProductNumber
FROM product p
INNER JOIN [dbo].[temp_Table] t on p.ProductID =t.ProductID
Where p.ProductID=1


--Practice 63 /* Update all product number in the product table using product_temp table based on inner join on product id = 1*/

UPDATE p
SET p.ProductNumber=t.ProductNumber
FROM product p
INNER JOIN temp_Table t on p.ProductID =t.ProductID

--============================================================
--  DELETE -- TRUNCATE (Remove all or more records in a table)
--============================================================
/*
DELETE:   DML Command
		  Removes one , multiple or all records/rows.
		  Where clause or JOIN condition are  allowed in DELETE
		  Slower Because The DELETE statement removes rows one at a time and records an entry in the transaction log for each deleted row
		  We can Rollback data if we maintain the transaction.


TRUNCATE: DDL Command 
		  Removes all rows from a table but the table structure and column remain.
		  Where clause or JOIN condition are not allowed in TRUNCATE
		  Faster
		  Truncate is not possible when a table is referenced by a Foreign Key.
		  We can not Rollback the data.
*/

 --Practice 64
/* Delete from product table where product Id = 110*/

Select * From Product

Select * From [dbo].[temp_Table]

Delete Product where ProductID = 110

--===  ROllBack Syntax ======

Begin Transaction

Delete Product where ProductID = 2

RollBack  Transaction

--Practice 65
/* Delete all product in the product table where productID is not in product_temp table*/

Delete Product where ProductID NOT IN 
(Select ProductID From [dbo].[temp_Table])

--Practice 66
/* Remove all the data in product_temp1 table */

Delete [dbo].[temp_Table]

--Practice 67
/* Remove all the data in product_temp2 table */

Truncate table [dbo].[temp_Table] 

--======================
-- SCALAR FUNCTIONS
--======================
--Scalar functions operate on a single value to produce a single value. Scalar functions do not necessary
--required GROUP BY clause

--ASCII -- Returns the number code that represents the specific character

Select ASCII('A') as Value
Select ASCII('B') as Value
Select ASCII('C') as Value

--CHAR -- Returns the character based on the number code
Select CHAR (65) As Value
Select CHAR (66) As Value

--CHARINDEX-- Returns the location of a substring in a string (PositionNumber)
--String:    A string is a sequence of characters. It can be a word, a sentence, or any combination of letters, numbers, and symbols.
--Substring: A substring is a part of a string. It's a sequence of characters that appears within another string

Select ProductName, CHARINDEX ('A', ProductName, 1) as PositionNumber From [dbo].[Product]
Where ProductID=1

--DATALENGTH-- Returns the length of an expression

Select ProductName, DATALENGTH(ProductName) as [Length]
From Product

--LEN -- Returns the length of the specified string
Select ProductName, LEN (ProductName) as [Length]
From Product

/*Difference among DATALENGTH, LENGTH and LEN Functions:

DATALENGTH: 
	Used In: SQL Server
    Returns: The number of bytes used to represent any expression.
    Use Case: Useful for finding the actual storage size of data.


LENGTH Function:
	Used In: MySQL, Oracle, PostgreSQL, and SQLite
	Returns: The number of characters in a string, including spaces.

LEN Function:
	Used In: Microsoft SQL Server and Sybase
	Returns: The number of characters in a string, excluding trailing spaces.
*/

--LEFT-- Extracts a substring from a string (starting from Left)

Select ProductName, Left (ProductName,10) as Left10
From Product

--RIGHT-- Extracts a substring from a string (starting from Right)

Select ProductName, Right (ProductName,10) as Right10
From Product

--LOWER -- Converts a string to lower case 
 
Select ProductName, LOWER (ProductName) as LowerCase
From Product

--UPPER -- Converts a string to Upper case
Select ProductName, UPPER  (ProductName) as UpperCase
From Product

--CONCAT -- Concatenates two or more strings together

Select ProductName, ProductNumber, CONCAT (ProductName, '***' , ProductNumber) as Concatenation
From Product

--LTRIM -- Removes leading spaces from a string

Select ProductName, LTRIM  (ProductName) as LEFTTRIM
From Product

--RTRIM -- Removes leading spaces from a string

Select ProductName, RTRIM  (ProductName) as RIGHTTRIM
From Product
Where ProductId = 1

--STR -- The STR function in SQL is used to convert numeric data into string data (character data) in SQL Server. 
--It allows you to specify the total length of the output string and the number of decimal places to include.

Select  STR(ProductID) + ' - ' + ProductName as ProductIDAndProductName
From Product

--STUFF -- Deletes a sequence of characters from a string and then inserts another sequence of characters
--into the string, starting at a specified position

Select STUFF ('Product', 2, 3,'xxx') as ReplaceProduct
Select STUFF ('Product', 2, 5,'xxxxx') as ReplaceProduct

--SUBSTRING -- Extracts a substring from a string
Select ProductName, SUBSTRING (ProductName, 1, 10) Substring 
From Product

--STRING_SPLIT--Returns characters in a table row format based on the specified split character

Select * From string_split ('HL-Mountain-Handlebars','-')


--STRING_AGG -- Generate aggregated strings with separator from a table or list

Select STRING_AGG (CountryName,',') As AggregatedCountryNames
From Country

--HASHBYTES --is the function used by hash algorithms to mask data and it returns varbinary data type
--Algorithms are MD2, MD4,MD5,SHA, SHA1,SHA2_256,SHA2_512 Starting with SQL Server 2016, all algorithms are deprecated except  SHA2_256, and SHA2_512

--CountryName,TotalQty,TotalCount,TotalPurchaseAmount

Select Country, Sum(OrderQty) as TotalQuantity,
Count(*) as TotalCount,
Sum(OrderQty*UnitPrice) as TotalPurchaseAmount 
From PurchaseTrans
Group by Country

--We want to mask Country and TotalQuantity

Select Country,
HASHBYTES ('SHA2_512',Country) as MaskedCountry,
HASHBYTES ('SHA2_512', CAST(Sum(OrderQty)as nvarchar)) as MaskedQuantity,
Count(*) as TotalCount,
Sum(OrderQty*UnitPrice) as TotalPurchaseAmount 
From PurchaseTrans
Group by Country

--CEILING - Returns the smallest integer value that is greater than or equal to the specified number

Select ListPrice as ActualPrice,
CEILING (ListPrice) as CeilingPrice
From PurchaseTrans

--FLOOR -- Returns the largest integer value that is equal to or less than the specified number

Select ListPrice as ActualPrice,
Floor (ListPrice) as CeilingPrice
From PurchaseTrans

--RAND -- Returns a random number or a random number within a range

Select RAND() as Generator 

--ROUND --Returns a rounded number to a certain number of decimal places

Select ListPrice*OrderQty as ActualAmount,
ROUND(ListPrice*OrderQty,1) as RoundAmount
From PurchaseTrans

--======================================
--		CTE (Common Table Expressions)
--======================================

/*
CTE: Its a temprary result set that can be referenced within the execution scope of a single 
SELECT, INSERT, UPDATE, or DELETE statement. It is defined using "With" clause.

Subquey: A subquery is a query nested inside another query. It can be part of a SELECT, INSERT, UPDATE, or DELETE statement.

Difference:

-Readability and Maintenance:     CTEs often make complex queries easier to read and maintain.

-Reusability within a Query:      CTEs can be referenced multiple times within the same query, whereas subqueries cannot.

-Performane:					  Subqueries can sometimes be more performant due to better optimization by the SQL engine, but this varies by scenario.

-Recursion and Hierarchical Data: CTEs support recursion(Self referencing), which is useful for hierarchical data( data that has parent-child relationship), unlike subqueries.

*/

--Practice 96
/* Use CTE to display the Purchase Transaction datasets of PurchaseAmount, supplier and Country*/

With Supplier_Sales_CTE
AS
(
Select OrderQty*UnitPrice as PurchaseAmount, Supplier, Country 
From PurchaseTrans
)

Select * From Supplier_Sales_CTE

--Practice 97
/* Use CTE to display the Purchase Transaction datasets of PurchaseAmount, supplier, Country and
Order Year*/

With Supplier_Sales_CTE
AS
(
Select OrderQty*UnitPrice as PurchaseAmount, Supplier, Country, Year(OrderDate) as OrderYear
From PurchaseTrans
)
Select * From Supplier_Sales_CTE

--Practice 98
/* Use CTE to Display Distinct combination of both Customer Shipment and Purchase Transaction
datasets without duplicate data for Canada */

Select * From CustomerShipment
Select * From PurchaseTrans

With Sales_CTE 
AS
(
Select AccountNumber,Supplier,City,StateProvince,Employee From CustomerShipment Where Country= 'Canada'
Union
Select AccountNumber,Supplier,City,StateProvince,Employee From PurchaseTrans Where Country= 'Canada'
)
Select * From Sales_CTE

--Practice 99
/* Use CTE to Display Purchase Transaction datasets with product name,category and special offer */

With Purchase_Trans_Dataset_CTE
AS
(

Select p.ProductName,p.ProductNumber,c.CategoryName,s.SpecialOffer,SUM(t.OrderQty*t.UnitPrice) as PurchaseAmount
From PurchaseTrans t
INNER JOIN Product p on p.ProductID=t.ProductID
INNER JOIN ProductCategory pc on pc.ProductID=p.ProductID
INNER JOIN Category c on c.CategoryID = pc.CategoryID
INNER JOIN SpecialOffer s on s.SpecialOfferID=t.SpecialOfferID
Group by  p.ProductName,p.ProductNumber,c.CategoryName,s.SpecialOffer
)

Select * From Purchase_Trans_Dataset_CTE

--Practice 101
/*
Use CTE to display the percentile of Purchase Transaction datasets of the supplier by category of purchase amount by Total
Category amount*/

With Purchase_Trans_Dataset_CTE (Supplier,StateProvince,ProductName,SpecialOffer,CategoryName,PurchaseAmount)
AS
(

Select p.ProductName,p.ProductNumber,c.CategoryName,s.SpecialOffer,SUM(t.OrderQty*t.UnitPrice) as PurchaseAmount
From PurchaseTrans t
INNER JOIN Product p on p.ProductID=t.ProductID
INNER JOIN ProductCategory pc on pc.ProductID=p.ProductID
INNER JOIN Category c on c.CategoryID = pc.CategoryID
INNER JOIN SpecialOffer s on s.SpecialOfferID=t.SpecialOfferID
Group by  p.ProductName,p.ProductNumber,c.CategoryName,s.SpecialOffer
)
,

Category_Dataset(c.CategoryID,CategoryAmount)
AS
(
Select c.CategoryID,SUM(t.OrderQty*t.UnitPrice) as CategoryAmount
From PurchaseTrans t
INNER JOIN Product p on p.ProductID=t.ProductID
INNER JOIN ProductCategory pc on pc.ProductID=p.ProductID
INNER JOIN Category c on c.CategoryID = pc.CategoryID
Group by c.CategoryID
)

Select Supplier,StateProvince,ProductName,SpecialOffer,CategoryName,(Round((PurchaseAmount/s.CategoryAmount)*100,4) As PercentilePurchaseTransaction
From Purchase_Trans_Dataset_CTE t
INNER JOIN Category_Sales s on t.categoryID=s.CategoryID


