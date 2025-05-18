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
```
