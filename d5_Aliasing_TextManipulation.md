💡 **Key Points:**

- Aliasing
- LIKE function

Action items

- [ ] Scenarios where like is used and = is used

_Note ends here_

✏️ **Notes:**

Main lecture notes

💡 **Renaming column names**

we can use the `AS` keyword to rename the column in the output.

This doesn't change any of the underlying data or the column name in the database. It just renames it for the results of this one query. This is a useful practice to give our columns extremely **readable** and **explainable** names.

applying transformations on the output of SQL queries is fairly common.

```sql
SELECT order_id, customer_id, restaurant_id, order_status AS status
FROM orders;
```

- Code for sample data
  ```sql
  -- Customers Table
  CREATE TABLE Customers (
      customer_id SERIAL PRIMARY KEY,
      name VARCHAR(255) NOT NULL,
      email VARCHAR(255) UNIQUE NOT NULL,
      phone VARCHAR(20),
      address TEXT
  );

  -- Restaurants Table
  CREATE TABLE Restaurants (
      restaurant_id SERIAL PRIMARY KEY,
      name VARCHAR(255) NOT NULL,
      cuisine_type VARCHAR(50),
      location VARCHAR(255)
  );

  -- Menu Items Table
  CREATE TABLE MenuItems (
      item_id SERIAL PRIMARY KEY,
      restaurant_id INTEGER REFERENCES Restaurants(restaurant_id),
      name VARCHAR(255) NOT NULL,
      price DECIMAL(10, 2) NOT NULL,
      description TEXT
  );

  -- Orders Table
  CREATE TABLE Orders (
      order_id SERIAL PRIMARY KEY,
      customer_id INTEGER REFERENCES Customers(customer_id),
      order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
      total_price DECIMAL(10, 2) NOT NULL,
      status VARCHAR(50) DEFAULT 'Pending'
  );

  -- Order Items Table
  CREATE TABLE OrderItems (
      order_item_id SERIAL PRIMARY KEY,
      order_id INTEGER REFERENCES Orders(order_id),
      item_id INTEGER REFERENCES MenuItems(item_id),
      quantity INTEGER NOT NULL,
      subtotal DECIMAL(10, 2) NOT NULL
  );

  -- Delivery Staff Table
  CREATE TABLE DeliveryStaff (
      staff_id SERIAL PRIMARY KEY,
      name VARCHAR(255) NOT NULL,
      phone VARCHAR(20),
      location VARCHAR(255)
  );

  -- Delivery Assignments Table
  CREATE TABLE DeliveryAssignments (
      assignment_id SERIAL PRIMARY KEY,
      order_id INTEGER REFERENCES Orders(order_id),
      staff_id INTEGER REFERENCES DeliveryStaff(staff_id),
      assignment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );

  -- Reviews Table
  CREATE TABLE Reviews (
      review_id SERIAL PRIMARY KEY,
      customer_id INTEGER REFERENCES Customers(customer_id),
      restaurant_id INTEGER REFERENCES Restaurants(restaurant_id),
      order_id INTEGER REFERENCES Orders(order_id),
      rating INTEGER NOT NULL,
      comment TEXT,
      date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );
  ```
  ```sql
  -- Sample data for Customers Table
  INSERT INTO Customers (name, email, phone, address) VALUES
      ('John Doe', 'john@example.com', '+91 9876543210', '123 Main St, City'),
      ('Alice Smith', 'alice@example.com', '+91 8765432109', '456 Oak St, Town'),
      ('Bob Johnson', 'bob@example.com', '+91 7654321098', '789 Pine St, Village'),
      ('Eva Davis', 'eva@example.com', '+91 6543210987', '101 Cedar St, Suburb'),
      ('Charlie Brown', 'charlie@example.com', '+91 5432109876', '202 Elm St, Metro');

  -- Sample data for Restaurants Table
  INSERT INTO Restaurants (name, cuisine_type, location) VALUES
      ('Pizza Palace', 'Italian', 'Downtown'),
      ('Sushi Haven', 'Japanese', 'Uptown'),
      ('Burger Barn', 'American', 'Midtown'),
      ('Taco Town', 'Mexican', 'Eastside'),
      ('Curry Corner', 'Indian', 'Westside');

  -- Sample data for MenuItems Table
  INSERT INTO MenuItems (restaurant_id, name, price, description) VALUES
      (1, 'Margherita Pizza', 10.99, 'Classic tomato and cheese'),
      (1, 'Pepperoni Pizza', 12.99, 'Spicy pepperoni, tomato, and cheese'),
      (2, 'Sashimi Combo', 18.99, 'Assortment of fresh sashimi slices'),
      (2, 'California Roll', 14.99, 'Avocado, crab, and cucumber rolled in seaweed'),
      (3, 'Classic Burger', 8.99, 'Beef patty, lettuce, tomato, and mayo'),
      (3, 'BBQ Chicken Burger', 10.99, 'Grilled chicken, BBQ sauce, and coleslaw'),
      (4, 'Taco Platter', 9.99, 'Assorted tacos with seasoned meat and toppings'),
      (4, 'Quesadilla', 7.99, 'Cheese, veggies, and choice of meat folded in a tortilla'),
      (5, 'Chicken Curry', 12.99, 'Spicy chicken curry with basmati rice'),
      (5, 'Vegetable Biryani', 11.99, 'Fragrant rice with mixed vegetables and spices');

  -- Sample data for Orders Table
  INSERT INTO Orders (customer_id, total_price, status) VALUES
      (1, 23.98, 'Delivered'),
      (2, 15.50, 'Pending'),
      (3, 32.97, 'Processing'),
      (4, 27.75, 'Delivered'),
      (5, 19.99, 'Pending');

  -- Sample data for OrderItems Table
  INSERT INTO OrderItems (order_id, item_id, quantity, subtotal) VALUES
      (1, 1, 2, 21.98),
      (2, 3, 1, 15.50),
      (3, 5, 3, 32.97),
      (4, 8, 2, 27.75),
      (5, 10, 1, 19.99);

  -- Sample data for DeliveryStaff Table
  INSERT INTO DeliveryStaff (name, phone, location) VALUES
      ('Sam Deliveryman', '+91 7654321098', 'City Center'),
      ('Emma Courier', '+91 9876543210', 'Suburb Area'),
      ('Leo Rider', '+91 8765432109', 'Downtown'),
      ('Mia Driver', '+91 6543210987', 'Uptown'),
      ('Noah Messenger', '+91 5432109876', 'Midtown');

  -- Sample data for DeliveryAssignments Table
  INSERT INTO DeliveryAssignments (order_id, staff_id) VALUES
      (1, 1),
      (2, 2),
      (3, 3),
      (4, 4),
      (5, 5);

  -- Sample data for Reviews Table
  INSERT INTO Reviews (customer_id, restaurant_id, order_id, rating, comment) VALUES
      (1, 1, 1, 5, 'Great pizza!'),
      (2, 2, 2, 4, 'Good sushi, but delivery took a bit long'),
      (3, 3, 3, 3, 'Burger was okay, expected better'),
      (4, 4, 4, 5, 'Excellent service and prompt delivery'),
      (5, 5, 5, 4, 'Delicious curry, but portion size could be larger');
  ```

