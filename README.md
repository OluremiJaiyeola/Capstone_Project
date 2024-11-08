# Capstone_Project
---

[Project Overview](#project-overview)

[Data Cleaning and Preparation](#data-cleaning-and-preparation)

[Data Sources](#data-sources)

[Tools Used](tools-used)

[Exploratory Data Analysis](#exploratory-data-analysis)

[Data Analysis with Excel](#data-analysis-with-excel)

[Data Visualization with Power BI](#data-visualization-with-power-bi)

[Data Analysis with SQL](data-analysis-with-sql)

[Insights on Analysis](#insights-on-analysis)


## Project Overview.
---

In this project, we ware tasked with analyzing the sales performance of a retail store. we will need to explore sales data to uncover key insights such as top - selling products, regional performance and monthly sales trends. The goal is to produce an interractive Power BI dashboard that highlights these findings. The purpose is also to understand, interprete and present the data for easy understanding and call to action. I worked on the Sales data set and the Customer data set through which i used Mictosoft Excel, SQL and Power Bi. These two data contain data set for: 
- Sales Data - This shows the selling of different products in different Regions at different times.
- Customer Data - This shows customer behaviour to TV subscription over a certain period of time.

## Data Cleaning and Preparation.
---
This is where some lines of codes, queries and DAX Expressions are used during the analysis. At the initial stage, some cleaning and preparations were performed:
- Loading data and inspection 
- Cleaning the data
- Removing the duplicates
- Handling missing variables

## Data Sources
---
The main soources for this data anaylsis are the "Sales Data.csv" and "Customer Data.csv" which are open source datasets which are available for free download from online repositories like Kaggle, FRED or other similar platforms. 

## Tools Used.
---
- Microsoft Excel: Employed for data cleaning and visualization [Download Here](https://www.microsoft.com)
- STructured Query Language (SQL): For data cleaning through queries
- Power Business Intelligence (Power BI): For both data cleaning and visualization. [Download Here](https://www.microsoft.com)

## Exploratory Data Analysis.
---
EDA involves the exploring of data to answer some questions about the data such as;

### Sales Data
- What is the total sales for each product category
- Number of sales in each region
- Total revenue per product Monthly Sales totals for the current year
- Top 5 customers  by purchase amount
- Percentage of total sales contributed by each region
- Products with no sales in the last quarter

### Customer Data
- The total number of customers for each region
- The most popular subscription type by number of customers
- Customers who cancelled their subscription within 6 months
- Customers with subscription longer than 12 months
- Total revenue by subscription type
- Top 3 regions by subscription cancellations
- Total number of active and cancelled subscription

## Data Analysis with Excel.
---
### Sales Data

![Screenshot (13)](https://github.com/user-attachments/assets/7642ddd5-a856-47f3-99ce-c3021e3be33d)

![Screenshot (14)](https://github.com/user-attachments/assets/5200f7a8-b048-47ed-a2ae-7b8e76f6555c)

### Customer Data

![Screenshot (16)](https://github.com/user-attachments/assets/f1434706-5756-4f7f-ad13-743f429ad4e1)

![Screenshot (17)](https://github.com/user-attachments/assets/4f699be4-21ef-49cf-820f-690e3cd42b44)

## Data Visualization with Power BI.
---

### Sales data

![Screenshot (18)](https://github.com/user-attachments/assets/06c50952-e1f1-4d19-9876-56e8e95a4f5d)


Customer data

![Screenshot (19)](https://github.com/user-attachments/assets/48a3244d-166d-4ef0-9c3f-bf4604f62f3a)



![Attrition count 2](https://github.com/user-attachments/assets/10d925bf-c91a-4239-8529-f636fc032f7d)




## Insights on Analysis.
---

### Sales Data
- Shoes is the best selling product with over 600,000 sales
- Socks is the least selling product with about 180,000 sales
- The busiest month was February
- There was strong and high sales in the first and second quarter
- Sales was down by about 50% in the last quarter
- In all the regions, South had 44% of sales, East had 23%, North had 18% and lastly West with 14%
- Average sales for the year was 211.78% showing strong customer interest throughout the year 

### Customer data
- Basic Subsciption emerged as the most popular type of subscription accounting for over 50% of all the subscriptions
- Average Subscription was 365vdays
- East showed a remarkable commitment to their subscription with zero cancellation while others occasionaly cancel their subscription
- Total revenue generated was 68 million which is quite impressive and also shows the loyalty cusomers place on theie subscription thereby building a strong foundation for the service
 
## Data Analysis with SQL.
---

### Sales Data
```SQL
SELECT * FROM [dbo].[Sales Data];

- Total sales from each product category 
select PRODUCT, SUM(Total_Sales) as Totalsales from [dbo].[Sales Data]
group by Product;

- Number of sales transaction in each region 
select Region, COUNT(Total_Sales) as  No_of_sales_by_region
from [dbo].[Sales Data]
Group by Region

- Highest selling product by total sales value 
select PRODUCT, SUM(Total_Sales) as highest_selling_product from [dbo].[Sales Data]
group by Product
order by PRODUCT desc

- Total revenue per product 
select PRODUCT, SUM(Total_Sales) as Totalsales from [dbo].[Sales Data]
group by Product;

- Monthly sales totals for the current year 
select month(orderdate) as month,
SUM(quantity*unitprice) as monthly_sales
from[dbo].[Sales Data]
where YEAR(OrderDate)=YEAR(GETDATE())
group by MONTH(OrderDate);

- Top 5 customers by total purchase amount 
select top 5 customer_Id,
sum(total_sales) AS Total_purchase_Amount
From [Sales Data]
group by customer_ID
order by
Total_purchase_Amount DESC;

- Percentage of total sales contributed by each region 

SELECT REGION, sum(quantity*unitprice) as Totalsales,
sum(quantity*unitprice)* 1.0/ (select sum(Quantity * Unitprice) 
from [dbo] .[Sales Data]) * 100
as PercentageOfTotalSales
from [dbo] .[Sales Data]
group by Region;

- Products with no sale in the last quater 
select distinct product, orderid, quantity
from [dbo].[Sales Data]
where product NOT IN
(select distinct product
from [dbo].[Sales Data]
where OrderDate between '2024-07-01' and '2024-09-30'
)

Customer Data

SELECT * FROM [dbo].[Customer Data];

- Total number of customers from each region------
Select region, COUNT(distinct customerid)
as total_customer from [dbo].[Customer data]
group by Region;

- Most popular subscription type by the number of customers----
Select top 1 subscriptiontype, 
COUNT(distinct customerid)
as total_customers from [dbo].[Customer data]
group by SubscriptionType
order by total_customers desc;

- Customers who cancelled their subscription within 6 months 
select customerid, customername, canceled
from [dbo].[Customer data]
where canceled = 'true' and month (subscriptionStart) between 1 and 6

- Average subscription duration for all customers 
select AVG(datediff(day, subscriptionstart, subscriptionend))
as avg_subscription_duration from [dbo].[Customer data]

- Customers with subscription longer than 12 months 
select customerid from [dbo].[Customer data]
where DATEDIFF(month, subscriptionstart, subscriptionend) >12;

- Total Revenue by subscription type 
select subscriptiontype, 
SUM(revenue) as total_revenue from [dbo].[Customer data]
group by SubscriptionType;

- Top 3 regions by subscription cancellation 
select Region, COUNT(customerid) as Totalccancellations
from [dbo].[Customer data]
where canceled = 'True'
group by Region
order by Totalccancellations DESC;

- Total number of active and cancelled subscription 
Select canceled, COUNT(customerid) as SubscriptionCount
from[dbo].[Customer data]
Group by Canceled;






