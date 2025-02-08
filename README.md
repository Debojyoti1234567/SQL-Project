Features

Database Creation & Management: Structured tables with appropriate relationships.

Data Insertion: Pre-populated customer, product, and order data.

Order Processing & Tracking: Orders and order details are linked.

Data Analysis Queries:

Total sales calculations

Best-selling products

Customer engagement insights

Revenue analysis by product category

Monthly sales trends

Identifying inactive customers

Average order value determination

Stock management insights

🏗️ Database Schema

The database consists of four main tables:

Customer: Stores customer details (ID, name, email, location, registration date).

Products: Stores product details (ID, name, category, price, stock quantity).

Orders: Stores order information (order ID, customer ID, order date, total amount, status).

Order_Details: Links products to orders (order detail ID, order ID, product ID, quantity, price).

📊 SQL Queries

The project includes various SQL queries for business insights and analytics, such as:

Total Sales: Calculates overall revenue from completed orders.

Top 5 Best-Selling Products: Ranks products based on sales volume.

Customer with Most Orders: Identifies the most frequent buyers.

Revenue by Product Category: Breaks down sales by product category.

Monthly Sales Trend: Analyzes sales performance over time.

Inactive Customers: Lists customers who haven't placed any orders.

Average Order Value: Determines the average transaction amount.

Products with Low Stock & High Demand: Identifies products needing restocking.

New vs Returning Customers: Classifies customers based on order frequency.

Revenue from Each Customer: Displays spending per customer.
