## BigQuery

- Step 1: Create a Google Cloud Project `retail-sales-data`
- Step 2: Enable BigQuery API
    - Go to the BigQuery API page.
    - Click "Enable" to enable the BigQuery API for your project.
- Step 3: Set Up Billing
- Step 4: Create a BigQuery Dataset
    - Go to BigQuery
    - Create a Dataset:`retail_data`
- Step 5: Create Tables in BigQuery
    - Create Fact and Dimension Tables:
    - Click on your dataset (retail_data) and then click "Create Table."
    - Choose Create Empty Table as the source.


Create the sales Fact Table:
Table name: sales
```sql
sale_id: INTEGER,
product_id: INTEGER,
store_id: INTEGER,
date_id: INTEGER,
amount_sold: FLOAT,
quantity_sold: INTEGER
```

Create the `products` Dimension Table:
Table name: `products`
```sql
product_id: INTEGER,
product_name: STRING,
category: STRING
```

Create the `stores` Dimension Table:
Table name: `stores`
```sql
store_id:INTEGER,
store_name:STRING,
location:STRING
```

Create the `dates` Dimension Table:
Table name: `dates`
```sql
date_id:INTEGER,
day:INTEGER,
month:INTEGER,
year:INTEGER
```

- Step 6: Load Data into Tables
- Step 7: Run Queries
- Step 8: Visualize Data with Google Data Studio (Looker)

## Seed data
```sql
LOAD DATA INTO `retail_data.products`
FROM FILES (
  format = 'CSV',
  uris = ['gs://retails-dummy/products.csv']
);

LOAD DATA INTO `retail_data.dates`
FROM FILES (
  format = 'CSV',
  uris = ['gs://retails-dummy/dates.csv']
);

LOAD DATA INTO `retail_data.sales`
FROM FILES (
  format = 'CSV',
  uris = ['gs://retails-dummy/sales.csv']
);

LOAD DATA INTO `retail_data.stores`
FROM FILES (
  format = 'CSV',
  uris = ['gs://retails-dummy/stores.csv']
);
```

## Query
- Total Sales by Product and Month:
```sql
SELECT
    p.product_name,
    d.month,
    SUM(s.amount_sold) AS total_sales
FROM
    `retail_data.sales` s
JOIN
    `retail_data.products` p ON s.product_id = p.product_id
JOIN
    `retail_data.dates` d ON s.date_id = d.date_id
GROUP BY
    p.product_name, d.month
ORDER BY
    total_sales DESC;
```

- Sales Performance by Store Location:
```sql
SELECT
    st.location,
    SUM(s.amount_sold) AS total_sales
FROM
    `retail_data.sales` s
JOIN
    `retail_data.stores` st ON s.store_id = st.store_id
GROUP BY
    st.location
ORDER BY
    total_sales DESC;
```