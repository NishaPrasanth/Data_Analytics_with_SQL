# Data_Analytics_in_bigquery_SQL
## 1. Project Overview

***Project Name:*** ***Analysing E-commerce data in bigquery GCP platform*** 

***Dataset Name:*** ***Public Dataset: The Look E-commerce***  Link : https://console.cloud.google.com/bigquery?ws=!1m4!1m3!3m2!1sbigquery-public-data!2sthelook_ecommerce

***Objective:*** The objective is to understand customer behavior, optimize sales strategies, and improve business operations by tracking key metrics, identifying trends, and uncovering areas for improvement



## 2. Data Sources  
ID : bigquery-public-data.thelook_ecommerce

| Table Name               | Description                    |
|--------------------------|--------------------------------|
| distribution_center      | Number of rows 10              |
| events                   | Number of rows 2,422,740       |
| inventory_items          | Number of rows 488,990         |
| order_items              | Number of rows 181,065         |
| orders                   | Number of rows 124,811         |
| Products                 | Number of rows 29,120          |
| users                    | Number of rows 100,000         |



## 3.Extracting tables from public dataset to Bigquery table

step 1: Open the **bigquery sandbox**  and create a **Project file**.

step 2: after opening the bigquery console click on the ***+ Add data***option present on the ***Explore*** panel.

Step 3: In **Add data** screen click on the **public datasets** option or type public dataset.

step 4: In public datasets there will be a **search marketplace**, type  the dataset name **"thelookEcommerce"**

step 5: there will be a product details of the data click on the **view datasets** option.

step 6: the dataset will be visible in the **explore panel** under **bigquery public data**.

step 7: select the dataset under the dataset there will be 7 table. select each table to explore the table **schema, details, preview**.

step 8: Now extract the tables from public to local project file as **bigquery tables** by creating a dataset under a projectfile.

step 9: this can be done as by selecting **search query** option above the table information.The **untitled query** space opens to query the table. use the below statement to select the whole table to view. 


```
SELECT * FROM `bigquery-public-data.thelook_ecommerce.distribution_centers`

``` 
Step 7: Select the needed columns.The result will be viewed down as query result.Save the results by clicking the **Save results -> bigquerytable**.

step 8: **Destination** screen pops up where type **file name ,the dataset name and table name** and click **Save**. the table with selected columns save in the local repository for analysis.

Use the following methods to extract all the tables to the local project file. 

## 4. Data Profiling and Quality Check

After saving all the tables to the local file. check for the duplicates,missing values in the table.

1. Checking Null

* Select the distribution table and observe the column names. select the search query and type the below code to check for null count in each row of selected column.

```
SELECT COUNT(*) AS null_count
FROM `my-project-001-415805.Ecommerce.Distribution_center` 
WHERE name IS NULL 
```

