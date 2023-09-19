## Maven toys pet project
Sales & products data analysis for a fictitious chain of toy stores in Mexico called Maven Toys. Data includes information about products, stores and daily sales transactions.

Data processing was carried out using SQL. 
Visualization - in [Tableau](https://public.tableau.com/views/MavenToysBusinessAnalysis/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link):

<div class='tableauPlaceholder' id='viz1695139342834' style='position: relative'><noscript><a href='#'><img alt='Dashboard 1 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Ma&#47;MavenToysBusinessAnalysis&#47;Dashboard1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='MavenToysBusinessAnalysis&#47;Dashboard1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Ma&#47;MavenToysBusinessAnalysis&#47;Dashboard1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /></object></div>

### **SQL code**

Sales by category per day
```SQL
SELECT sales.date, products.product_category, SUM(sales.units) AS sold_units
FROM sales LEFT JOIN products
ON sales.product_id = products.product_id
GROUP BY sales.date, products.product_category
ORDER BY date, sold_units DESC
```



Sales by category and product for the entire period
```SQL
SELECT products.product_category, products.product_name, COUNT(sales.units), 
SUM(sales.units*products.product_price) AS sold_income, SUM(sales.units*(products.product_price - products.product_cost)) AS sold_profit
FROM sales LEFT JOIN products
ON sales.product_id = products.product_id
GROUP BY products.product_category, products.product_name
ORDER BY sold_profit DESC
```
The most popular products by stores
```SQL
SELECT stores.store_city, stores.store_name, products.product_name, SUM(sales.units) AS sold_product_amount, 
SUM(sales.units*(products.product_price-products.product_cost)) AS product_profit
FROM sales LEFT JOIN products
ON sales.product_id = products.product_id
LEFT JOIN stores
ON sales.store_id = stores.store_id
GROUP BY  stores.store_city, stores.store_name, products.product_name
ORDER BY sold_product_amount DESC
```
Sales by city and location
```SQL
SELECT stores.store_city, stores.store_location, SUM(sales.units) AS sold_product_amount, 
SUM(sales.units*(products.product_price-products.product_cost)) AS product_profit
FROM sales LEFT JOIN products
ON sales.product_id = products.product_id
LEFT JOIN stores
ON sales.store_id = stores.store_id
GROUP BY  stores.store_city, stores.store_location
ORDER BY product_profit DESC
```
Stores by profitability
```SQL
WITH alldata AS (
SELECT *
FROM sales LEFT JOIN products
ON sales.product_id = products.product_id
LEFT JOIN stores
ON sales.store_id = stores.store_id
)

SELECT store_city, store_name, SUM(units*(product_price-product_cost)) AS profit
FROM alldata
GROUP BY store_city, store_name
ORDER BY profit DESC
```

