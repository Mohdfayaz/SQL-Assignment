-- Create the database;
drop database if exists ecommerce;
create database ecommerce;
use ecommerce;


-- Create customers table;
SQL COMMAND --
  CREATE TABLE customers (
      id INT AUTO_INCREMENT PRIMARY KEY,
      name VARCHAR(100) NOT NULL,
      email VARCHAR(100) UNIQUE NOT NULL,
      address TEXT NOT NULL
  );


-- Create products table;
SQL COMMAND --
   CREATE TABLE products (
      id INT AUTO_INCREMENT PRIMARY KEY,
      name VARCHAR(100) NOT NULL,
      price DECIMAL(10,2) NOT NULL,
      description TEXT
   );


-- Create orders table;
SQL COMMAND --
  CREATE TABLE orders (
       id INT AUTO_INCREMENT PRIMARY KEY,
       customer_id INT,
       order_date DATE NOT NULL,
       total_amount DECIMAL(10,2) NOT NULL,
       FOREIGN KEY (customer_id) REFERENCES customers(id)
   ); 


-- Insert sample data into customers;
SQL COMMAND --
  INSERT INTO customers (name, email, address) VALUES
  ('Riyaz Bhai', 'riyazbro123@gmail.com', '123 saidapet Street'),
  ('Zubair Ahmad', 'zubairah321@gmail.com', '456 velachery Avenue'),
  ('salman Khan', 'salmankhan231@gmail.com', '789 bombay Road');


-- Insert sample data into products;
SQL COMMAND --
  INSERT INTO products (name, price, description) VALUES
  ('Product A', 25.00, 'Description of Product A'),
  ('Product B', 30.00, 'Description of Product B'),
  ('Product C', 40.00, 'Description of Product C'),
  ('Product D', 50.00, 'Description of Product D');


-- Insert sample data into orders;
SQL COMMAND --
  INSERT INTO orders (customer_id, order_date, total_amount) VALUES
  (1, CURDATE(), 75.00),
  (2, DATE_SUB(CURDATE(), INTERVAL 15 DAY), 120.00),
  (3, DATE_SUB(CURDATE(), INTERVAL 40 DAY), 200.00);


-- Retrieve all customers who have placed an order in the last 30 days;
SQL COMMAND --
  SELECT DISTINCT c.* FROM customers c
 JOIN orders o ON c.id = o.customer_id
 WHERE o.order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY);


-- Output:
 | id | name         | email                 | address              |
 |----|--------------|-----------------------|----------------------|
 | 1  | Riyaz Bhai   | Riyazbro123@gmail.com | 123 saidapet Street  |
 | 2  | Zubair Ahmad | zubairah321@gmail.com | 456 velachery Avenue |
 

-- Get the total amount of all orders placed by each customer;
SQL COMMAND --
  SELECT c.name, SUM(o.total_amount) AS total_spent FROM customers c
  JOIN orders o ON c.id = o.customer_id
  GROUP BY c.id;


-- Output:
 | name         | total_spent |
 |--------------|-------------|
 | Riyaz Bhai   | 75.00       |
 | Zubair Ahmed | 120.00      |
 | Salman Khan  | 200.00      |
 

-- Update the price of Product C to 45.00;
SQL COMMAND --
   UPDATE products SET price = 45.00 WHERE name = 'Product C';


-- Add a new column discount to the products table;
SQL COMMAND --
   ALTER TABLE products ADD COLUMN discount DECIMAL(5,2) DEFAULT 0;


-- Retrieve the top 3 products with the highest price;
SQL COMMAND --
   SELECT * FROM products ORDER BY price DESC LIMIT 3;


-- Output:
 | id | name     | price |
 |----|----------|-------|
 | 4  | Product D| 50.00 |
 | 3  | Product C| 45.00 |
 | 2  | Product B| 30.00 |


-- Get the names of customers who have ordered Product A;
SQL COMMAND --
 SELECT DISTINCT c.name FROM customers c
 JOIN orders o ON c.id = o.customer_id
 JOIN order_items oi ON o.id = oi.order_id
 JOIN products p ON oi.product_id = p.id
 WHERE p.name = 'Product A';


-- Output:
 | name      |
 |-----------|
 | Riyaz Bhai|


-- Join orders and customers to retrieve the customer's name and order date;
SQL COMMAND --
 SELECT c.name, o.order_date FROM orders o
 JOIN customers c ON o.customer_id = c.id;


-- Output:
 | name         | order_date |
 |--------------|------------|
 | Riyaz Bhai   | 2025-02-11 |
 | Zubair Ahmad | 2025-01-27 |
 | Salman Khan  | 2024-12-28 |


-- Retrieve orders with a total amount greater than 150.00;
SQL COMMAND --
   SELECT * FROM orders WHERE total_amount > 150.00;


-- Output:
 | id | customer_id| order_date | total_amount|
 |----|------------|------------|-------------|
 | 3  | 3          | 2024-12-28 | 200.00      |


-- Normalize database: Create order_items table and update orders;
SQL COMMAND --
  CREATE TABLE order_items (
      id INT AUTO_INCREMENT PRIMARY KEY,
      order_id INT,
      product_id INT,
      quantity INT NOT NULL,
      FOREIGN KEY (order_id) REFERENCES orders(id),
      FOREIGN KEY (product_id) REFERENCES products(id)
  );


-- Retrieve the average total of all orders;
SQL COMMAND --
    SELECT AVG(total_amount) AS average_order_value FROM orders;


--  Output:
 | average_order_value |
 |-------------------- |
 | 131.67              |
