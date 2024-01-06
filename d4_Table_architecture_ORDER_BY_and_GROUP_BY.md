
💡 **Key Points:**

- Basic Table creation
- Basic functions

Action items

- [ ]  Research on how databases are executed
 

 

*Note ends here*


✏️ **Notes:**

Main lecture notes


💡 How databases work in real time ?



The sequence of SQL keywords in a query follows a specific pattern for effective data retrieval and manipulation:

1. **SELECT:** Specify the columns you want to retrieve data from.
2. **FROM/JOIN:** Indicate the tables involved in the query, and specify how they are related.
3. **WHERE:** Filter the rows based on specified conditions.
4. **GROUP BY:** Group the results based on certain columns.
5. **HAVING:** Apply conditions to grouped data.
6. **ORDER BY:** Sort the results based on specified columns.
7. **LIMIT/OFFSET:** Control the number of rows returned and start position for pagination.

These keywords collectively form the structure of a SQL query, providing a systematic way to retrieve, filter, and organize data from a database.

Also, understanding the logical processing order of SQL queries can indeed help developers optimize and write more efficient code. While the keywords are written in a certain order in the SQL query, the database engine processes them in a different sequence:

1. **FROM/JOIN:** The database retrieves the data from the specified tables or performs necessary joins.
2. **WHERE:** Filters the rows based on the conditions specified in the WHERE clause. This helps reduce the dataset early in the process.
3. **GROUP BY:** If used, the data is grouped according to the specified columns. This is typically followed by aggregate functions.
4. **HAVING:** Filters the grouped results based on conditions. It operates similarly to the WHERE clause but is applied after the GROUP BY phase.
5. **SELECT:** Specifies the columns to be included in the result set. This is done after the initial data retrieval and filtering.
6. **ORDER BY:** Sorts the result set as the final step before presenting the output.
7. **LIMIT/OFFSET:** If used, limits the number of rows in the final result set.

Understanding this execution order allows developers to optimize queries by placing conditions in the WHERE clause to filter data early in the process, reducing the amount of data that needs to be processed and improving performance. It's a valuable insight for writing efficient and well-performing SQL code.

![execution_order1.jpeg](https://prod-files-secure.s3.us-west-2.amazonaws.com/1ad15fa5-a02a-4764-88da-60131e3283ee/06a3daf0-89ff-4492-b0e8-bdd4830fe3c8/execution_order1.jpeg)


💡 Exploring the data : Agg. Functions



So far, we've generally stuck to getting specific rows from a table. This has helped us answer many different questions. But often when we report data in dashboards, presentations, or emails, we'll need to aggregate information like computing averages, counts, sum, min, max etc.

- `SUM`: calculate the sum across the different rows
- `COUNT`: number of entries in this column
- `AVG`: average of all values in this column
- `MIN`: minimum of all values in this column
- `MAX`: maximum of all values in this column

In SQL, **GROUP BY** organizes rows with similar values in a specific column, letting you apply functions to each group. It's like sorting a playlist by artist—you can see how many songs each artist has. It's handy for summarizing and analyzing data with shared characteristics.

At this point, some of you may be thinking: *I now understand how to compute statistics on an entire table (like revenue of all time, orders across all time), but how do I compute these statistics with respect to something?*

Let's take a look at this together. Some examples of this type of query might be:

- Instead of finding total revenue of all time ➡️ finding revenue per quarter
- Instead of finding the average price of listing across all listings ➡️ finding the average price of listing per neighbourhood
- Instead of finding the total number of students on Uplimit ➡️ finding the number of students for each course

The HAVING clause is used to filter the result set based on a condition on a grouped column or an aggregate function.

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

- WHERE clause is used for general filtering purposes and it follows after FROM keyword while writing the sql code
    - Retrieve the total revenue for each day:
    
    ```sql
    
    SELECT date, SUM(revenue) FROM sales GROUP BY date;
    ```
    
    - Retrieve the total revenue for each product:
    
    ```sql
    
    SELECT product, SUM(revenue) FROM sales GROUP BY product;
    ```
    
    - Retrieve the top-selling products in descending order by quantity sold:
    
    ```sql
    SELECT product, SUM(quantity) FROM sales GROUP BY product ORDER BY SUM(quantity) DESC;
    ```
    


💡 Order By



- The ORDER BY clause in SQL arranges query results based on specified columns and sorting orders. It's like organizing a library by book title or author. This helps present data in a structured way, making it easier to find, analyze, and comprehend.
- It is considered a best practice to explicitly write "ASC" in the ORDER BY clause, although databases typically default to ascending order. Being explicit enhances code readability and avoids potential confusion, ensuring that sorting is done in the intended order.
    
    ```sql
    SELECT product, SUM(quantity) FROM sales GROUP BY product ORDER BY SUM(quantity) DESC;
    ```
    

*Summary ends here*
