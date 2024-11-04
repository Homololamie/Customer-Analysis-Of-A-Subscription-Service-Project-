#The Incubator Hub Customer Analysis Project
This contain my project documentation on Customer Segmentation Analysis For A Subscription Service


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

![image](https://github.com/user-attachments/assets/2696309a-5c9e-4774-8533-3930da6d0d21)


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

![image](https://github.com/user-attachments/assets/e9367617-e42a-4138-b1d7-259069ec58b1)


### Excel Analysis
The Following Analysis Was Performed Using Pivot Table 
-	Revenue By Region

 ![image](https://github.com/user-attachments/assets/81863c50-91e4-46cd-96d4-ce36f2f202f7)


-	Revenue By Subscription Type
	
  ![image](https://github.com/user-attachments/assets/31b0a4a0-9266-4a53-acdd-418f662885e1)

  

-	Number Of Subscription Type

 ![image](https://github.com/user-attachments/assets/1d3ba706-72b4-479c-ba64-3b86371d3734)
 

-	Percentage Revenue By Subscription Type

  ![image](https://github.com/user-attachments/assets/be5a936a-6ff4-426b-9710-dbeaf16c30f9)


-	Average Duration By Subscription Type

  ![image](https://github.com/user-attachments/assets/e2b26829-58af-43c3-b697-f0cf0e9567f0)


-	Revenue By Month

![image](https://github.com/user-attachments/assets/eeb74fd6-0963-4a17-a47e-e82f619634e5)



-	Month By Count  Of Subscription

  ![image](https://github.com/user-attachments/assets/39551c9e-a0e1-4bb4-94f9-34b7a3ab80cc)


-	Revenue By Cancelled 

  ![image](https://github.com/user-attachments/assets/0e3ea337-eed7-477b-9fa2-84395b25ed9e)

  

-	Percentage Revenue By Cancelled

  ![image](https://github.com/user-attachments/assets/6af124e2-44ac-4863-9d2b-221f8927bef2)


-	Revenue By Region | Cancelled

![image](https://github.com/user-attachments/assets/8e058aef-0c01-42f1-a8fb-c11f0b071f59)

-	Count  Of Cancelled

![image](https://github.com/user-attachments/assets/c930afe6-399c-4158-bd39-549250506845)

-	Revenue By Region | Subscription Type

 ![image](https://github.com/user-attachments/assets/ac6f13a4-e269-4dc1-b831-0ad68ba93a77)


The Following Analysis Was Performed Using Excel Function

-	Average Subscription Duration 

Formula:	![image](https://github.com/user-attachments/assets/80b16cb0-9c76-4cee-b41a-21eb2c15aa55) 
Answer:		![image](https://github.com/user-attachments/assets/f932aacc-16d4-4c67-9489-ddf44f4ee516)


-	The Most Popular Subscription Types.

  formula:	![image](https://github.com/user-attachments/assets/9b640ead-2ac8-45e6-b93f-72260e95b42b) 
  
  Answer:	![image](https://github.com/user-attachments/assets/bf288466-c09d-41b3-bab9-bf6abc18c04c)

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
-- Subscription End

``` Sql 
UPDATE lita_customer
SET subscriptionend  = (CASE WHEN
subscriptionend LIKE '%/%' Then 
DATE_FORMAT(STR_TO_DATE(subscriptionEnd,'%m/%d/%Y'),'%Y-%m-%d')
END);
```

``` Sql
ALTER table lita_customer
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
![image](https://github.com/user-attachments/assets/62fdf4eb-094c-40cd-8dbd-3a38d3c4c2ae)


2) find the most popular subscription type by the number of customers. 
``` Sql
SELECT subscriptiontype, COUNT(*) AS 'Count of SubscriptionType'
FROM lita_customer
GROUP BY SubscriptionType;
```
![image](https://github.com/user-attachments/assets/a39e2480-50b1-4dbc-acff-6a599fb2c913)


3) find customers who canceled their subscription within 6 months.
``` Sql
SELECT CustomerName, SubscriptionStart ,SubscriptionEnd,Canceled AS 'Cancelled Subscription', 
DATEDIFF(subscriptionend, subscriptionstart) AS 'Sub_Diff'
FROM lita_customer 
WHERE DATEDIFF(subscriptionend, subscriptionstart) <= (365/2);
```
No record was return for this query, which mean no customer cancelled their subscription within 6 month.See Below

![image](https://github.com/user-attachments/assets/8361193f-c768-4be6-b11a-42ecfb0570e1)


4) calcuate the average subscription duration for all customers. 
``` Sql
SELECT ROUND(AVG((DATEDIFF(subscriptionend,subscriptionstart)))) AS 'Avg Duration'
FROM lita_customer;
```

![image](https://github.com/user-attachments/assets/170676e2-d9fe-475e-9172-90d121c0e00a)


5) find customers with subscriptions longer than 12 months. 
``` Sql
SELECT DISTINCT CustomerName,DATEDIFF(subscriptionend, subscriptionstart) AS 'Sub Duration'
FROM lita_customer
WHERE DATEDIFF(subscriptionend, subscriptionstart) > 365;
```

![image](https://github.com/user-attachments/assets/6cabbcea-4660-4bdf-a7d5-45dd212d67d8)


6)  calculate total revenue by subscription type. 
``` Sql
SELECT SubscriptionType, SUM(REvenue) as 'Total Revenue'
From lita_customer
GROUP BY SubscriptionType;
```
![image](https://github.com/user-attachments/assets/cea5ecda-901b-436e-844b-16834be93e9c)


7)  find the top 3 regions by subscription cancellations. 
``` Sql
SELECT region, COUNT(canceled) AS 'Number of Subscription Cancellations'
FROM lita_customer
WHERE canceled = 'false'
GROUP BY region
ORDER BY COUNT(canceled) DESC
LIMIT 3;
```

![image](https://github.com/user-attachments/assets/ef983e87-3c4e-4ce6-9bc4-cf66cef63095)



8)  find the total number of active and canceled subscriptions.
``` Sql
SELECT Canceled, COUNT(*) AS ' Number of Subscription'
FROM lita_customer
GROUP BY  canceled;
```
![image](https://github.com/user-attachments/assets/693c9d09-796e-41ec-b427-c5c5632f4a19)

