
💡 **Key Points:**

- Basic Table creation
- Basic functions

Action items

- [ ]  Research on LIMIT



✏️ **Notes:**

Main lecture notes


💡 Table creation



- During the creation of tables, the CREATE keyword is employed. Typically, when designing database tables, it is essential to specify their name, data type, and optionally, their cardinality and null representation. While constructing a single table, the elements marked with stars (*) are not mandatory.
- Think of creating a table like designing a spreadsheet for tracking sales. Each row is a sale, with an ID, sale date, product name, quantity sold, and revenue earned. It's a structured way to organize and manage sales information in a database.

```sql
CREATE TABLE sales (
id INT,
sale_date DATE,
product VARCHAR(50),
quantity INT,
revenue DECIMAL(10,2)
);
```

```sql
INSERT INTO sales (id, date, product, quantity, revenue)
VALUES
  (1, '2021-01-01', 'Product A', 10, 100.00),
  (2, '2021-01-02', 'Product B', 5, 50.00),
  (3, '2021-01-03', 'Product C', 15, 150.00),
  (4, '2021-01-04', 'Product A', 8, 80.00),
  (5, '2021-01-05', 'Product B', 12, 120.00),
  (6, '2021-01-06', 'Product C', 20, 200.00),
  (7, '2021-01-07', 'Product A', 6, 60.00),
  (8, '2021-01-08', 'Product B', 18, 180.00),
  (9, '2021-01-09', 'Product C', 10, 100.00),
  (10, '2021-01-10', 'Product A', 4, 40.00);
```


💡 Exploring the data



- WHERE clause is used for general filtering purposes and it follows after FROM keyword while writing the sql code
    - Retrieve all the data from the sales table:
    
    ```sql
    
    SELECT * 
    FROM sales;
    ```
    
    - Retrieve all the data from the sales table and limit it to 5:
    
    ```sql
    
    SELECT * FROM sales
    LIMIT 5;
    ```
    
    - Retrieve data only for a specific product, such as "Product A":
    
    ```sql
    SELECT * FROM sales 
    WHERE product = 'Product A';
    ```
    

*Summary ends here*
