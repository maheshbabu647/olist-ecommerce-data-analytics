# Olist E-Commerce — Data Dictionary

## 1. Dataset overview

The dataset contains nine CSV files covering orders, customers, products, sellers, payments, reviews, and geolocation.

| Dataset | Initial rows | Grain — one row represents |
|---|---:|---|
| orders | 99,441 | One order |
| customers | 99,441 | One customer record |
| order_items | 112,650 | One product item within an order |
| order_payments | 103,886 | One payment transaction/installment record |
| order_reviews | 99,224 | One review record |
| products | 32,951 | One product |
| sellers | 3,095 | One seller |
| geolocation | Pending | One geolocation record |
| category_translation | 71 | One product category translation |

*Row counts are preliminary observations and will be validated during profiling.*

## 2. Table definitions

### 2.1 orders

**Grain:** One row per order.

| Column | Description |
|---|---|
| order_id | Unique order identifier |
| customer_id | Customer record associated with the order |
| order_status | Current/final order status in the dataset |
| order_purchase_timestamp | Date and time the order was purchased |
| order_approved_at | Payment approval timestamp |
| order_delivered_carrier_date | Date order was handed to the carrier |
| order_delivered_customer_date | Date order was delivered to customer |
| order_estimated_delivery_date | Estimated delivery date |

### 2.2 customers

**Grain:** One customer record.

| Column | Description |
|---|---|
| customer_id | Customer record identifier |
| customer_unique_id | Identifier representing the actual customer across orders |
| customer_zip_code_prefix | Customer postal code prefix |
| customer_city | Customer city |
| customer_state | Customer state |

### 2.3 order_items

**Grain:** One product item within an order.

| Column | Description |
|---|---|
| order_id | Associated order |
| order_item_id | Sequential item number within the order |
| product_id | Product identifier |
| seller_id | Seller identifier |
| shipping_limit_date | Seller shipping deadline |
| price | Price of the item |
| freight_value | Freight/shipping charge |

### 2.4 order_payments

**Grain:** One payment record associated with an order.

| Column | Description |
|---|---|
| order_id | Associated order |
| payment_sequential | Sequential payment number |
| payment_type | Payment method |
| payment_installments | Number of installments |
| payment_value | Payment amount |

### 2.5 order_reviews

**Grain:** One review record.

| Column | Description |
|---|---|
| review_id | Review identifier |
| order_id | Associated order |
| review_score | Customer review score |
| review_comment_title | Review title |
| review_comment_message | Review text |
| review_creation_date | Review creation timestamp |
| review_answer_timestamp | Review response timestamp |

### 2.6 products

**Grain:** One product.

| Column | Description |
|---|---|
| product_id | Product identifier |
| product_category_name | Product category in Portuguese |
| product_name_lenght | Product name character count |
| product_description_lenght | Product description character count |
| product_photos_qty | Number of product photos |
| product_weight_g | Product weight in grams |
| product_length_cm | Product length in centimetres |
| product_height_cm | Product height in centimetres |
| product_width_cm | Product width in centimetres |

### 2.7 sellers

**Grain:** One seller.

| Column | Description |
|---|---|
| seller_id | Seller identifier |
| seller_zip_code_prefix | Seller postal code prefix |
| seller_city | Seller city |
| seller_state | Seller state |

### 2.8 geolocation

**Grain:** To be confirmed after inspecting the CSV.

The table contains geographic information associated with Brazilian postal code prefixes. Exact columns and row-level meaning will be documented after schema inspection.

### 2.9 product_category_name_translation

**Grain:** One product category translation.

| Column | Description |
|---|---|
| product_category_name | Original Portuguese category |
| product_category_name_english | English translation |

## 3. Important relationships

| Primary table | Related table | Join key | Relationship |
|---|---|---|---|
| orders | customers | customer_id | Many orders to one customer record |
| orders | order_items | order_id | One order to many items |
| orders | order_payments | order_id | One order to many payment records |
| orders | order_reviews | order_id | One order to potentially multiple reviews |
| order_items | products | product_id | Many items to one product |
| order_items | sellers | seller_id | Many items to one seller |
| products | category_translation | product_category_name | Many products to one category translation |
| customers | geolocation | customer_zip_code_prefix | Potential geographic lookup |
| sellers | geolocation | seller_zip_code_prefix | Potential geographic lookup |

## 4. Analytical cautions

1. Orders and order items have different grains. Joining them can multiply order rows.
2. `customer_id` and `customer_unique_id` represent different concepts.
3. An order can have multiple payment records.
4. An order may have multiple item records and potentially multiple reviews.
5. `price`, `freight_value`, and `payment_value` are different financial measures.
6. Order status must be considered before defining completed sales or delivered orders.
7. Postal code prefixes may not uniquely identify a geographic coordinate.
8. The initial row counts and relationships must be validated before analytical use.

## 5. Documentation status

This is an initial schema-level dictionary. Data types, primary keys, foreign keys, nullability, uniqueness, and data-quality observations will be added during profiling and database modelling.