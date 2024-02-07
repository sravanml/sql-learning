
💡 **Key Points:**

- JOINS
- Few peactical examples

Action items

- [ ]  Understand joins

*Note ends here*

✏️ **Notes:**

# Understanding SQL Joins: A Comprehensive Guide

In the realm of relational databases, the ability to combine data from multiple tables is a fundamental skill. This process is facilitated by SQL joins, a powerful mechanism for linking rows from different tables based on specified conditions. Let's delve into the world of joins to uncover their types, use cases, and practical examples.

- Source code
    
    ```sql
    -- Employees Table
    CREATE TABLE employees (
        employee_id SERIAL PRIMARY KEY,
        employee_name VARCHAR(255) NOT NULL,
        department_id INTEGER,
        location VARCHAR(50)
    );
    
    -- Departments Table
    CREATE TABLE departments (
        department_id SERIAL PRIMARY KEY,
        department_name VARCHAR(255) NOT NULL,
        location VARCHAR(50)
    );
    
    -- Sample Data for Employees
    INSERT INTO employees (employee_name, department_id, location) VALUES
    ('John Doe', 1, 'New York'),
    ('Jane Smith', 2, 'London'),
    ('Bob Johnson', 1, 'New York'),
    ('Alice Williams', 3, 'Berlin');
    
    -- Sample Data for Departments
    INSERT INTO departments (department_name, location) VALUES
    ('HR', 'New York'),
    ('Finance', 'London'),
    ('IT', 'Berlin');
    
    -- Customers Table
    CREATE TABLE customers (
        customer_id SERIAL PRIMARY KEY,
        name VARCHAR(255) NOT NULL,
        email VARCHAR(255) UNIQUE NOT NULL,
        phone VARCHAR(20),
        address TEXT
    );
    
    -- Restaurants Table
    CREATE TABLE restaurants (
        restaurant_id SERIAL PRIMARY KEY,
        name VARCHAR(255) NOT NULL,
        cuisine_type VARCHAR(50),
        location VARCHAR(255)
    );
    
    -- Menu Items Table
    CREATE TABLE menu_items (
        item_id SERIAL PRIMARY KEY,
        restaurant_id INTEGER REFERENCES restaurants(restaurant_id),
        name VARCHAR(255) NOT NULL,
        price DECIMAL(10, 2) NOT NULL,
        description TEXT
    );
    
    -- Orders Table
    CREATE TABLE orders (
        order_id SERIAL PRIMARY KEY,
        customer_id INTEGER REFERENCES customers(customer_id),
        order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        total_price DECIMAL(10, 2) NOT NULL,
        status VARCHAR(50) DEFAULT 'Pending'
    );
    
    -- Order Items Table
    CREATE TABLE order_items (
        order_item_id SERIAL PRIMARY KEY,
        order_id INTEGER REFERENCES orders(order_id),
        item_id INTEGER REFERENCES menu_items(item_id),
        quantity INTEGER NOT NULL,
        subtotal DECIMAL(10, 2) NOT NULL
    );
    
    -- Delivery Staff Table
    CREATE TABLE delivery_staff (
        staff_id SERIAL PRIMARY KEY,
        name VARCHAR(255) NOT NULL,
        phone VARCHAR(20),
        location VARCHAR(255)
    );
    
    -- Delivery Assignments Table
    CREATE TABLE delivery_assignments (
        assignment_id SERIAL PRIMARY KEY,
        order_id INTEGER REFERENCES orders(order_id),
        staff_id INTEGER REFERENCES delivery_staff(staff_id),
        assignment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );
    
    -- Reviews Table
    CREATE TABLE reviews (
        review_id SERIAL PRIMARY KEY,
        customer_id INTEGER REFERENCES customers(customer_id),
        restaurant_id INTEGER REFERENCES restaurants(restaurant_id),
        order_id INTEGER REFERENCES orders(order_id),
        rating INTEGER NOT NULL,
        comment TEXT,
        date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    );
    
    -- Sample Data for Customers
    INSERT INTO customers (name, email, phone, address) VALUES
    ('John Doe', 'john@example.com', '1234567890', '123 Main St'),
    ('Jane Smith', 'jane@example.com', '9876543210', '456 Oak St'),
    ('Bob Johnson', 'bob@example.com', '5678901234', '789 Pine St');
    
    -- Sample Data for Restaurants
    INSERT INTO restaurants (name, cuisine_type, location) VALUES
    ('Tasty Bites', 'Italian', 'Downtown'),
    ('Spice Haven', 'Indian', 'Midtown'),
    ('Sushi Delight', 'Japanese', 'Uptown');
    
    -- Sample Data for Menu Items
    INSERT INTO menu_items (restaurant_id, name, price, description) VALUES
    (1, 'Margherita Pizza', 12.99, 'Classic tomato and cheese'),
    (1, 'Chicken Alfredo', 18.50, 'Creamy Alfredo sauce with grilled chicken'),
    (2, 'Chicken Tikka Masala', 15.75, 'Grilled chicken in a spiced tomato curry'),
    (3, 'Sashimi Platter', 24.99, 'Assorted slices of fresh raw fish');
    
    -- Sample Data for Orders
    INSERT INTO orders (customer_id, total_price, status) VALUES
    (1, 31.98, 'Delivered'),
    (2, 45.25, 'Processing'),
    (3, 22.50, 'Shipped');
    
    -- Sample Data for Order Items
    INSERT INTO order_items (order_id, item_id, quantity, subtotal) VALUES
    (1, 1, 2, 25.98),
    (2, 3, 1, 15.75),
    (3, 2, 3, 55.50);
    
    -- Sample Data for Delivery Staff
    INSERT INTO delivery_staff (name, phone, location) VALUES
    ('Mike Davis', '555-1234', 'Downtown'),
    ('Lisa Patel', '555-5678', 'Midtown'),
    ('Chris Johnson', '555-9876', 'Uptown');
    
    -- Sample Data for Delivery Assignments
    INSERT INTO delivery_assignments (order_id, staff_id) VALUES
    (1, 1),
    (2, 2),
    (3, 3);
    
    -- Sample Data for Reviews
    INSERT INTO reviews (customer_id, restaurant_id, order_id, rating, comment) VALUES
    (1, 1, 1, 5, 'Great pizza!'),
    (2, 2
    
    -- Select all data from each table
    --SELECT * FROM customers;
    --SELECT * FROM restaurants;
    --SELECT * FROM menu_items;
    --SELECT * FROM orders;
    --SELECT * FROM order_items;
    --SELECT * FROM delivery_staff;
    --SELECT * FROM delivery_assignments;
    --SELECT * FROM reviews;
    --This code creates tables for customers, restaurants, menu items, orders, order items,
    ```
    

