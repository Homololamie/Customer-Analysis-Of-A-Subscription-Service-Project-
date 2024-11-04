#The Incubator Hub Customer Analysis Project
This contains my project documentation of Customer Segmentation Analysis For A Subscription Service


### Project Title: Customer Segmentation Analysis For A Subscription Service

### Project Overview
The Project Aims To Analyze Customer Data For A Subscription Service To Identify Segments And Trends. 
By Exploring The Various Parameter I Seek To Uncover Customer Behavior, Track Subscription Types, And Identify Key Trends In Cancellations And Renewals. 

### Data Source
The Data Was Provided By The Incubator Hub ,In CSV Format. 

##### The Dataset Includes The Following Columns:
Customerid: Unique Identifier For Each Customer
Customername: Customer Name
Region: Customer Location 
Subscription Type : The Type Of Subscription Purchased By Each Customer
Subscription Start: The Date Subscription Was Purchased By Customer
Subscription End :The  Date  Subscription Ended
Cancelled: This Column  Indicate Whether The Subscription Is Cancelled Or Not 

###### Screenshot Of The Data Is Shown Below

### Tools Used
  - Microsoft Excel For Cleaning And Analysing The Data.
  - Structered Query Language (SQL) For Querying The Data
  - Microsoft Powerbi For Visualization

### Data Cleaning And Preparation
- Ensured Data Quality By Correcting Any Spelling Errors, Removing Duplicate Entries And Blank Cells
- Ensured All Data Fields Were Assigned The Correct Data Types And Date Fields Set To Date Format.
- Created New Columns :
- Duration: This Was Created  Subtracting Subscription Start From Subscription End
- Created Pivot Table

#### Data Screenshot  After Cleaning  

### Excel Analysis
The Following Analysis Was Performed Using Pivot Table 
-	Revenue By Region

-	Revenue By Subscription Type

-	Number Of Subscription Type

-	Percentage Revenue By Subscription Type

-	Average Duration By Subscription Type 

-	Revenue By Month

-	Month By Number Of Subscription

-	Revenue By Cancelled

-	Percentage Revenue By Cancelled

-	Revenue By Region | Cancelled

-	Cancelled By Number Of Cancelled

-	Revenue By Region | Subscription Type

The Following Analysis Was Performed Using Excel Function
-	Average Subscription Duration 

-	The Most Popular Subscription Types.
### Structured Query Language (SQL) Analysis

#### Data Cleaning And Preparation
  - Imported Customer Data Into Project_Lita  Database Already Created 
- Data Datatype Was Checked For Error, Subscriptionstart And Subscriptionend  Have 'Text' Datatype
-  Changed Subscriptionstart And Subscriptionend Column Text Datatype To  Date Datatype With The Query Below
-- Subscription Start
 ```Sql
UPDATE lita_customer
SET subscriptionstart  = (CASE WHEN
subscriptioNStart LIKE '%/%' THEN
DATE_FORMAT(STR_TO_DATE(subscriptionstart,'%m/%d/%Y'),'%Y-%m-%d')
End); 
```
```Sql
ALTER table lita_customer
MODIFY column subscriptionstart Date; 	
```
	Subscription End
``` Sql 
UPDATE lita_customer
SET subscriptionend  = (CASE WHEN
subscriptionend LIKE '%/%' Then 
DATE_FORMAT(STR_TO_DATE(subscriptionEnd,'%m/%d/%Y'),'%Y-%m-%d')
END);
```

	``` Sql
	aLTER table lita_customer
MODIFY column subscriptionend Date;
	```
- To Confirm The datatype Has Been  Changed, The Query Below Was Used
  
```Sql
DESCRIBE lita_Customer;
```
#### The Following Queries was written to Extract The Following Insights From the data :	

1)  retrieve the total number of customers from each region. 
``` Sql
SELECT region, COUNT(DISTINCT customerid) AS 'Number of Customer'
FROM  lita_customer
GROUP BY region;
```

2) find the most popular subscription type by the number of customers. 
``` Sql
SELECT subscriptiontype, COUNT(*) AS 'Count of SubscriptionType'
FROM lita_customer
GROUP BY SubscriptionType;
```





3) find customers who canceled their subscription within 6 months.
``` Sql
SELECT CustomerName, SubscriptionStart ,SubscriptionEnd,Canceled AS 'Cancelled Subscription', 
DATEDIFF(subscriptionend, subscriptionstart) AS 'Sub_Diff'
FROM lita_customer 
WHERE DATEDIFF(subscriptionend, subscriptionstart) <= (365/2);
```
4) calcuate the average subscription duration for all customers. 
``` Sql
SELECT ROUND(AVG((DATEDIFF(subscriptionend,subscriptionstart)))) AS 'Avg Duration'
FROM lita_customer;
```

5) find customers with subscriptions longer than 12 months. 
``` Sql
SELECT DISTINCT CustomerName,DATEDIFF(subscriptionend, subscriptionstart) AS 'Sub Duration'
FROM lita_customer
WHERE DATEDIFF(subscriptionend, subscriptionstart) > 365;
```
6)  calculate total revenue by subscription type. 
``` Sql
SELECT SubscriptionType, SUM(REvenue) as 'Total Revenue'
From lita_customer
GROUP BY SubscriptionType;
	```
	


7)  find the top 3 regions by subscription cancellations. 
``` Sql
SELECT region, COUNT(canceled) AS 'Number of Subscription Cancellations'
FROM lita_customer
WHERE canceled = 'false'
GROUP BY region
ORDER BY COUNT(canceled) DESC
LIMIT 3;
```

8)  find the total number of active and canceled subscriptions.
``` Sql
SELECT Canceled, COUNT(*) AS ' Number of Subscription'
FROM lita_customer
GROUP BY  canceled;
```
