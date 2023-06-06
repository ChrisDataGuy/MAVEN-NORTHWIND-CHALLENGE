# MAVEN-NORTHWIND-CHALLENGE
![header](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/318916f9-f845-4782-93c1-d79beb20e4af)
A Sales analysis project of a fictitious gourmet food supplier - NORTHWIND TRADERS

## Introduction
This a sales analytics challenge organized by Maven Analytics, below is the challenge objective:
For the Maven Northwind Challenge, you'll be working as a BI Developer for Northwind Traders, a global import and export company that specializes in supplying high-quality gourmet food products to restaurants, cafes, and specialty food retailers around the world.
As part of your role, you've been tasked with building a top-level KPI dashboard for the executive team. Its purpose should be to allow them to quickly understand the company's performance in key areas, including:
-	Sales trends
-	Product performance
-	Key customers
-	Shipping costs
The dashboard should be built to evolve and accommodate new data over time, but you've been encouraged by your manager to have insights & recommendations ready to share with the VPs. This is your chance to impress!

## About the Dataset
It is a sales & order data for Northwind Traders, a fictitious gourmet food supplier, including information on customers, products, orders, shippers, and employees. The dataset can be downloaded [here](https://maven-datasets.s3.amazonaws.com/Northwind+Traders/Northwind+Traders.zip)

## Skills Demonstrated
-	Data cleaning and transformation
-	Data modelling
-	Data visualization
-	DAX measures
-	Parameters
-	Reporting

## Data Transformation and Cleaning
Data cleaning and transformation was done in Power Query editor and following steps were taken:
-	Removed duplicate rows in the _orders_ tables.
-	Set columns to the right data types.
-	Merged the _order details_, orders and _customers_ tables using the **OrderID** and **CustomerID** columns to get a comprehensive _new order details_ table.
-	Merged the _products_ and _categories_ tables using the **ProductID** column to get a new _product table_ containing all products details and their category details.

## Data Modelling
Below is a picture of the data model along with the Dax measures table as well as the parameters:
![MODEL](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/8211dbcc-be17-4b1f-b574-140ca9c817fc)

## DAX Measures
-	Calendar Table was created with the DAX: 
`Calendar Table = CALENDAR(MIN(New_order_table[orderDate]),MAX(New_order_table[shippedDate]))`
The FORMAT, MONTH and WEEDAY DAX formulas were used to get the Year, Quarter, Month, Month Number, Day and Day number of the Calendar table.
-	Total Orders: Calculates the total number of orders generated.
`Total orders = DISTINCTCOUNT(New_order_table[orderID])`
-	Total Sales: Calculates the total amount of sales generated.
`Total Sales = SUM(New_order_table[Sales])`
-	Freight: Calculates the total of shipment cost
`Freight = SUM(New_order_table[freight])`
-	Revenue: Calculates the total revenue generated from sales.
`Revenue = [Total Sales] - [Freight]`
-	% Revenue: To calculate the % of total sales that were revenue
`% Revenue = DIVIDE([Revenue], [Total Sales], 0)`
-	Product Unit sold: Calculates the total product units sold (quantity)
`Product Units Sold = SUM(New_order_table[quantity])`
-	Total Active Products: Calculates the total number of products that are currently active.
`Total Active Products = DISTINCTCOUNT(New_order_table[productID]) - SUM(Products_table[discontinued])`
-	Total Customers: Calculates the total of customers/business partners
`Total Customers = DISTINCTCOUNT(New_order_table[CustomerID])`
-	Average Order Value: Calculates the average value per order
`AOV = [Total Sales] / [Total orders]`
-	Average Delivery time (days): Calculates the average delivery time of an order in days
`Average Delivery days = AVERAGE(New_order_table[Delivery time (days)])`
-	Average fulfillment cost per order: Calculates the average shipment cost per order
`Average Fulfillment cost per Order = [Freight] / [Total orders]`
-	Average order delay time in days: Calculates the average delay time in days, this is based on a calculated column called Shipment delay days which checks for the difference between when an order was expected and when it was finally delivered, returns 0 for values less than or equals to zero.
`Average Order Delay days = AVERAGE(New_order_table[Shipment delay (days)])`
-	Average revenue per order: Calculates the average revenue per order, that is total sales on the order minus the shipment cost.
`Average Revenue per Order = [Revenue] / [Total orders]`
-	Average sales per order: Calculates the average sales generated per order.
`Average Sales per Order = [Total Sales] / [Total orders]`
-	% Sales contribution by country: This calculates the % sales contribution for each country.
`% Sales Contribution = DIVIDE([Total Sales], CALCULATE([Total Sales], ALLSELECTED(New_order_table[country])))`
-	Customer ranking by revenue: Ranks customers based on the revenue they generated. This helped to filter for the top 3 customers by country.
`Customer Rank by revenue = RANKX(ALLSELECTED(New_order_table[CompanyName]), [Revenue])`
-	Team Laura Callahan: Calculates the total sales of sales manager – Laura Callahan and all sales representatives that reports to her in the US.
`Team Laura Callahan = 
(CALCULATE([Total Sales], New_order_table[EmployeeID] = 8) + 
[Nancy Sales] +
[Janet Sales] +
[Margaret Sales])`
-	Team Steven Buchanan: Calculates the total sales of sales manager – Steven Buchanan and all sales representatives that reports to him in the UK.
`Team Steven Buchanan = 
CALCULATE([Total Sales], New_order_table[EmployeeID] = 5) +
[Michael Sales] +
[Robert Sales] +
[Anne Sales]`
-	% Team Laura Sales: Calculates the % contribution to total sales of Laura Callahan and all sales representatives that reports to her in the US.
`% Team laura = DIVIDE([Team Laura Callahan], [Total Sales], 0)`
-	% Team Steven Sales: Calculates the % contribution to total sales of Steven Buchanan and all sales representatives that reports to him in the UK.
`% Team Steven = DIVIDE([Team Steven Buchanan], [Total Sales], 0)`
-	Anne Sales: Calculates the total sales for the Sales representative – Anne Dodsworth
`Anne Sales = CALCULATE([Total Sales], New_order_table[EmployeeID] = 9)`
-	Janet Sales: Calculates the total sales of the sales representative – Janet Leverling
`Janet Sales = CALCULATE([Total Sales], New_order_table[EmployeeID] = 3)`
-	Margaret Sales: Calculates the total sales of the sales representative – Margaret Peacock
`Margaret Sales = CALCULATE([Total Sales], New_order_table[EmployeeID] = 4)`
-	Michael Sales: Calculates the total sales of the sales representative – Michael Suyama
`Michael Sales = CALCULATE([Total Sales], New_order_table[EmployeeID] = 6)`
-	Nancy Sales: Calculates the total sales of the sales representative – Nancy Davolio
`Nancy Sales = CALCULATE([Total Sales], New_order_table[EmployeeID] = 1)`
-	Robert Sales: Calculates the total sales of the sales representative – Robert King
`Robert Sales = CALCULATE([Total Sales], New_order_table[EmployeeID] = 7)`

## Data Analysis and Visualization:
### Top Metrics
The Top metrics showed that we wad $1.27 million  in total sales, with 84% revenue at $1.06 million, shipment accounted for the remainder of $207K. 830 orders were generated which led to the sales of 51,317 units of different products, with an average order value of $1,525.05 and average revenue per order of $1,275.29. The business currently supplies to 89 different customers across 21 countries and 69 cities, supplying a total of 69 active products currently. Delivery is handled by 3 shipping companies at an average order fulfillment cost of $249.77 and an average delivery time of 8.06 days, delays in order delivery averaged at 0.26days.
![Top metrics](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/efdc740b-b37b-48f6-89ad-c1d1c26db9a8)

### Slicers
Slicers were created for country, product category and date to help filter data charts.
![country slicer](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/5319c5cd-8cb5-4096-9280-b2de48234825)
![cayegory slicer](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/748e9e52-eb5d-4daa-8542-12da445f21a7)

### Revenue trend
Revenue showed an upward trend on average from July 2013 to April 2015, however there has been a steady topline growth from December 2014, peaking at April 2015 with total revenue of $103,612.15. This massively dipped in May 2015 to $15,758.88, inference cannot be drawn on this as the data provided insight into just six (6) days of the month of May, a full month’s data will give the full picture but forecast showed it was grow on the sales of April 2015.
![revenue trend](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/6c9cdbe4-8313-4c08-a0a4-9caddf98f2d6)

### Top 3 companies by Country
Using the country filter, this gives us the top 3 companies by each country with reference to total sales generated. The chart provides insight on the revenue generated and freight spent by the top 3 companies. Overall the top 3 companies are:
-	QUICK-Stop (Germany): $110,277.31
-	Ernst Handel (Austria): $104,874.98
-	Save-a-lot Markets (USA): $104,361.95
![top 3 companies](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/eb4926b1-6497-4abf-bf1d-cbfecfab1483)

### Average Revenue per Order vs Average Shipment cost per Order by Country
This used correlation to pick out what countries presented the best markets for the business, what will represent a best scenario will be a high revenue per order and a low to moderate shipment cost per order. Switzerland and Canada presents the best business scenario, with revenue per order above average and shipment cost below average line. Other countries around the average line for shipment cost and good revenue value included Denmark, Belgium, Germany, USA, and Sweden. Outliers like Austria and Ireland incurred high shipment costs but this profitably cushioned by the price point in these countries which is reflected in the high average revenue per order generated in these countries at $2,506.81 and $2,250.81 respectively.
![average revenue vs fullfilment](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/118bf01b-4d5e-4188-8ff6-f18de66168b2)

### Shippers and Shipping Costs
This chart compared the revenue generated by orders delivered by each shipping company and average delivery time in days. For a business whose primary focus is customer satisfaction, focus was on delivery time (On-Time in Full – OTIF) is a major distribution metric and hinges on customer satisfaction, while the data does not provide information on returns to ascertain if orders were delivered in full and there were no damages on transit, we compared delivery time to the total revenue generated for each shipping company. 
United Package raked in $533,547.63 in total sales in delivery of 326 orders and took 17% in total Freight of $91,569.81, delivery time averaged at 8.45 days.
Federal Shipping raked in $319,960.33 in total sales in delivery of 255 orders and took 17% in total Freight of $63,445.14, delivery time averaged at 7.42 days.
Speedy Express raked in $348,839.94 in total sales in delivery of 249 orders and took 15% in total Freight of $52,291.15, delivery time averaged at 8.19 days.
In terms of shipping cost, to maximize revenue more orders should go through the Speedy Express as they had a low shipping cost and a moderate delivery time.
![shippers](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/c24af80d-0ada-493d-b8ae-1ca602069153)

### Sales managers and Sales Representatives
The Vice President of the sales department  - Andrew Fuller overseas the affairs of two sales teams:
-	The sales team in the USA led by the sales manager – Laura Callahan, consisting of three (3) sales representatives – Nancy Davolio, Janet Leverling and Margaret Peacock.
-	The sales team in the UK, led by the sales manager – Steven Buchanan, consisting of three (3) sales representatives – Michael Suyama, Robert King and Anne Dodsworth. 
The USA team contributed 59.63% of total sales while the UK team contributed 27.22% of total sales and 13.15% came from Andrew Fuller.
Below is the sales generated by each sales representative in descending order:
-	Margaret Peacock: $232,891
-	Janet Leverling: $202,813
-	Nancy Davolio: $192,108
-	Robert King: $77,308
-	Michael Suyama: $73,913
![sales managers contribution](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/6133582c-6f13-468d-a182-a5278096d594)
![sales reps](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/ca506b00-b53e-4837-b855-0b91e2dc9c8c)

### In-Depth Analysis of Selected Key Metrics
A dynamic X and Y axis line chart along with a date slicer to help stakeholders granularly look into key metrics and within any date range. This was done using Parameters in Power BI. See picture below:
![dynamic x-y axis analysis](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/4ed9c351-313f-4aaa-8199-076c66b1728e)

### Analysis by Country
A table summarizing some of the key metrics for each country a glance:
![country analysis](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/da4c5186-85ee-45e0-9553-917137e33c8b)

### Insights and Recommendations
In addition to the insights provided above, here are some insights and recommendations from the analysis:
-	Germany represents the best market outside of the USA with a total revenue of $192k and contributing 20% of sales of the topline product category - Beverages. This is further emphasized by the fact that QUICK - Stop is the best performing customer, contributing 47% of total sales in Germany. To maximize profitability and grow revenue, there is need to leverage on the already established market in Germany to drive sales of Meat and Poultry, as it is the category that provides the best revenue per order, with an average of $902.19 and an average order value of $1,012. This can be replicated in the other countries - Austria and USA, that make up the top 3 markets.
-	The Austria and Ireland markets present opportunities to boost revenue and profitability by leveraging on the price point there. The average revenue per order of $2,506.81 and $2,250.81 respectively outweigh shipment costs. Leveraging on these price points, we can drive the sales of Beverages, Dairy Products and Confections. Attract more customers/partners in Austria as its just 2 customers there.
-	In terms of shipping, Federal Shipping provides the best shipping company and plan, while they provide the second best Average fulfillment cost per order of $248.80, putting customer satisfaction at forefront, they provide the best average delivery time  of 7.42 days. The low average shipping cost of Speedy Express can be used to reduce the cost of freight for countries like Austria and Ireland where the cost of freight is already too high, this will in turn improve revenue.
-	A deep dive into the UK team led by Steven Buchanan is recommended to understand the possible challenges of the team and plan to improve on their 27.22% contribution to total sales. Brand promotions, strategic drive of bestselling products using incentives and most likely personnel change is advised. Laura Callahan and the US team are doing a great job contributing 59.62% of total sales, we must on ride this wave by motivating the team by acknowledgements and recognitions and also copying best practice.

Here is our full dashboard of two pages:

![Final Report_Rev_page-0001](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/41e23353-50b8-40a6-a161-dbba39e97fe5)

![Final Report_Rev_page-0002](https://github.com/ChrisDataGuy/MAVEN-NORTHWIND-CHALLENGE/assets/109347195/c81a47f1-017d-42cb-a762-12c7eab9cef2)

#THANK YOU! 😊
