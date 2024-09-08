# MySQL E-Commerce Database Setup and Queries
This project demonstrates the creation and manipulation of a MySQL database for an e-commerce application. It includes the creation of tables, inserting data, and performing various queries to analyze the data.

## Project Setup
### Prerequisites
MySQL (installed and running on your machine)

## Installation and Setup

1. Start MySQL Server: Ensure that the MySQL server is running. You can start it using:

```
brew services start mysql
```

2. Connect to MySQL: Open your terminal and connect to MySQL as the root user:

```
mysql -u root -p
```
Enter your password when prompted.

3. Create Database and Tables: Execute the following SQL commands to set up the database and tables:
```
-- Create the database
CREATE DATABASE ecommerce;

-- Use the newly created database
USE ecommerce;

-- Create customers table
CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_name VARCHAR(100),
    email VARCHAR(100),
    address VARCHAR(255),
    city VARCHAR(100),
    country VARCHAR(100)
);

-- Create products table
CREATE TABLE products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    product_name VARCHAR(100),
    price DECIMAL(10, 2),
    stock INT
);

-- Create orders table
CREATE TABLE orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

-- Create order_details table
CREATE TABLE order_details (
    order_detail_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT,
    product_id INT,
    quantity INT,
    price DECIMAL(10, 2),
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```
4. Insert Data: Populate the tables with sample data:
```
-- Insert data into customers table
INSERT INTO customers (customer_name, email, address, city, country)
VALUES
    ('John Doe', 'john@example.com', '123 Elm St', 'New York', 'USA'),
    ('Jane Smith', 'jane@example.com', '456 Maple Ave', 'Los Angeles', 'USA');

-- Insert data into products table
INSERT INTO products (product_name, price, stock)
VALUES
    ('Laptop', 999.99, 50),
    ('Smartphone', 599.99, 100),
    ('Tablet', 399.99, 75);

-- Insert data into orders table
INSERT INTO orders (customer_id, order_date)
VALUES
    (1, '2024-09-01'),
    (2, '2024-09-03');

-- Insert data into order_details table
INSERT INTO order_details (order_id, product_id, quantity, price)
VALUES
    (1, 1, 1, 999.99),  -- John bought 1 Laptop
    (1, 3, 2, 799.98),  -- John bought 2 Tablets
    (2, 2, 1, 599.99);  -- Jane bought 1 Smartphone
```
# Query Examples
1. Total Sales by Product:
```
SELECT p.product_name, SUM(od.quantity * od.price) AS total_sales
FROM order_details od
JOIN products p ON od.product_id = p.product_id
GROUP BY p.product_name;
```
2. Total Spent by Customer:
```
SELECT c.customer_name, SUM(od.quantity * od.price) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_details od ON o.order_id = od.order_id
GROUP BY c.customer_name
ORDER BY total_spent DESC
LIMIT 3;
```
3. Total Value of Each Order:

```
SELECT o.order_id, c.customer_name, SUM(od.quantity * od.price) AS total_value
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN order_details od ON o.order_id = od.order_id
GROUP BY o.order_id, c.customer_name;
```
4. Products with Low Stock:

```
SELECT product_name, stock
FROM products
WHERE stock < 20;
```
5. Orders within a Date Range:

```
SELECT o.order_id, c.customer_name, o.order_date
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
WHERE o.order_date BETWEEN '2024-09-01' AND '2024-09-30';
```
