# Brazilian-Olist-ECommerce-store-Sales-trend-and-customer-behavior-analysis
![tải xuống](https://github.com/user-attachments/assets/bb89186a-7ae7-4769-a5a3-74dff11313a3)

## 1. Introduction

Olist is a Brazilian e-commerce platform that connects small and medium-sized businesses to customers across Brazil. The platform operates as a marketplace, where merchants can list their products and services and customers can browse and purchase them online.
## 2. Data source

●	Source: Public dataset from Kaggle (Brazilian E-Commerce Public Dataset by Olist: https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce/data?select=olist_order_items_dataset.csv)

## 3. Data overview

●	Description:  This is a Brazilian ecommerce public dataset of orders made at Olist Store. The dataset has information of 100k orders from 2016 to 2018 made at multiple marketplaces in Brazil. Its features allows viewing an order from multiple dimensions: from order status, price, payment and freight performance to customer location, product attributes and finally reviews written by customers. It also released a geolocation dataset that relates Brazilian zip codes to lat/lng coordinates.

●	Structure:

1.	order_items.csv
   
○	Order_id : order unique identifier

○	order_item_id : sequential number identifying number of items included in the same order.

○	product_id: product unique identifier

○	seller_id: seller unique identifier

○	shipping_limited_date: Shows the seller shipping limit date for handling the order over to the logistic partner.

○	price: item price

○	freight_value: item freight value item (if an order has more than one item the freight value is splitted between items)

2.	customers_data.csv
   
○	customer_id : key to the orders dataset. Each order has a unique customer_id.

○	customer_unique_id : unique identifier of a customer.

○	customer_zip_code_prefix : first five digits of customer zip code

○	customer_city : customer city name

○	customer_state : customer state

3.	order_payments.csv
   
○	Order_id : unique identifier of an order.

○	payment_sequential : a customer may pay an order with more than one payment method. If he does so, a sequence will be created to

○	payment_type: method of payment chosen by the customer.

○	payment_installments : number of installments chosen by the customer.

○	payment_value: transaction value.

4.	order_reviews.csv
   
○	Order_id : order unique identifier

○	review_id: unique review identifier

○	review_score: Note ranging from 1 to 5 given by the customer on a satisfaction survey.

○	review_comment_title : Comment title from the review left by the customer, in Portuguese.

○	review_comment_message: Comment message from the review left by the customer, in Portuguese.

○	review_creation_date: Shows the date in which the satisfaction survey was sent to the customer.

○	review_answer_timestamp: Shows satisfaction survey answer timestamp.

5.	orders_dataset.csv
   
○	Order_id : unique identifier of the order

○	customer_id: key to the customer dataset. Each order has a unique customer_id.

○	order_status: Reference to the order status (delivered, shipped, etc).

○	order_purchase_timestamp: Shows the purchase timestamp.

○	order_approved_at : Shows the payment approval timestamp.

○	order_delivered_carrier_date: Shows the order posting timestamp. When it was handled to the logistic partner.

○	order_delivered_customer_date:: Shows the actual order delivery date to the customer.

○	order_estimated_delivery_date: Shows the estimated delivery date that was informed to customer at the purchase moment.

6.	products_dataset.csv
   
○	product_id: unique product identifier

○	product_category_name: root category of product, in Portuguese.

○	product_name_length : number of characters extracted from the product name.

○	product_description_length: number of characters extracted from the product description.

○	product_photo_qty: number of product published photos

○	product_weight_g : product weight measured in grams.

○	product_length_cm: product length measured in centimeters.

○	product_height_cm: product height measured in centimeters

○	product_width_cm: product width measured in centimeters.


7.	product_category_name_translation.csv

○	product_category_name : category name in Portuguese

○	product_category_name_english: category name in English

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
### 2.1 Remove unesscessary column
There are some columns, we may not use them, so I delele it

- Remove columns : product_name_lenght','product_description_lenght','product_photos_qty','product_length_cm','product_height_cm','product_width_cm' in product_datase

- Remove column review_comment_title', 'review_comment_message and review_id in review_order
### 2.2 Handling missing value

There are missing values in orders_dataset, product_dataset and order_review.

- Remove row has missing value in orders_dataset
- fill missing value in product_weight by median
- Remove row have missing value in product_category_name of product_dataset

### 2.3 Join table
#### 2.3.1 product_dataset and product_name_translation
Delete product_name_translation after merge table
#### 2.3.2 order_review and orders_dataset
- Recreate Order_review table, group by order_ID, review_score = average review_score of this order, get the fist line of review+creation_date and review_answer_timestamp if order has been reviewed more than 1
- After merging table, delete order_review
#### 2.3.3 orders_dataset, order_items and order_payment
For these table, we should not join them, instead of that, we keep order_ID which have in all tables
### 2.4 Fixing data types
- Convert automaitcally data type (Most of them change from object to string) and conver column has date to datetime type
- Change shipping_limit_date type from object to date_time
- Convert date columns to datetime type
### 2.5 Adding columns
#### 2.5.1 Adding column in dataframe orders_database
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
#### 2.4.2 Adding columns in order_items

Add 2 columns in order_items

- Total_value = price + freight_value
- total_item_order = get max order_item_id of order

#### 2.4.3 Adding column in Customer_data
- add column region
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
### 3. Download cleaned_CSV to import PowerBI and make a dashboard

### 4 Creating a new dataframe for checking correlation

- df is dataframe which order_items join in product_dataset

- df1 is dataframe which order_dataset join in customer_data

- df2 is dataframe whwich df1 join in order_ítems

- sales_df is dataframe which merge all tables together to check correlation between variables
- payment_sale_corr dataframe
#### creating sales_df
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

price	total_item_order	freight_value	product_weight_g	total_value	review_score	delivery_time	estimated_delivery_time	handling_time	weekday_Friday	...	time_slot_Evening Peak	time_slot_Late Evening	time_slot_Late Night	time_slot_Morning	time_slot_Noon	region_Midwest	region_North	region_Northeast	region_South	region_Southeast
count	95814.000000	95814.000000	95814.000000	95814.000000	95814.000000	95814.000000	95814.0	95814.0	95814.000000	95814.00000	...	95814.000000	95814.000000	95814.000000	95814.000000	95814.000000	95814.000000	95814.000000	95814.000000	95814.000000	95814.000000
mean	124.441240	1.141274	19.660175	2080.851577	159.086407	4.155416	12.048845	23.1966	2.790521	0.14191	...	0.179807	0.225092	0.047457	0.176394	0.125524	0.058384	0.018567	0.093650	0.143445	0.685954
std	188.376224	0.534098	15.680450	3720.738143	217.495059	1.284155	9.428425	8.766273	3.462421	0.34896	...	0.384028	0.417645	0.212614	0.381157	0.331314	0.234469	0.134991	0.291343	0.350527	0.464137
min	0.000000	1.000000	0.000000	2.000000	9.000000	1.000000	0.0	0.0	0.000000	0.00000	...	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000
25%	41.000000	1.000000	13.000000	300.000000	61.000000	4.000000	6.0	18.0	1.000000	0.00000	...	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000
50%	79.000000	1.000000	16.000000	700.000000	105.000000	5.000000	10.0	23.0	2.000000	0.00000	...	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	1.000000
75%	139.000000	1.000000	21.000000	1800.000000	176.000000	5.000000	15.0	28.0	4.000000	0.00000	...	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000	1.000000
max	6735.000000	21.000000	409.000000	40425.000000	13664.000000	5.000000	208.0	154.0	126.000000	1.00000	...	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000	1.000000
8 rows × 28 columns
Depending table above,

- I can see that product_weight = 0 in some orders, we need to replace this value by meadian
- if handling time < 0 , replace by median


``` python
sales_df.head()
```

#### creating payment_sale_corr dataframe

Create dataframe named payment_sale_corr which based on order_payments dataframe which has additional column named total_Sequantial which show which show total number of payment method customer use to pay for an order. first line show the max payment_sequential but other line of order show value = 0

``` python
# create a column name total_srquential which show total number of payment method customer use to pay for an order. 
# first line show the max payment_sequential but other line of order show value = 0
idx_max = order_payments.groupby('order_id')['payment_sequential'].idxmax()

order_payments['total_sequential'] = 0

order_payments.loc[idx_max, 'total_sequential'] = order_payments.loc[idx_max, 'payment_sequential']
```
``` python
payment_sale_corr = order_payments.drop(columns=['order_id','payment_sequential'],axis=1)
```

##### cleaning payment_sale_corr 
- convert datatype of string/object/catergorical data to numerical data
- convert float to int
``` python
payment_sale_corr.head()
```