## Types of Joins:

1. **Inner Join:**
    - Returns rows with matching values in both tables.
    - Example:
        
        ```sql
        --Retrieve orders with customer information.
        SELECT orders.order_id, customers.name AS customer_name, orders.total_price
        FROM orders
        INNER JOIN customers ON orders.customer_id = customers.customer_id;
        ```
        
2. **Left Join (or Left Outer Join):**
    - Retrieves all rows from the left table and matching rows from the right table.
    - Example:
        
        ```sql
        --Retrieve all customers and their orders, including those without orders.
        SELECT customers.name AS customer_name, orders.order_id, orders.total_price
        FROM customers
        LEFT JOIN orders ON customers.customer_id = orders.customer_id;
        ```
        
3. **Right Join (or Right Outer Join):**
    - Opposite of the left join; returns all rows from the right table and matching rows from the left table.
    - Example:
        
        ```sql
        --Retrieve all orders and their customers, including those without customers.
        
        SELECT orders.order_id, customers.name AS customer_name, orders.total_price
        FROM orders
        RIGHT JOIN customers ON orders.customer_id = customers.customer_id;
        ```
        
4. **Full Join (or Full Outer Join):**
    - Retrieves rows when there is a match in either the left or right table.
    - Example:
        
        ```sql
        --Retrieve all customers and their orders, showing unmatched records from both sides.
        SELECT customers.name AS customer_name, orders.order_id, orders.total_price
        FROM customers
        FULL JOIN orders ON customers.customer_id = orders.customer_id;
        ```
        
5. **Cross Join:**
    - Returns the Cartesian product of both tables, producing all possible combinations of rows.
    - Example:
        
        ```sql
        --Retrieve all combinations of customers and menu items.
        SELECT customers.name AS customer_name, menu_items.name AS item_name
        FROM customers
        CROSS JOIN menu_items;
        
        ```
        

## Practical Examples:

```sql
-- Beginner Level:

-- Question 1: Retrieve a list of employees with their corresponding department names.
SELECT employees.employee_name, departments.department_name
FROM employees
INNER JOIN departments ON employees.department_id = departments.department_id;

-- Question 2: Display all departments, including those without any employees.
SELECT departments.*, employees.employee_name
FROM departments
LEFT JOIN employees ON departments.department_id = employees.department_id;

-- Medium Level:

-- Question 3: List all orders along with customer names. Display "No Customer" for orders without customers.
SELECT orders.*, customers.name AS customer_name
FROM orders
LEFT JOIN customers ON orders.customer_id = customers.customer_id;

-- Question 4: Get a count of employees in each department. Include departments with zero employees.
SELECT departments.department_name, COUNT(employees.employee_id) AS employee_count
FROM departments
LEFT JOIN employees ON departments.department_id = employees.department_id
GROUP BY departments.department_name;

-- Advanced Level:

-- Question 5: Retrieve the total revenue from orders along with the names of customers who placed those orders.
SELECT orders.total_price, customers.name AS customer_name
FROM orders
LEFT JOIN customers ON orders.customer_id = customers.customer_id;

-- Question 6: Identify departments with the highest and lowest numbers of employees.
WITH EmployeeCounts AS (
    SELECT department_id, COUNT(employee_id) AS employee_count
    FROM employees
    GROUP BY department_id
)
SELECT departments.*, COALESCE(employee_count, 0) AS total_employees
FROM departments
LEFT JOIN EmployeeCounts ON departments.department_id = EmployeeCounts.department_id
ORDER BY total_employees DESC, department_id;
```

Understanding SQL joins, including the cross join, is pivotal for harnessing the full potential of relational databases. With these examples, you're equipped to navigate diverse scenarios and master the art of combining data seamlessly.
