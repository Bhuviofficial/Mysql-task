This project demonstrates how to design and manage a simple E-Commerce Database System using MySQL.
It includes database creation, table relationships, normalization, and various SQL queries with real-world use cases.


📂 Project Overview

The e-commerce system consists of three main tables:

customers – Stores customer details

orders – Stores order details

products – Stores product details

Later, the database is normalized by adding an order_items table.

🏗️ 1. Create Database
CREATE DATABASE ecommerce;
USE ecommerce;

🧱 2. Create Tables
Customers Table
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    address VARCHAR(255)
);

Products Table
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    price DECIMAL(10,2),
    description TEXT,
    discount DECIMAL(5,2) DEFAULT 0
);

Orders Table
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10,2),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

📥 3. Insert Sample Data
Customers
INSERT INTO customers (name, email, address)
VALUES
('John Doe','john@example.com','USA'),
('Alice Smith','alice@example.com','UK'),
('Bob Lee','bob@example.com','India');

Products
INSERT INTO products (name, price, description)
VALUES
('Product A', 50.00,'Description for Product A'),
('Product B', 80.00,'Description for Product B'),
('Product C', 40.00,'Description for Product C'),
('Product D', 120.00,'Description for Product D');

Orders
INSERT INTO orders (customer_id, order_date, total_amount)
VALUES
(1, CURDATE(), 150.00),
(2, CURDATE() - INTERVAL 10 DAY, 250.00),
(3, CURDATE() - INTERVAL 40 DAY, 100.00);

🧪 Queries & Outputs
✅ 1. Customers who ordered in last 30 days
SELECT * FROM customers
WHERE id IN (
    SELECT customer_id FROM orders
    WHERE order_date >= CURDATE() - INTERVAL 30 DAY
);

✅ 2. Total order amount per customer
SELECT c.name, SUM(o.total_amount) AS total_spent
FROM customers c
JOIN orders o ON c.id = o.customer_id
GROUP BY c.id;

✅ 3. Update price of Product C to 45.00
UPDATE products
SET price = 45.00
WHERE name = 'Product C';

✅ 4. Add “discount” column

(Only run once — if already added, skip)

ALTER TABLE products
ADD COLUMN discount DECIMAL(5,2) DEFAULT 0;

✅ 5. Top 3 highest priced products
SELECT *
FROM products
ORDER BY price DESC
LIMIT 3;

✅ 6. Customers who ordered Product A

(Requires normalized order_items table)

SELECT DISTINCT c.name
FROM customers c
JOIN orders o ON c.id = o.customer_id
JOIN order_items oi ON o.id = oi.order_id
JOIN products p ON oi.product_id = p.id
WHERE p.name = 'Product A';

✅ 7. Join customers + orders (name + order date)
SELECT c.name, o.order_date
FROM customers c
JOIN orders o ON c.id = o.customer_id;

✅ 8. Orders with amount > 150
SELECT *
FROM orders
WHERE total_amount > 150.00;

🔄 9. Normalization – Create order_items Table
Create table:
CREATE TABLE order_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

Remove product-level data from orders

(Orders now only store total amount)

No changes needed unless product columns existed.

✅ 10. Average order total
SELECT AVG(total_amount) AS average_order_total
FROM orders;

✔️ Tools Used

MySQL Workbench

MySQL Server 8+

📌 Notes

If you run queries multiple times, remove duplicates before re-running inserts.

Safe update mode must be disabled for UPDATE statements.

REPOLINK:
git-clone:https://github.com/Bhuviofficial/Mysql-task
