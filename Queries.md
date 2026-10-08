```sql
-- Question 1 --
SELECT
	DISTINCT(SLS.Invoice.SaleTypeRef) AS 'SaleID'
FROM SLS.Invoice
GO
```
<img width="260" height="333" alt="image" src="https://github.com/user-attachments/assets/b68e3ff0-8af4-4752-a523-1701661f2cb5" />


```sql
-- Question 2 --
SELECT 
	SaleTypeRef AS 'SaleID',
	COUNT(*) AS 'Value_Counts'
FROM SLS.Invoice
GROUP BY SaleTypeRef
GO
```
<img width="350" height="357" alt="image" src="https://github.com/user-attachments/assets/6ecf0dc8-8209-43ba-b210-62cc2de33fab" />


```sql
-- Question 3 --
SELECT 
	SaleTypeRef  AS 'SaleID',
	COUNT(*) AS 'Value_Counts',
	CAST((COUNT(*) * 1.0/ SUM(COUNT(*)) OVER()) AS DECIMAL(24,12)) AS 'Value_Fraction'
FROM SLS.Invoice
GROUP BY SaleTypeRef
ORDER BY Value_Fraction DESC;
GO
```
<img width="470" height="339" alt="image" src="https://github.com/user-attachments/assets/94581e17-adf7-4bf6-b433-87807cfe9674" />


```sql
-- Question 4 --
SELECT 
	i.CustomerRealName,
	COUNT(i.Number) AS 'Num',
	SUM(COUNT(i.Number)) OVER() AS 'Total'
FROM sls.Invoice AS i
GROUP BY i.CustomerRealName
GO
```
<img width="471" height="333" alt="image" src="https://github.com/user-attachments/assets/f0b51a30-47e3-4b6b-818c-3bfba8b0d9ae" />

```sql
-- Question 5 --
SELECT 
	i.CustomerRealName,
	COUNT(i.Number) AS 'Num',
	MAX(i.Date) AS 'MaxOrders',
	MIN(i.Date) AS 'MinOrders'
FROM sls.Invoice AS i
GROUP BY i.CustomerRealName
GO
```
<img width="689" height="247" alt="image" src="https://github.com/user-attachments/assets/1adbea94-c9bb-4860-a5a1-05fdf0a9b022" />

```sql
-- Question 6 -- SQ(SubQuery)
SELECT
	SQ.CustomerRealName,
	SQ.Num,
	MAX(SQ.MaxOrders) OVER() AS 'MaxOrders',
	MIN(SQ.MinOrders) OVER() AS 'MinOrders'
FROM 
(SELECT 
	i.CustomerRealName,
	COUNT(i.Number) AS 'Num',
	MAX(i.Date) AS 'MaxOrders',
	MIN(i.Date) AS 'MinOrders'
FROM sls.Invoice AS i
GROUP BY i.CustomerRealName) AS SQ;
GO
```
<img width="908" height="336" alt="image" src="https://github.com/user-attachments/assets/503258ba-8ed1-497f-a31b-e9db5ea5c546" />

```sql
-- Question 7
SELECT 
  i.ItemID,
  i.Title,
  i.isActive
FROM inv.Item AS i
WHERE i.Title LIKE N'حلوا%';
GO
```
<img width="230" height="174" alt="image" src="https://github.com/user-attachments/assets/a75c3a8d-36cb-41e8-9fe4-103881d2af3d" />

```sql
-- Question 8-1
SELECT 
  i.InvoiceItemID,
  i.InvoiceRef
FROM SLS.InvoiceItem as i 
JOIN INV.Item AS ii ON i.ItemRef = ii.ItemID
WHERE ii.Title LIKE N'حلوا%'
GO
```
<img width="300" height="336" alt="image" src="https://github.com/user-attachments/assets/bfeff6d3-4988-4830-ab46-1747088d23ea" />

```sql
-- Question 8-2
SELECT
  i.InvoiceItemID,
  i.InvoiceRef
FROM SLS.InvoiceItem AS  i 
WHERE ItemRef IN (SELECT
  ii.ItemID
FROM INV.Item AS  ii
WHERE Title LIKE N'حلوا%')
ORDER BY i.InvoiceItemID;
GO
```
<img width="300" height="336" alt="image" src="https://github.com/user-attachments/assets/a3438972-61c9-4511-958b-491b084917b1" />

```sql
-- Question 9-1
SELECT
  ii.ItemID,
  ii.Title
FROM INV.Item AS  ii
LEFT JOIN SLS.InvoiceItem AS i ON ii.ItemID = i.ItemRef
WHERE ItemRef IS NULL
GO
```
<img width="310" height="335" alt="image" src="https://github.com/user-attachments/assets/f311b35f-f25f-46d4-a510-f6a728e30968" />

```sql
-- Question 9-2
SELECT
  ii.ItemID,
  ii.Title
FROM INV.Item AS ii 
WHERE ItemID NOT IN (SELECT
  i.ItemRef
 FROM sls.InvoiceItem AS i);
 GO
```
<img width="310" height="335" alt="image" src="https://github.com/user-attachments/assets/359bebd8-f1e4-4caf-86c7-7fbfa4a59c27" />
