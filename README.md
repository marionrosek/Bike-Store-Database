## Biker Store Database
#### You’re working for a bike company and they want to:

#### Increase revenue
#### Understand customers
#### Optimize inventory
#### Improve operations
```python
import pandas as pd
import sqlite3
```
```python
brands_df=pd.read_csv('brands.csv')
categories_df=pd.read_csv('categories.csv')
customers_df=pd.read_csv('customers (1).csv')
order_items_df=pd.read_csv('order_items.csv')
orders_df=pd.read_csv('orders.csv')
products_df=pd.read_csv('products.csv')
staffs_df=pd.read_csv('staffs.csv')
stocks_df=pd.read_csv('stocks.csv')
stores_df=pd.read_csv('stores.csv')
```
```python
conn=sqlite3.connect('bike_store.db')
```
```python
pd.read_sql("SELECT name FROM sqlite_master WHERE type='table';",conn)
```
## Data Analysis
### 1. Bikes with the most revenue
```sql
pd.read_sql("SELECT p.product_name, SUM(oi.quantity * oi.list_price) AS revenue
FROM order_items oi 
JOIN products p ON oi.product_id=p.product_id 
GROUP BY product_name 
ORDER BY revenue DESC LIMIT 5;", conn)
```
#### Trek Slash 8 27.5-2016 earns the Biker store the most revenue and the company might invest in these bikes to earn more revenue.
### 2. Most expensive bikes
```sql
pd.read_sql("SELECT product_name,list_price FROM products ORDER BY list_price DESC LIMIT 5;",conn)
```
#### The Trek Domane and silque brand bikes are the most expensive bikes in the store
### 3. Orders made per customer
```sql
pd.read_sql("SELECT customer_id,COUNT(order_id) as total_orders
FROM orders
GROUP BY customer_id
ORDER BY total_orders DESC
LIMIT 10;", conn)
```
### 4. Most valuable customer
```sql
pd.read_sql("SELECT c.first_name,c.last_name, SUM(quantity * list_price) AS total_amount_spent
 FROM customers c
JOIN orders o ON c.customer_id=o.customer_id
JOIN order_items oi ON o.order_id=oi.order_id
GROUP BY c.first_name,c.last_name
ORDER BY total_amount_spent DESC
LIMIT 1;", conn)
```
#### Pamelia Newman is store's most valuable customer with highest amount spent on bikes
### 5. Bike Categories with most sales
```sql
pd.read_sql("SELECT c.category_name, SUM(oi.quantity * oi.list_price) AS sales
 FROM categories c
JOIN products p ON c.category_id=p.category_id
JOIN order_items oi ON p.product_id=oi.product_id
GROUP BY category_name ORDER BY sales DESC
LIMIT 5;", conn)
```
### 5. Bike Categories with most sales
```sql
pd.read_sql("SELECT c.category_name, SUM(oi.quantity * oi.list_price) AS sales 
FROM categories c 
JOIN products p ON c.category_id=p.category_id
JOIN order_items oi ON p.product_id=oi.product_id
GROUP BY category_name ORDER BY sales ASC 
LIMIT 2;", conn)
```
#### Mountain bikes have the most sales in the biker store, followed by road bikes, while 
children's bicycles and comfort bicycles have the lowest sales in the Biker Store; therefore,
The Biker store could invest in more mountain bikes for more sales
#### 6. Stock levels in the Store
```sql
pd.read_sql("SELECT store_name,SUM(st.quantity) AS stock_levels
 FROM stores s JOIN stocks st ON s.store_id=st.store_id
GROUP BY store_name
ORDER BY stock_levels ASC;",conn)
```
```sql
pd.read_sql("SELECT product_name,SUM(st.quantity) AS stock_levels
 FROM products p
JOIN stocks st ON p.product_id=st.product_id
GROUP BY product_name
ORDER BY stock_levels ASC
LIMIT 5;", conn)
ORDER BY stock_levels ASC;",conn)
```
#### The Baldwin Bikes store has the lowest stock of bikes among other stores, while
Trek Domane SLR Frameset - 2018 is the lowest stocked  bicycle, and the Biker store needs
to add more bikes.
