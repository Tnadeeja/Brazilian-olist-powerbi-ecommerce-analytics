# Data Model Design

## Purpose

The purpose of the data model is to organize the cleaned e-commerce data into a professional Power BI star schema. This improves report performance, simplifies DAX calculations, and makes the model easier to understand and maintain.

## Modeling Approach

The raw CSV files were first loaded as staging queries using the `stg_` prefix. Cleaned queries were then created using the `clean_` prefix. Finally, business-ready fact and dimension tables were created for the Power BI model.

Only the final fact and dimension tables were loaded into the Power BI model. Staging, helper, and cleaning queries were disabled from loading to keep the model clean.

## Final Model Tables

### Dimension Tables

- Dim Customer
- Dim Product
- Dim Seller
- Dim Date

### Fact Tables

- Fact Sales
- Fact Payments
- Fact Reviews

## Dimension Table Details

### Dim Customer

Grain: One row per customer_id.

Purpose:

- Customer location analysis
- Customer state analysis
- Customer-level filtering
- Repeat customer analysis using customer_unique_id

Key column:

- customer_id

### Dim Product

Grain: One row per product_id.

Purpose:

- Product category analysis
- Product size and weight analysis
- Product performance analysis

Key column:

- product_id

Additional columns created:

- product_category
- product_category_pt
- product_volume_cm3
- has_product_dimensions

### Dim Seller

Grain: One row per seller_id.

Purpose:

- Seller performance analysis
- Seller city/state analysis
- Seller contribution analysis

Key column:

- seller_id

### Dim Date

Grain: One row per calendar date.

Purpose:

- Time-based analysis
- Monthly trends
- Yearly trends
- Time intelligence calculations

Key column:

- date

The Dim Date table was marked as the official date table using the `date` column.

## Fact Table Details

### Fact Sales

Grain: One row per product item sold inside an order.

Source:

- clean_order_items
- selected order-level columns from clean_orders

Purpose:

- Sales analysis
- Revenue analysis
- Freight analysis
- Product performance
- Seller performance
- Delivery performance

Key columns:

- order_item_key
- order_id
- customer_id
- product_id
- seller_id
- purchase_date

Numeric columns:

- price
- freight_value
- item_total_value
- delivery_days
- estimated_delivery_days
- delivery_delay_days

### Fact Payments

Grain: One row per payment transaction.

Source:

- clean_order_payments
- selected order-level columns from clean_orders

Purpose:

- Payment method analysis
- Installment analysis
- Payment value analysis

Key columns:

- payment_key
- order_id
- customer_id
- purchase_date

Numeric columns:

- payment_value
- payment_installments

### Fact Reviews

Grain: One row per review record.

Source:

- clean_order_reviews
- selected order-level columns from clean_orders

Purpose:

- Review score analysis
- Customer satisfaction analysis
- Review response time analysis
- Delivery performance vs review score analysis

Key columns:

- review_key
- review_id
- order_id
- customer_id
- review_creation_day

Numeric columns:

- review_score
- review_response_days
- delivery_days
- delivery_delay_days

## Relationships

| From Table | From Column | To Table | To Column | Cardinality | Cross Filter |
|---|---|---|---|---|---|
| Dim Customer | customer_id | Fact Sales | customer_id | One-to-many | Single |
| Dim Customer | customer_id | Fact Payments | customer_id | One-to-many | Single |
| Dim Customer | customer_id | Fact Reviews | customer_id | One-to-many | Single |
| Dim Product | product_id | Fact Sales | product_id | One-to-many | Single |
| Dim Seller | seller_id | Fact Sales | seller_id | One-to-many | Single |
| Dim Date | date | Fact Sales | purchase_date | One-to-many | Single |
| Dim Date | date | Fact Payments | purchase_date | One-to-many | Single |
| Dim Date | date | Fact Reviews | review_creation_day | One-to-many | Single |

## Modeling Decisions

- A star schema was used instead of one large flat table.
- Fact tables were kept at their natural grains.
- Direct fact-to-fact relationships were avoided.
- Dimension tables were connected to fact tables using one-to-many relationships.
- Single-direction filtering was used to keep the model simple and predictable.
- Dim Date was marked as the official Date table for time intelligence.
- Staging and cleaning queries were disabled from loading into the final model.

## Why Fact-to-Fact Relationships Were Avoided

Fact Sales, Fact Payments, and Fact Reviews have different grains.

- Fact Sales is item-level.
- Fact Payments is payment transaction-level.
- Fact Reviews is review-level.

Directly connecting fact tables using order_id could create many-to-many relationship issues and incorrect calculations. Instead, the model uses shared dimensions such as Dim Customer and Dim Date.