![Screenshot 2025-03-29 152501](https://github.com/user-attachments/assets/15587041-403d-4ff6-b190-64362fd99c8a)

the result shows with 0 null count. there is no null count.

* Events table.
```
SELECT COUNT(*) AS null_count
FROM `my-project-001-415805.Ecommerce.events_data`where event_type IS  NULL
```
![Screenshot 2025-03-29 153119](https://github.com/user-attachments/assets/8bd0430f-01ee-4dd8-8a39-582afb950fe9)


the result shows with 0 null count. there is no null count.

* Inventory_items table.

while exploring the inventory items table. all other columns except sold_at has no null count.
the sold_at column has data of only sold products and there is null value in unsold products.So, we keep the null count as unsold data count

```
SELECT COUNT(*) AS null_count
FROM `my-project-001-415805.Ecommerce.inventory_items` 
WHERE created_at IS NULL
```
there is no data to display.

```
SELECT  COUNT(*) AS unsold,
FROM `my-project-001-415805.Ecommerce.inventory_items` 
where sold_at is null

```

```
SELECT  COUNT(*) AS unsold,
count(*)-count(sold_at) as sold_out
FROM `my-project-001-415805.Ecommerce.inventory_items` 
```

![Screenshot 2025-03-29 161710](https://github.com/user-attachments/assets/5ec7dbc4-ca33-4bf9-bb3c-e31502d2179c)

![Screenshot 2025-03-29 161530](https://github.com/user-attachments/assets/8b329c2c-9ea2-449a-8f98-62e285626863)

by using the above results we sort the sold_out and un_sold data by finding the null count in sold_at. we need the null rows for identifying the unsold products.

* order_items table

```
SELECT status
FROM `my-project-001-415805.Ecommerce.order_2`
where status is null
```
there is no null in the status column, check all other columns in the table.

```
SELECT SUM(CASE WHEN  shipped_at IS NULL THEN 1 ELSE 0 END) AS ship_null_count,
    SUM(CASE WHEN delivered_at IS NULL THEN 1 ELSE 0 END) AS delivery_null_count,
    SUM(CASE WHEN returned_at IS NULL THEN 1 ELSE 0 END) AS return_null_count,
FROM `my-project-001-415805.Ecommerce.order_2`

```
by checking the three columns of order data. we have total the following null values in column.This shows the product delivered or shipped or returned data. So, deleting the null can cause the missed information.


<img width="895" alt="image" src="https://github.com/user-attachments/assets/45936749-e995-4d03-b554-c274b18dfd21" />


* order data table

 ```
  SELECT count(created_at) as null_count 
FROM `my-project-001-415805.Ecommerce.order_data` 
where created_at is null
```

there is no null count in order table.

* Product data table

```
SELECT count(retail_price) as null_count 
FROM `my-project-001-415805.Ecommerce.product_data` 
where retail_price is null
```
by selecting various columns there is no null count in product data table.

* user data table

```
SELECT count(created_at) as null_count, 
 FROM `my-project-001-415805.Ecommerce.user_data`
 where created_at is null
```
there is no null count in any user table column.

## finding Duplicated

1. no duplicated in distribution center data
   
```
SELECT id, COUNT(*) ,
 FROM `my-project-001-415805.Ecommerce.Distribution_center` 
 GROUP BY id
HAVING COUNT(*) > 1
```
2. no duplicated in events data
```   
 SELECT id, COUNT(*) 
FROM `my-project-001-415805.Ecommerce.events_data`
GROUP BY id
HAVING COUNT(*) > 1
```
3.no duplicated in inventory_items data
```
 SELECT id, COUNT(*) 
FROM `my-project-001-415805.Ecommerce.inventory_items`
GROUP BY id
HAVING COUNT(*) > 1
```
4.no duplicated in order_items data 
```
SELECT id, COUNT(*) 
 FROM `my-project-001-415805.Ecommerce.order_2` 
 GROUP BY id
HAVING COUNT(*) > 1
```
5.no duplicated in orders data 

```SELECT order_id, COUNT(*) 
 FROM `my-project-001-415805.Ecommerce.order_data` 
 GROUP BY order_id
HAVING COUNT(*) > 1
```
6.no duplicated in products data 
```
SELECT id, COUNT(*) 
 FROM `my-project-001-415805.Ecommerce.product_data` 
 GROUP BY id
HAVING COUNT(*) > 1
```
7.no duplicated in  user data 
```
SELECT id, COUNT(*) 
 FROM `my-project-001-415805.Ecommerce.user_data` 
 GROUP BY id
HAVING COUNT(*) > 1
```
## Join Data 

Joining the common data order_items and order together

```
SELECT a.user_id,a.order_id,a.created_at,a.num_of_item,a.status,
b.product_id,b.sale_price,
FROM `my-project-001-415805.Ecommerce.order_data` a 
JOIN `my-project-001-415805.Ecommerce.order_2` b on a.order_id = b.order_id
```

<img width="740" alt="image" src="https://github.com/user-attachments/assets/86144515-ae37-45a7-9e66-8d88cad3b1e3" />


## extracting dateformat 



## Analysing
* **Summary of distribution center data**
```
SELECT count(id)as total_center
FROM `my-project-001-415805.Ecommerce.Distribution_center`
```
there are total 10 centers 


```
SELECT *
FROM `my-project-001-415805.Ecommerce.Distribution_center`

```
<img width="718" alt="image" src="https://github.com/user-attachments/assets/797a25e9-4cae-4d4e-a95e-b59f20d90846" />

* **summary inventory data**
  
```
SELECT extract(year from created_at) as year,
       count(id) as Total_inventory,
       round(sum(cost)) as total_cost,
       round(avg(cost)) as avg_cost,
       round (Avg (product_retail_price)) as Avg_price, 
       count(sold_at) as sold_out ,
       count(*)-count(sold_at) as not_sold_out,
FROM `my-project-001-415805.Ecommerce.inventory_items` 
group by year
order by  year DESC

```
<img width="617" alt="image" src="https://github.com/user-attachments/assets/97fc8cf4-85b2-4890-9a95-85f91e606b66" />

```
SELECT extract(year from created_at) as year,
       product_distribution_center_id as center,
       count(id) as Total_inventory,
       round(sum(cost)) as total_cost,
       round(avg(cost)) as avg_cost,
       round (Avg (product_retail_price)) as Avg_price, 
       count(sold_at) as sold_out ,
       count(*)-count(sold_at) as not_sold_out,
FROM `my-project-001-415805.Ecommerce.inventory_items` 
group by year,center
order by year,center
```


<img width="623" alt="image" src="https://github.com/user-attachments/assets/0507322f-4d2f-43f9-9792-71aeb134d1b8" />

* **events data summary:**
  
```
  select event_type,
count(id) as total_events, round(avg(sequence_number)) as sequence,
FROM `my-project-001-415805.Ecommerce.events_data` 
group by event_type
```


<img width="616" alt="image" src="https://github.com/user-attachments/assets/2e34e34e-0bc5-4702-b1ee-594b2b407ff5" />

```
select state,city, count(*) as total_events
FROM `my-project-001-415805.Ecommerce.events_data` 
group by state,city 
order by total_events desc
limit 10
```
<img width="645" alt="image" src="https://github.com/user-attachments/assets/26f9b221-9eb5-4236-a3b8-57b2f3d1c164" />

```
select traffic_source, count(*) as total_events
FROM `my-project-001-415805.Ecommerce.events_data` 
group by traffic_source 
order by total_events desc
```
<img width="620" alt="image" src="https://github.com/user-attachments/assets/9fa1be0b-815b-4db3-9262-9e6b3c531cdb" />




1. **Understanding Customer Behavior:**
   * **Identify trends and patterns:**
     Analyze website traffic, navigation paths, and user engagement to understand how customers browse and interact with your online store.
   * Group customers based on demographics, purchase history, and online behavior to personalize marketing efforts and product recommendations.
   * **Improve customer experience:** Identify areas where customers drop off in the funnel (e.g., abandoned carts, slow checkout process) and optimize the online experience. 
   

1. **Optimizing Marketing Strategies:**
   * **Measure campaign effectiveness:** Track the performance of different marketing channels and campaigns to identify what's working and what's not. 
   * **Optimize ad spend:** Use data to allocate marketing budgets more effectively and target the right audience with the right message. 
   * **Personalize marketing messages:** Tailor marketing communications to individual customer segments based on their preferences and behavior. 



Improving Website Performance:
Optimize website layout and design: Analyze user behavior on your website to identify areas for improvement in terms of navigation, layout, and user experience. 
Streamline the checkout process: Identify bottlenecks in the checkout process and optimize it for faster and smoother transactions. 
Improve product recommendations: Use data to personalize product recommendations based on customer browsing and purchase history. 
Driving Revenue and Profitability:
Increase conversion rates: Identify and address factors that lead to low conversion rates (e.g., poor website design, confusing navigation, high shipping costs). 
Improve customer retention: Understand why customers are leaving and implement strategies to retain them. 
Optimize pricing strategies: Analyze pricing data and competitor pricing to optimize prices and maximize profitability. 
Improve inventory management: Use data to predict demand and optimize inventory levels to avoid stockouts and reduce holding costs. 