💡 Filtering: LIKE, Percentage and Underscore

we've always compared text with exact matches like product = ‘product A’.but often you will encounter situations where you want to match some partial text.for example app reviews, feedback surveys, customer mails etc.

The **LIKE** operator allows us to match text data against a pattern. It can help filter based on substring, prefix, or suffix text. There are two wildcard operators that are used with LIKE. The `%` operator matches any string of zero or more characters. The `_` (underscore) character matches any single character.

**Examples:**

- '%s' matches any string that ends with s, so it matches 's', 'ends' and 'looks'. On the other hand, it does not match 'sit', 'test' or 'ghost'.
- '%s\_' matches any string whose second to last character is s, so it matches 'test' and 'ghost'
- '\_s%' matches any string whose second character is s, so it matches 'assign' but not 'test'

```sql
-- Retrieve the names and phone numbers of all customers
-- whose phone numbers end with '1234'.
SELECT name, phone_number
FROM Customers
WHERE phone_number LIKE '%1234';
```

```sql
--List the names of restaurants that have 'Pizza' in their name.
SELECT name
FROM Restaurants
WHERE name LIKE '%Pizza%';
```

```sql
--Find the menu items containing the word 'Spicy' in their names.
SELECT name
FROM MenuItems
WHERE name LIKE '%Spicy%';
```

```sql
--Retrieve the names and addresses of customers whose names start with 'A' and end with 'n'.
SELECT name, address
FROM Customers
WHERE name LIKE 'A%n';
```

```sql
--Identify the delivery staff members with 'C__rier' in their job title (where the underscores (_) represent two missing characters).
SELECT name
FROM DeliveryStaff
WHERE job_title LIKE '%C__rier%';
```

```sql
--Find the reviews that contain the word 'Exc_llent' in their comments (where the underscore (_) represents a single missing character).
SELECT comment
FROM Reviews
WHERE comment LIKE '%Exc_llent%';
```

```sql
--Retrieve the names and addresses of customers whose names start with 'M' and have an 'e' as the third character.
SELECT name, address
FROM Customers
WHERE name LIKE 'M_e%';
```
