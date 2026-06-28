# Data Cleaning Plan

## Cleaning Approach

The raw CSV files were loaded into Power BI as staging queries using the `stg_` prefix. Cleaned versions of each table were created using referenced queries with the `clean_` prefix.

This approach keeps the raw source queries separate from the cleaned analytical queries.

## Cleaned Tables

- clean_orders
- clean_customers
- clean_sellers
- clean_products
- clean_order_items
- clean_order_payments
- clean_order_reviews
- clean_geolocation
- clean_category_translation

## Main Cleaning Steps

### Orders
- Corrected data types
- Cleaned order status text
- Created purchase, approved, delivered, and estimated delivery date columns
- Created delivery days, estimated delivery days, and delivery delay days
- Created delivery performance status
- Created delivered and cancelled flags

### Customers
- Corrected data types
- Cleaned customer city and state text
- Renamed zip code column

### Sellers
- Corrected data types
- Cleaned seller city and state text
- Renamed zip code column

### Products
- Corrected data types
- Cleaned product category text
- Merged category translation data
- Created English and Portuguese category columns
- Replaced missing category values with unknown
- Kept missing numeric product dimensions as null

### Order Items
- Corrected data types
- Cleaned ID columns
- Created shipping limit day
- Created item total value
- Created free freight flag
- Created order item key

### Payments
- Corrected data types
- Cleaned payment type
- Created payment key
- Created installment group
- Created payment value status

### Reviews
- Corrected data types
- Cleaned review ID, order ID, and comment columns
- Created review creation day and review answer day
- Created review response days
- Created review sentiment group
- Created review comment flag
- Created review key

### Geolocation
- Corrected data types
- Cleaned city and state text
- Renamed columns
- Removed exact duplicate rows
- Created location key

### Category Translation
- Corrected data types
- Cleaned category text
- Renamed columns