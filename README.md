# Brazilian-Olist-ECommerce-store-Sales-trend-and-customer-behavior-analysis
![1_1k72mg1_CZvLptX77zzKTg](https://github.com/user-attachments/assets/ea1c9d33-16c2-4010-b136-3f3f3b2e208c)

## Purpose and outcome
### Purpose
- Identify top product category and state by sale
- Analyze sales trends over time to identify seasonality, growth patterns, and sales fluctuations.
- Discover customer behavior
- Uncover insights to help improve customer retention, marketing targeting, and sales strategy.
- Segment customers based on purchasing habits and engagement.

### Outcomes
- Identify top 5 product_catergory and state has most sales
- Identified peak sales periods and trends by day, week, and month.
- Visualized repeat purchase behavior and retention rates over time.
- Performed RFM segmentation to group customers into categories 
- Provide actionable recommendations for increasing retention and repeat customer rate
  
## 1. Introduction

Olist is a Brazilian e-commerce platform that connects small and medium-sized businesses to customers across Brazil. The platform operates as a marketplace, where merchants can list their products and services and customers can browse and purchase them online.
## 2. Data source

●	**Source**: Public dataset from Kaggle (Brazilian E-Commerce Public Dataset by Olist: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce/data?select=olist_order_items_dataset.csv)

## 3. Data overview

●	Description:  This is a Brazilian ecommerce public dataset of orders made at Olist Store. The dataset has information of 100k orders from 2016 to 2018 made at multiple marketplaces in Brazil. Its features allows viewing an order from multiple dimensions: from order status, price, payment and freight performance to customer location, product attributes and finally reviews written by customers. It also released a geolocation dataset that relates Brazilian zip codes to lat/lng coordinates.

●	Structure:

1.	order_items.csv
   
-**Order_id** : order unique identifier
-	**order_item_id**: sequential number identifying number of items included in the same order.
-	**product_id**: product unique identifier
-	**seller_id**: seller unique identifier
-	**shipping_limited_date**: Shows the seller shipping limit date for handling the order over to the logistic partner.
-	**price**: item price
-	**freight_value**: item freight value item (if an order has more than one item the freight value is splitted between items)


2.	customers_data.csv
   
-	**customer_id** : key to the orders dataset. Each order has a unique customer_id.
-	**customer_unique_id** : unique identifier of a customer.
-	**customer_zip_code_prefix** : first five digits of customer zip code
-	**customer_city** : customer city name
-	**customer_state** : customer state


3.	order_payments.csv
   
-	**Order_id** : unique identifier of an order.
-	**payment_sequential** : a customer may pay an order with more than one payment method. If he does so, a sequence will be created to
-	**payment_type**: method of payment chosen by the customer.
-	**payment_installments** : number of installments chosen by the customer.
-	**payment_value**: transaction value.


4.	order_reviews.csv
   
-	**Order_id** : order unique identifier
-	**review_id**: unique review identifier
-	**review_score**: Note ranging from 1 to 5 given by the customer on a satisfaction survey.
-	**review_comment_title** : Comment title from the review left by the customer, in Portuguese.
-	**review_comment_message**: Comment message from the review left by the customer, in Portuguese.
-	**review_creation_date**: Shows the date in which the satisfaction survey was sent to the customer.
-	**review_answer_timestamp**: Shows satisfaction survey answer timestamp.


5.	orders_dataset.csv
- **Order_id** : unique identifier of the order
-	**customer_id**: key to the customer dataset. Each order has a unique customer_id.
-	**order_status**: Reference to the order status (delivered, shipped, etc).
-	**order_purchase_timestamp**: Shows the purchase timestamp.
-	**order_approved_at** : Shows the payment approval timestamp.
-	**order_delivered_carrier_date**: Shows the order posting timestamp. When it was handled to the logistic partner.
-	**order_delivered_customer_date**:: Shows the actual order delivery date to the customer.
-	**order_estimated_delivery_date**: Shows the estimated delivery date that was informed to customer at the purchase moment.**
   

6.	products_dataset.csv
   
-	**product_id**: unique product identifier
-	**product_category_name**: root category of product, in Portuguese.
-	**product_name_length** : number of characters extracted from the product name.
-	**product_description_length**: number of characters extracted from the product description.
-	**product_photo_qty**: number of product published photos
-	**product_weight_g** : product weight measured in grams.
-	**product_length_cm**: product length measured in centimeters.
-	**product_height_cm**: product height measured in centimeters
-	**product_width_cm**: product width measured in centimeters



7.	product_category_name_translation.csv

-	**product_category_name** : category name in Portuguese
-	**product_category_name_english**: category name in English


## Technologies Used

* Google Colabs: using Python to load data, clean data and visualize heatmap to check correlation
  
* Power Bi: visualizing the dataset.

# Process

## Steps on Google colab

## 1. Load data

Firsly, I download the dataset from Kaggle on the above link. Then, I upload it on Github to get link and use code below to load data

``` python
customer_data = pd.read_csv('https://raw.githubusercontent.com/MinhThuanPhung/Brazilian-E-Commerce/refs/heads/main/olist_customers_dataset.csv')
order_items = pd.read_csv('https://raw.githubusercontent.com/MinhThuanPhung/Brazilian-E-Commerce/refs/heads/main/olist_order_items_dataset.csv')
order_payments = pd.read_csv('https://raw.githubusercontent.com/MinhThuanPhung/Brazilian-E-Commerce/refs/heads/main/olist_order_payments_dataset.csv')
order_review = pd.read_csv('https://raw.githubusercontent.com/MinhThuanPhung/Brazilian-E-Commerce/refs/heads/main/olist_order_reviews_dataset.csv')
orders_dataset = pd.read_csv('https://raw.githubusercontent.com/MinhThuanPhung/Brazilian-E-Commerce/refs/heads/main/olist_orders_dataset.csv')
product_dataset = pd.read_csv('https://raw.githubusercontent.com/MinhThuanPhung/Brazilian-E-Commerce/refs/heads/main/olist_products_dataset.csv')
product_name_translation = pd.read_csv('https://raw.githubusercontent.com/MinhThuanPhung/Brazilian-E-Commerce/refs/heads/main/product_category_name_translation.csv')
```
## 2. Data Cleaning and Exploratory Data Analysis (EDA)
## 2.1 Data cleaning
### 2.1.1 Remove unesscessary column
There are some columns, we may not use them, so I delele it

- Remove columns : product_name_lenght','product_description_lenght','product_photos_qty','product_length_cm','product_height_cm','product_width_cm' in product_datase

- Remove column review_comment_title', 'review_comment_message and review_id in review_order
### 2.1.2 Handling missing value

There are missing values in orders_dataset, product_dataset and order_review.

- Remove row has missing value in orders_dataset
- fill missing value in product_weight by median
- Remove row have missing value in product_category_name of product_dataset

### 2.1.3 Join table
#### product_dataset and product_name_translation
Delete product_name_translation after merge table
####  order_review and orders_dataset
- Recreate Order_review table, group by order_ID, review_score = average review_score of this order, get the fist line of review+creation_date and review_answer_timestamp if order has been reviewed more than 1
- After merging table, delete order_review
#### orders_dataset, order_items and order_payment
For these table, we should not join them, instead of that, we keep order_ID which have in all tables
```python
# filter common order_id in 2 table
common_order_ids = set(orders_dataset['order_id']) & set(order_payments['order_id']) & set(order_items['order_id'])
```
```python
# filter common order_id in  orders_dataset
orders_dataset = orders_dataset[orders_dataset['order_id'].isin(common_order_ids)]
```
```python
# filter common order_id in order_payments and order_items
order_payments = order_payments[order_payments['order_id'].isin(common_order_ids)]
```

```python
order_items = order_items[order_items['order_id'].isin(common_order_ids)]
```
### 2.1.4 Fixing data types
- Convert automaitcally data type (Most of them change from object to string) and conver column has date to datetime type
- Change shipping_limit_date type from object to date_time
- Convert date columns to datetime type
### 2.1.5 Adding columns
####  Adding column in dataframe orders_database
There are 3 columns need to be added in dataframe orders_database

- delivert_time : Total time which customer received package, it calculated from order approved until it actually delivered to customer
- Estimated_delivery_time: time from order approved until it estimated delivered
- handling_time : calculate from order approved until it delivered to shiping carrier
- purchase_hour : which show the time customer place order
- weekday : show the days in week customer placed order
- time_slot : classify purchase_hour into 7 types
   * late night : if 0 <= hour < 6
   * Early Morning : 6 <= hour < 9:
   * Morning :  9 <= hour < 12:
   *  Noon : 12 <= hour < 14 
   * afternoon : 14 <= hour < 17:
   * Evening Peak : 17 <= hour < 20
   * Late Evening : after 20
There are errors in data, so result may < 0 sometime, flexiblity, we can use order_pusechased_timestamp instead of order_approved at

Then convert data type after create new columns
####  Adding columns in order_items

Add 2 columns in order_items

- Total_value = price + freight_value. Total amount of order
- total_item_order = get max order_item_id of order. Which show total items in order

####  Adding column in Customer_data
- add column region : show the region of customer. Clasify customer location from state into 5 region
``` python
# classify customer location by state
# southest = ['SP', 'RJ', 'ES','MG']
# northest= ['MA', 'PI', 'CE', 'RN', 'PE', 'PB', 'SE', 'AL', 'BA']
# north =  ['AM', 'RR', 'AP', 'PA', 'TO', 'RO', 'AC']
# midwest = ['MT', 'GO', 'MS' ,'DF' ]
# south = ['SC', 'RS', 'PR']

state_to_region = {
    # Southeast
    'SP': 'Southeast', 'RJ': 'Southeast', 'ES': 'Southeast', 'MG': 'Southeast',
    # Northeast
    'MA': 'Northeast', 'PI': 'Northeast', 'CE': 'Northeast', 'RN': 'Northeast',
    'PE': 'Northeast', 'PB': 'Northeast', 'SE': 'Northeast', 'AL': 'Northeast', 'BA': 'Northeast',
    # North
    'AM': 'North', 'RR': 'North', 'AP': 'North', 'PA': 'North',
    'TO': 'North', 'RO': 'North', 'AC': 'North',
    # Midwest
    'MT': 'Midwest', 'GO': 'Midwest', 'MS': 'Midwest', 'DF': 'Midwest',
    # South
    'SC': 'South', 'RS': 'South', 'PR': 'South'
}

customer_data['region'] = customer_data['customer_state'].map(state_to_region)
```
#### descriptive statistics of dataframe
- There are payment value = 0, replace by total value with corresponding orderID in order_items dataframe
- There are some order with total_item_order = 0, replace by median 
#### 2.1.6. Download cleaned_CSV to import PowerBI and make a dashboard

## 2.2 EDA 
### 2.2.1 Sales correlation
#### Sales and other factors correlation
##### Creating a new dataframe for checking correlation

- df is dataframe which order_items join in product_dataset

- df1 is dataframe which order_dataset join in customer_data

- df2 is dataframe whwich df1 join in order_ítems

- sales_df is dataframe which merge all tables together to check correlation between variables


#####  creating sales_df
``` python
# merge order_items with product_dataset
df = order_items.merge(product_dataset, on='product_id', how='left')
```
``` python
sales_df = df.groupby('order_id').agg({
    'price': 'mean',
    'total_item_order': 'first',
    'freight_value': 'mean',
    'product_weight_g': 'mean',
    'total_value': 'sum'

}).reset_index()
```

``` python
# merge customer_data with orders_dataset - df1 then merge it with grouped_df
df1 = orders_dataset.merge(customer_data, on='customer_id', how='left')
sales_df = sales_df.merge(df1, on='order_id', how='outer')
```
##### cleaning sales_df
- fill Na of product_weight by median
- Remove unessary columns : customer_zip_code_prefix, customer_city, order_delivered_carrier_date, order_delivered_customer_date, order_estimated_delivery_date , review_answer_timestamp , review_creation_date , order_approved_at
- checking describe sales
- Convert Categorical data to Numerical type : weekdat, time_slot and region columns need to be change. Some has float type, need change to int
``` python
sales_df.describe()
```
| Thống kê         | price   | total_item_order | freight_value | product_weight_g | total_value | review_score | delivery_time |
|------------------|---------|------------------|----------------|------------------|--------------|----------------|----------------|
| **count**        | 95814.0 | 95814.000000     | 95814.0        | 95814.0          | 95814.0     | 95814.0       | 95814.0        |
| **mean**         | 125.04  | 1.14             | 20.14          | 2080.81          | 159.56      | 4.16          | 12.05          |
| **std**          | 188.31  | 0.53             | 15.68          | 3720.76          | 217.50      | 1.28          | 9.43           |
| **min**          | 0.85    | 1.00             | 0.00           | 0.00             | 9.59        | 1.0           | 0.00           |
| **25% (Q1)**     | 41.90   | 1.00             | 13.37          | 300.0            | 61.80       | 4.0           | 6.00           |
| **50% (Median)** | 79.00   | 1.00             | 16.35          | 700.0            | 105.28      | 5.0           | 10.00          |
| **75% (Q3)**     | 139.90  | 1.00             | 21.15          | 1800.0           | 176.16      | 5.0           | 15.00          |
| **max**          | 6735.00 | 21.00            | 409.68         | 40425.0          | 13664.08    | 5.0           | 208.00         |


Depending table above,

- I can see that shipping_weight = 0 in some orders, we need to replace this value by meadian
- replace delivery time with 0 value by median




```python
correlation_matrix_sale = sales_df.corr()
correlation_matrix_sale.head()

plt.figure(figsize=(20, 15))
sns.heatmap(correlation_matrix_sale, annot=True, cmap='coolwarm')
plt.title('Sales Correlation Matrix Heatmap')

plt.show()
```
![tải xuống (4)](https://github.com/user-attachments/assets/060ce184-bdb6-4048-b97e-da001c5fd696)


### 💡Insights

🔥 Strong Correlations 

- price and total_value show a very strong positive correlation: 0.92. This makes sense, as price is a major component of the total order value. The more expensive items sold, it contribute more to total revenue

- product_weight_g and freight_value: 0.62. Heavier products tend to incur higher shipping costs.

- product_weight_g and total_value: 0.46. Heavier products are often more expensive, contributing to a higher total value.


### 2.2.2 Retained Customer correlation

##### creating retained_corr_matrix dataframe 

``` python
# count number of order by each customer
order_counts = df2.groupby('customer_unique_id').size()

# Khách retained nếu mua > 1 lần
retained_customers = order_counts[order_counts > 1].index

# label retained
df2['is_retained'] = df2['customer_unique_id'].isin(retained_customers).astype(int)
```


```python
retained_df = df2.groupby('customer_unique_id').agg({
    'is_retained': 'first',
    'order_id': 'count',
    'total_item_order': 'first',
    'price': 'sum',
    'freight_value': 'sum',
    'review_score': 'mean',
    'handling_time': 'mean',
    'region': 'first'

}).rename(columns={'order_id': 'total_orders'})
```
```python
retained_corr_matrix = retained_df.corr()
plt.figure(figsize=(8, 6))
sns.heatmap(retained_corr_matrix, annot=True, cmap='coolwarm', fmt=".2f")
plt.title("Customer retention Correlation Heatmap", fontsize=14)
plt.show()
```
![tải xuống](https://github.com/user-attachments/assets/a0511451-1998-41ac-8a34-c59f9a2e2af0)


**💡Insight**

- total_orders with customer retention (0.77): Customers with more total orders are more likely to be retained.

- total_item_order with customer retention (0.70): Customers ordering more items are more likely to stay.

- freight_value with customer retention (0.37): Higher shipping value has a moderate positive correlation with retention.
- review_score has a moderate negative correlation with delivery_time (-0.33), which makes sense—longer delivery often leads to lower review scores.
## Process in Power BI
- Create a new table named calendar show all date from all dataframe:  Calendar = CALENDARAUTO()
- Create a table name payment_distinct which each orderid has one row only, get max of payment_sequential and sum of payment_installments from table order_payments
- Create  a quick measure : there are 2 columns, revenue for new customer_unique_id (revenue from new customer) - use quick measure to create it, and column revenue from old customer = Total revenue - revenue for new customer_unique_id
- Create meausure name sale-cus metric which include meausure below
   * AOV by region
   * AOV by category
   * Average order value
   * AvgReviewByCategory
   * New Customers
   * New Customers LY(last year)
   * Num Old customer = [Total customer] - [New Customers]
   * Previous Year Orders : total order previous year
   * Repeat customer rate = DIVIDE([Num Old customer],[Total customer],0)
   * revenue per customer = DIVIDE([Total_revenue], [Total customer],0)
   * revenue Previous Years
   * Total customer = DISTINCTCOUNT(customer_data_clean[customer_unique_id])
   * Total Customers LY (last year)
   * Total orders = COUNT(orders_dataset_clean[order_id])
   * Total_revenue = SUM( order_items_clean[total_value])
- Create table named CustomerFirstOrderMonth which have customer_unique_id and firspurchaseMonth (first month customer purchase order)
- Create a meausre named RetainedCustomers show number of retained customer, measure named NewCustomers, and RetentionRate = DIVIDE([RetainedCustomers], [NewCustomers])
- In table order_dataset create columns corhortMonth and CohortIndex
- Finally check relationship all tables and make a relationship if some does not have it yet.

## 📊 Visualize and analysis
## 1. Sales trend

![image](https://github.com/user-attachments/assets/b402d220-2c01-450f-b9ce-e88d718a5530)


**Fig.1:** Sales Dashboard

 In general, revenue and total order number inscreased through the year, in 2018 revenue each moth increased double compare with 2017. Notiblely, sales increase in end of the year 2017 in Nov, Dec and also Jan. Peak season in these months
![z6614621357829_72b2a9c92a203176ad3308c9b5c8fffb](https://github.com/user-attachments/assets/b1cabbe6-ac3f-466e-b249-523fa8925e7e)
**Fig.2:** Revenue and orders

Data shows order in Olist store from Oct 2016 until  Aug 2018. Through these years:
- Total revenue = 15.29 million USD
- Total orders = 95.81K orders
- Average order value = 159.56 USD

![z6614672286276_10b118fe55b6ec4c37d3606d41d861ac](https://github.com/user-attachments/assets/dc0d6bfb-7ee0-4fa7-ae72-ce2ec0d61b30)

Notably, data show only 8 months in 2018 however, revenue increased 22.4% , total orders increased 21.77% compared with 2017


![z6614696170162_95c7da0a42d8a86b54a85edd95f6ff65](https://github.com/user-attachments/assets/3c66d22c-3223-4003-a632-60347293ac01)
**Fig.3:** Top 5 product catergories by revenue

Top 5 product catergories have the highest revenue repectively
- Bed_bad_table : 
- health_beauty : , it sold 5800 items which the top 1 the best selling in 2018.
- Sport_lesure
- computer accessories
- watch_gift
 

This below picture is the reverse: the bottom 10 product groups with the lowest performance in their category
![z6614740006804_eff3e99899411c3836a594a61a0d8da3](https://github.com/user-attachments/assets/e0e73339-a6fb-417d-b99e-6f9369ec86b9)
**Fig.5:** Top 10 best selling group_product

Revenue by new customer accounts for the majority revenue (average 99.9% in 3 years)
![image](https://github.com/user-attachments/assets/d344dbff-7198-4c89-8c05-e25ae7259f81)

Top 5 citys have highest revenue: Include SP, RJ, MG, RS, PR
- SP also contribute the higest revenue, which account 30% total revenue in 2018 increase 10% compared with 2017 market share. Only 5 states above account for more than 50% total revenue of all state (2018). This number is around 45% in 2017
![image](https://github.com/user-attachments/assets/15e56f1f-c2b0-4235-b4df-d9d4187506bc)


**Revenue by day in week and time**
Orders placed in monday and tuesday bring more revenue than other days in week. Whiles, afternoon and late night also bring more revenue than other time in a day. 

![image](https://github.com/user-attachments/assets/4db2ad7b-a3c5-47d7-96e2-72681ae5b502)

**Revenue YTD**

- The Total_revenue YTD shows a steady upward trend throughout both 2017 and 2018.

- Revenue accumulates consistently over the months, indicating stable sales performance during the two years.

![image](https://github.com/user-attachments/assets/a21f30ac-b1b2-487e-959d-f91d2e990212)


**🔍 What do we see through the charts?**
- ✅ Revenue and total order number inscreased year by year, this is because Olist was enstablished in 2015, in period 2017., 2018, system maybe more stable and more popular so more orders. Moreover, in 2018 ecommerce in Brazil become more recognized so people buying product via e-market more.
- ✅ Through the year, Nov,Dec and Jan are the month have most sales, it is maybe due to the holiday season of the year, when people tend to buy more to prepare for big holiday.
- ✅ Orders placed in monday and tuesday bring more revenue than other days in week. Whiles, afternoon and late night also bring more revenue than other time in a day
- ✅ Top 5 states have highest revenue : SP, RJ, MG, RS, PR, only 5 these states but account for half of total revenue
- Bed_bad_table, health_beauty, Sport_lesure, computer accessories, watch_gift these product groups have highest revenue and demand in this period. Only 5 products group but account for approximately 55% total revenue. In this list, health_beauty contributed sinificantly in total revenue.
- ✅ Most revenue is contribued by new customer (about 99%)

**🤔What caused sales increased in those years?**

There are only 2 months in 2016 in this data, So trend focuses on 2017 and 2018_
![image](https://github.com/user-attachments/assets/3697d769-812a-4b1e-bd13-c5d960bcda68)

1.  Number of product increased sharply . In 2018, there are total 20090 products , increased 20.22% compared with 2017.
2. Total about 60k items sold in 2018 compare wih 49K in 2017
3. New customer in 2018 increase 21.21% compared with 2017
4. The top 2 best-selling product groups have seen a significant expansion in their product catalogs compared with 2017. They are  are beauty healthy( 61.03%) , bed_bath_table (17.69%), computer_accesories (12.44%) while average all product is 1.1% . The demand for these products also increase notably  beauty healthy( 62.93%) ,  computer_accesories (53.84%)
5. 
![image](https://github.com/user-attachments/assets/2c7ace13-4b87-4a5f-82d6-96d910bd2aa1)

   
**🤔Why sales increase sharply in Nov 2017**

![image](https://github.com/user-attachments/assets/e83a9776-95fd-465a-a399-5130274bf379)
![image](https://github.com/user-attachments/assets/99f257ae-f381-48e0-82a2-8f86da89adbc)


1. Number of product increased significantly from , 2731 in September, 2917 in October to 4371 in Novemeber. Item solds in this month increased tp 8400 items sold (growed 62% compared with october)
2. New customers are 7011 Customers in November , increase 63% compared with the last month.
3. Total product increased from 2917 product in Oct, to 4371 products in Nov
4. There was an increase high demend for these product beauty healthy (76.58%,  bed_bath_table  (101%)
![image](https://github.com/user-attachments/assets/0456e9b3-a8df-478d-887c-c86aade78827)


## 2. Customer behavior

Depending data, we can see that only 3.49% customer is old customer (who already bought at least 2 order), while new customer is 93K customer which account for 96.51% total customer in three years
Average review_score is 4.16 (5 is the highest score)

![image](https://github.com/user-attachments/assets/86e419bc-d2f4-496a-b238-d6239a6464ae)

### State metrix
![image](https://github.com/user-attachments/assets/8d6ce9e5-9262-4add-9ee1-09628cdedc48)

- Table show number of new customer,orders,  repeat customer rate, revenue per customer, average score, 

- SP, RJ, MG, RS, PR are top 5 state which has the highest customer, order, new orders. 

- Meanwhile, PB, AC,AP, RO, AL are top 5 states have the highes revenue per customer. Customer in these state paid for one order more than other state

- RR, RJ, CE, MA, PI are top 5 state has highest repeat customer rate

![image](https://github.com/user-attachments/assets/e6d3e985-c747-484b-a6b9-bc5aa251163a)


Top 5 best selling item by state PR Bed_bad_table, Sport_lesure, furniture decor, health_beauty, computer accessories

![image](https://github.com/user-attachments/assets/fe4ee3d7-1460-4162-9777-cbff44a02e50)

Top 5 best selling item by state SP Bed_bad_table, Sport_lesure, furniture decor, health_beauty, houseware
![image](https://github.com/user-attachments/assets/c4f42861-d642-4980-bfb1-1ab393b6c8ae)


### catergory metrix 

Top 5 best selling product category, 
- Bed_bad_table (number 1)
- Sport_lesure
- furniture decor
- health_beauty
- houseware
These products are the most preferable by customer since they are sold the most.

![image](https://github.com/user-attachments/assets/82fc0365-223f-4fc4-b7e8-b091cbf9a9f6)

Top 5 best selling product in Nov

![image](https://github.com/user-attachments/assets/fbba0caa-333d-4649-916e-06eeba4b5447)


### Payment and time

Customer prefer use credit card which account for  74% in total all payment method. The second rank is boleto. When customer use credit card, average installments is 3.5, while other payment method is around 1 time

![image](https://github.com/user-attachments/assets/35c2c675-d057-4302-9f1b-8ee2bd6bb84f)

Most of customer paid one time instead of paid many time in one order. 
![image](https://github.com/user-attachments/assets/008dd461-4861-4ba4-b159-653a7e653ed0)

![image](https://github.com/user-attachments/assets/a87d4e69-efe0-404f-a9a3-fb1485fbee6e)


Customer tend to buy product in late evening and afternoon. More order was bought in monday and tuesday

### Retention
![image](https://github.com/user-attachments/assets/474f8094-e8ca-49c7-98e6-81f8381f7f03)

- Retention Decline Over Time, A sharp drop-off after Month 0 is visible across all cohorts.
- September 2017 and August 2017 have the highest retention into later months (e.g., 0.52% at Month 5 for August 2017).These cohorts consistently retain more customers longer than others.
- Retention at Month 6 and beyond is very low (under 0.2%) in most cases. Indicates that customers are rarely returning past half a year after first purchase.
- Most customers make only one or two purchases, with retention falling sharply.
- Very few customers remain active beyond 3-5 months.


**🔍 What do we see through the charts?**

- ✅ Majority order has 5 start rate, however the repeat customer is quite low around 3.49%.
- ✅ Majority customers located in SP, RJ, MG, RS, PR but most of them buy one order only (has low repeat customer rate) While, RR, RJ, CE, MA, PI are top 5 state has highest repeat customer rate, they do not have many customer however, but customer paid for each order higher than top 5 states have highest customer number and higher than average order value in total.
- ✅ Customer prefer to buy these product in olist Bed_bad_table, Sport_lesure, furniture decor, health_beauty, houseware
- ✅ Customer prefer to use credit card to pay order, in average, customer usually prefer 3 insallment when they paid orders. Most Customer pays once for the order.
- ✅ Buyer tend to buy product in late night or afternoon, it's possiblely because at that time, most people done work or most works are complete so they can spend time on internet and do shopping online. Most orders were placed on monday and tuesday, which shows that customer prefer to order in weekday, in beginning of the week.
- ✅ Retention decline over the time with low rate in all cohort month. This suggests issues with long-term engagement, possibly due to: lack of loyalty incentives or Low purchase frequency by nature of the product or Poor follow-up marketing or customer experience.
- ✅ Bases on heatmap, We can see the relationship betwween retained customer with total item order is quite high. It notibaly shows that customer buy more item in order will have higher rate to buy more.
- ✅ Heatmap show negative relationship betwween delivery time and review_score. It means that customer tend to give higher score if order has short delivery time.

## 3.  RFM Analysis

RFM stands for Recency - Frequency - Monetary Value, I will be using this metrics to segment customers
Since the retention and rate of repeat customer is quite low, it is possiblely show that the loyalty program for customer work ineffectively. We need to clasify customer to run customize programe to win them back. 
### 3.1 create RFM dataframe

df3 was created by merging df1 with order_payment
Grouping by customer_unique_id to create rfm_df
``` python
# Create RFM dataframe which group by customer_unique_id, max order_purchase_timestamp,
rfm_df = df3.groupby("customer_unique_id").agg({
    # Calculate Recency using the recent_order_timestamp and the maximum timestamp for each customer

    'order_purchase_timestamp': lambda x: (pd.Timestamp(recent_order_date) - x.max()).days,
    'order_id': 'count',
    'payment_value': 'sum'
}).reset_index()
rfm_df.columns = ['customer_unique_id', 'Recency', 'Frequency', 'Monetary']
```
rfm_head()

| customer_unique_id                       | Recency | Frequency | Monetary |
|------------------------------------------|---------|-----------|----------|
| 0000366f3b9a7992bf8c76cfdf3221e2         | 111     | 1         | 141.90   |
| 0000b849f77a49e4a4ce2b2a4ca5be3f         | 114     | 1         | 27.19    |
| 0000f46a3911fa3c0805444483337064         | 537     | 1         | 86.22    |
| 0000f6ccb0745a6a4b88665a16c9f078         | 321     | 1         | 43.62    |
| 0004aac84e0df4da2b147fca70cf8255         | 288     | 1         | 196.89   |

create column R_score, F_score, M_score depending%

```python

# create column R_score, F_score, M_score depending%
rfm_df['R_score'] = pd.qcut(rfm_df['Recency'], 5, labels=[5, 4, 3, 2, 1])
rfm_df['F_score'] = pd.qcut(rfm_df['Frequency'].rank(method='first'), 5, labels=[1, 2, 3, 4, 5])
rfm_df['M_score'] = pd.qcut(rfm_df['Monetary'], 5, labels=[1, 2, 3, 4, 5])

# Create RFM segment
rfm_df['RFM_Score'] = rfm_df['R_score'].astype(str) + rfm_df['F_score'].astype(str) + rfm_df['M_score'].astype(str)
```
We have table 


| customer_unique_id                       | Recency | Frequency | Monetary | R_score | F_score | M_score | RFM_Score |
|------------------------------------------|---------|-----------|----------|---------|---------|---------|------------|
| 0000366f3b9a7992bf8c76cfdf3221e2         | 111     | 1         | 141.90   | 4       | 1       | 4       | 414        |
| 0000b849f77a49e4a4ce2b2a4ca5be3f         | 114     | 1         | 27.19    | 4       | 1       | 1       | 411        |
| 0000f46a3911fa3c0805444483337064         | 537     | 1         | 86.22    | 1       | 1       | 2       | 112        |
| 0000f6ccb0745a6a4b88665a16c9f078         | 321     | 1         | 43.62    | 2       | 1       | 1       | 211        |
| 0004aac84e0df4da2b147fca70cf8255         | 288     | 1         | 196.89   | 2       | 1       | 4       | 214        |

### 3.2 Clasify customer type based on RMF score
We clasify customer:
 def rfm_segment(row):
    r = int(row['R_score'])
    f = int(row['F_score'])
    m = int(row['M_score'])

    if r >= 4 and f >= 4 and m >= 4: Champions : These are the best customers. Customers who bought very recently (high Recency score). Buy very frequently (high Frequency score) and spend a lot (high Monetary score)
    
    elif f >= 4 and r >= 2: Loyal Customers : These customers are consistent buyers and fairly recent but may not have spent as much or very recently as Champions. They are valuable and likely to stick around. They buy frequently (Frequency score is high) and purchased somewhat recently
        
    elif r == 5 and f <= 2:'New Customers': These customers just made their first or second purchase. They’re new to your brand and could become loyal with good engagement. They purchased very recently and have only bought once or twice
  
    elif r <= 2 and f >= 3:'At Risk' : These customers used to be active but haven’t returned lately. They need attention. They haven’t purchased recently but they used to buy frequently

    Rest:  'Low-Value': These customers don’t contribute much currently or are inactive.

rfm_df['Segment'] = rfm_df.apply(rfm_segment, axis=1)

```python
customer_data1 = customer_data.groupby('customer_unique_id').first().reset_index()
rfm_df = rfm_df.merge(customer_data1, on='customer_unique_id', how='left')
```

```python
rfm_df.head()
```
| customer_unique_id                    | Recency | Frequency | Monetary | R_score | F_score | M_score | RFM_Score | Segment    | customer_state | region    |
|-------------------------------------|---------|-----------|----------|---------|---------|---------|-----------|------------|----------------|-----------|
| 0000366f3b9a7992bf8c76cfdf3221e2    | 111     | 1         | 141.9    | 4       | 1       | 4       | 414       | Low-Value  | SP             | Southeast |
| 0000b849f77a49e4a4ce2b2a4ca5be3f    | 114     | 1         | 27.19    | 4       | 1       | 1       | 411       | Low-Value  | SP             | Southeast |
| 0000f46a3911fa3c0805444483337064    | 537     | 1         | 86.22    | 1       | 1       | 2       | 112       | Low-Value  | SC             | South     |
| 0000f6ccb0745a6a4b88665a16c9f078    | 321     | 1         | 43.62    | 2       | 1       | 1       | 211       | Low-Value  | PA             | North     |
| 0004aac84e0df4da2b147fca70cf8255    | 288     | 1         | 196.89   | 2       | 1       | 4       | 214       | Low-Value  | SP             | Southeast |


Pie chart show Customer Segmentation

![image](https://github.com/user-attachments/assets/84e84d86-5961-46f7-bdb4-ceea9ac1ada3)


#### **👶👵Customer Segment Characteristics**

![image](https://github.com/user-attachments/assets/1a3b07d8-e17d-4875-bbf9-cfac5acda467)

**1. Champion**
   
   ![image](https://github.com/user-attachments/assets/486756eb-2588-4ea3-bb3f-f872cf523d15)

  -  Top 5 best selling products- the most preferable products of this type: bed bath table, health_beauty, watch_gifts, furniture_decor, housewares
  -  Champion customer likely to place order on monday and Thurday and in all time of the day but just few on late night and early morning
  -  Average payment installments is 4 higher than average which show that champion customer tend to pay order with many installments.
  -  They also have the highest average on order - show amount they paid for one order. This team has great potential in the high-end product segment. Since amount they paid for one order is highest.
    
**3. Loyal customers**

Top product which has grow sold > 150% and total sold items > 700 items In 2018 

![image](https://github.com/user-attachments/assets/587234e1-051f-4934-86cf-df12a61ca40e)
- Loyal Customers: Top 5 the most selected products bed bath table, health_beauty,sport_leisure, computer_accessories, furniture_decor
- Potential products: telephony, watch_gift
- Loyal customer usually place order on monday and tuesday and in late evening and afternoon
- Avg delivery time is higher than average. This indicates that it need to be improve to improve review_score and customer experience\
- Amount they pay for one order is lowest. It is possible that competitive price will need to focus on this team
  
**4. New customers****

   ![image](https://github.com/user-attachments/assets/23eced17-2752-41ce-b19b-35a8d427c359)

   
   - Top 5 the most selected products health_beauty, bed bath table, housewares, sport_leisure, watch_gifts
  - Many new customers place order on Tuesday and wednesday and in all time of the day accept late night or early morning are less
  - Most of package delivered to this type of customer faster than usual --> review score is highest among
  - 
**5. At risk**

![image](https://github.com/user-attachments/assets/302f2836-edda-411c-8b2c-4ef5da0f7c43)

- Top 5 the most selected products bed bath table, health_beauty, sport_leisure, furniture_decor, computer_accessories
- None of them have order in 2018, so base on data on 2017, we can see that bad_bath_table has most request of 2017 and demand compare with 2016 is notible
- Avg delivery time is higher than average. it also connects with review_score. So delivery time need to be impove
  
**6. Low value**

Top product which has grow sold > 150% and total sold items > 700 items In 2018 

![image](https://github.com/user-attachments/assets/284ea96c-e165-4d64-be90-3860c1b21243)

- Low-Value:Top 5 best selling products bed bath table, furniture_decor, sport_leisure,health_beauty, housewares
- Watch_gift and electronic are potential products for this type of customer
- Avg delivery time is higher than average, it also connects with review_score. So delivery time need to be improve
  
All customer prefer to pay order by credit card and boleto

#### Recommendation

Olist was establish in 2015, in period 2015-2018 is good to make new customer, but after this period, the more important that we need to retain customer then make a loyal customer


- Classify customer into 7 groups : Champions, Loyal, Potential Loyalist, New, At Risk, Hibernating, Others. Each notible state, depend on rate of customer type and Top 5 best selling product_category in this state to suggest suitable promotion to add or advertiment for each state. Focus on 5 states has the highest revenue/solds and 5 states have  highest average per order (state customer willing to pay more in each order)
- For example: for SP state - in top 5 higest revenue state, with champion customer. We can stimulate customer comsumption buy providing coupon for these group product such as : watch_gift, healthy_beauty. And more advertisement for this kind of product.
- Retention Programs: We can send email with coupon for second order,  cross-sell base on type the most selling product in customer state and product in the last orders. Marketing email should be sent in afternoon or late night since this time customer active the most.
- There is strong positive correlation betwween customer retention with total item in order. Customer likely buy many items will become loyal customer. So we can consider make a combo product more (cross-sell), for example, beauty_healthy is the one has very high demand in 2017 and sharply increased in 2018 - then become the top 1, So these products potentially are preferable by . We can make a combo such as micella water with makeup products.


