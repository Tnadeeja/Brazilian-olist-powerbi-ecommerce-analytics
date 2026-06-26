# Data Dictionary

## Dataset Overview

This project uses the Brazilian E-Commerce Public Dataset by Olist. The dataset contains multiple CSV files related to orders, customers, products, sellers, payments, reviews, and geolocation.

## Source Tables

| Table | Rows | Description |
|---|---:|---|
| olist_orders_dataset | 99,441 | Main order-level data |
| olist_order_items_dataset | 112,650 | Product/item-level order data |
| olist_order_payments_dataset | 103,886 | Payment transaction data |
| olist_order_reviews_dataset | 99,224 | Customer review data |
| olist_customers_dataset | 99,441 | Customer details and location |
| olist_products_dataset | 32,951 | Product details |
| olist_sellers_dataset | 3,095 | Seller details and location |
| olist_geolocation_dataset | 1,000,163 | Zip code, city, state, latitude, and longitude data |
| product_category_name_translation | 71 | Product category translation from Portuguese to English |

## Table Details

### olist_orders_dataset

Grain: One row per order.

Key columns:

- order_id
- customer_id
- order_status
- order_purchase_timestamp
- order_approved_at
- order_delivered_carrier_date
- order_delivered_customer_date
- order_estimated_delivery_date

Purpose:

Used to analyze order status, order dates, delivery performance, and order trends.

### olist_order_items_dataset

Grain: One row per product item in an order.

Key columns:

- order_id
- order_item_id
- product_id
- seller_id
- shipping_limit_date
- price
- freight_value

Purpose:

Used to analyze sales, freight value, product performance, category performance, and seller performance.

### olist_order_payments_dataset

Grain: One row per payment transaction.

Key columns:

- order_id
- payment_sequential
- payment_type
- payment_installments
- payment_value

Purpose:

Used to analyze payment methods, payment values, and installment behavior.

### olist_order_reviews_dataset

Grain: One row per review record.

Key columns:

- review_id
- order_id
- review_score
- review_comment_title
- review_comment_message
- review_creation_date
- review_answer_timestamp

Purpose:

Used to analyze customer satisfaction, review scores, and review behavior.

### olist_customers_dataset

Grain: One row per customer order record.

Key columns:

- customer_id
- customer_unique_id
- customer_zip_code_prefix
- customer_city
- customer_state

Purpose:

Used to analyze customers, repeat customers, and customer location.

### olist_products_dataset

Grain: One row per product.

Key columns:

- product_id
- product_category_name
- product_name_lenght
- product_description_lenght
- product_photos_qty
- product_weight_g
- product_length_cm
- product_height_cm
- product_width_cm

Purpose:

Used to analyze products, categories, product size, product weight, and product attributes.

### product_category_name_translation

Grain: One row per product category.

Key columns:

- product_category_name
- product_category_name_english

Purpose:

Used to translate product categories from Portuguese to English.

### olist_sellers_dataset

Grain: One row per seller.

Key columns:

- seller_id
- seller_zip_code_prefix
- seller_city
- seller_state

Purpose:

Used to analyze seller performance and seller location.

### olist_geolocation_dataset

Grain: Multiple rows per zip code prefix.

Key columns:

- geolocation_zip_code_prefix
- geolocation_lat
- geolocation_lng
- geolocation_city
- geolocation_state

Purpose:

Used for geographic analysis and map visuals.

Important note:

This table contains many duplicate records and should be cleaned before using it in the Power BI model.

## Main Relationships

| From Table | From Column | To Table | To Column |
|---|---|---|---|
| orders | customer_id | customers | customer_id |
| order_items | order_id | orders | order_id |
| order_items | product_id | products | product_id |
| order_items | seller_id | sellers | seller_id |
| payments | order_id | orders | order_id |
| reviews | order_id | orders | order_id |
| products | product_category_name | product_category_name_translation | product_category_name |

## Important Dataset Understanding Notes

- orders is at order level.
- order_items is at item/product level.
- payments is at payment transaction level.
- reviews is at review level.
- One order can have multiple items.
- One order can have multiple payment records.
- Some orders may have multiple review records.
- customer_id is unique per order/customer record.
- customer_unique_id can be used for repeat customer analysis.
- Product categories are originally in Portuguese and should be translated to English.
- Geolocation data has many duplicate rows and must be cleaned before modeling.