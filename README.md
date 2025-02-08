-- Create Database
CREATE DATABASE OnlineRetailDB;

-- Use the Database
USE OnlineRetailDB;

-- Create Customer Table
CREATE TABLE Customer (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    location VARCHAR(50),
    registration_date DATE
);

-- Create Products Table
CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10, 2),
    stock_quantity INT
);

-- Create Orders Table
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10, 2),
    status VARCHAR(20),
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id)
);

-- Create Order_Details Table
CREATE TABLE Order_Details (
    order_detail_id INT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    price DECIMAL(10, 2),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

-- Insert Data into Customer
INSERT INTO Customer VALUES (1, 'John Doe', 'john@example.com', 'New York', '2023-01-15');
INSERT INTO Customer VALUES (2, 'Jane Smith', 'jane@example.com', 'Los Angeles', '2023-02-20');
INSERT INTO Customer VALUES (3, 'Mike Johnson', 'mike@example.com', 'Chicago', '2023-03-10');
INSERT INTO Customer VALUES (4, 'Emily Davis', 'emily@example.com', 'Houston', '2023-04-05');
INSERT INTO Customer VALUES (5, 'David Brown', 'david@example.com', 'Phoenix', '2023-05-25');

-- Insert Data into Products
INSERT INTO Products VALUES (101, 'Laptop', 'Electronics', 800.00, 50);
INSERT INTO Products VALUES (102, 'Smartphone', 'Electronics', 500.00, 100);
INSERT INTO Products VALUES (103, 'Desk Chair', 'Furniture', 150.00, 30);
INSERT INTO Products VALUES (104, 'Coffee Maker', 'Appliances', 70.00, 20);
INSERT INTO Products VALUES (105, 'Backpack', 'Accessories', 40.00, 80);

-- Insert Data into Orders
INSERT INTO Orders VALUES (1001, 1, '2023-06-15', 1300.00, 'Completed');
INSERT INTO Orders VALUES (1002, 2, '2023-06-20', 540.00, 'Completed');
INSERT INTO Orders VALUES (1003, 3, '2023-07-05', 150.00, 'Pending');
INSERT INTO Orders VALUES (1004, 4, '2023-07-10', 70.00, 'Completed');
INSERT INTO Orders VALUES (1005, 5, '2023-07-15', 40.00, 'Canceled');

-- Insert Data into Order_Details
INSERT INTO Order_Details VALUES (1, 1001, 101, 1, 800.00);
INSERT INTO Order_Details VALUES (2, 1001, 102, 1, 500.00);
INSERT INTO Order_Details VALUES (3, 1002, 102, 1, 500.00);
INSERT INTO Order_Details VALUES (4, 1002, 105, 1, 40.00);
INSERT INTO Order_Details VALUES (5, 1003, 103, 1, 150.00);

-- Queries
-- 1. Total Sales
SELECT SUM(total_amount) AS Total_Sales FROM Orders WHERE status = 'Completed';

-- 2. Top 5 Best-Selling Products
SELECT p.product_name, SUM(od.quantity) AS Total_Sold
FROM Order_Details od
JOIN Products p ON od.product_id = p.product_id
GROUP BY p.product_name
ORDER BY Total_Sold DESC
LIMIT 5;

-- 3. Customer with the Highest Number of Orders
SELECT c.name, COUNT(o.order_id) AS Number_of_Orders
FROM Customer c
JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.name
ORDER BY Number_of_Orders DESC;

-- 4. Revenue by Product Category
SELECT p.category, SUM(od.price * od.quantity) AS Revenue
FROM Order_Details od
JOIN Products p ON od.product_id = p.product_id
GROUP BY p.category;

-- 5. Monthly Sales Trend
SELECT DATE_FORMAT(order_date, '%Y-%m') AS Month, SUM(total_amount) AS Monthly_Sales
FROM Orders
WHERE status = 'Completed'
GROUP BY Month
ORDER BY Month;

-- 6. Inactive Customer
SELECT name, email
FROM Customer
WHERE customer_id NOT IN (SELECT DISTINCT customer_id FROM Orders);

-- 7. Average Order Value
SELECT AVG(total_amount) AS Average_Order_Value
FROM Orders
WHERE status = 'Completed';

-- 8. Products with Low Stock and High Demand
SELECT p.product_name, p.stock_quantity, SUM(od.quantity) AS Total_Demand
FROM Products p
JOIN Order_Details od ON p.product_id = od.product_id
GROUP BY p.product_name, p.stock_quantity
HAVING p.stock_quantity < 50 AND Total_Demand > 1;

-- 9. New vs Returning Customer
SELECT c.name, COUNT(o.order_id) AS Number_of_Orders,
       CASE WHEN COUNT(o.order_id) = 1 THEN 'New' ELSE 'Returning' END AS Customer_Type
FROM Customer c
LEFT JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.name;

-- 10. Revenue from Each Customer
SELECT c.name, SUM(o.total_amount) AS Total_Spent
FROM Customer c
JOIN Orders o ON c.customer_id = o.customer_id
WHERE o.status = 'Completed'
GROUP BY c.name
ORDER BY Total_Spent DESC;
