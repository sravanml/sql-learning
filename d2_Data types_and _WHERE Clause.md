💡 **Key Points:**

- data types
- WHERE clause

**Action items**

- [ ]  Search for postgres data types documentation
- [ ]  practice two problems on w3 schools
- [ ]  find where WHERE clause cannot be applicable


✏️ **Notes:**

Main lecture notes


💡 Data types



- Data types serve as attributes defining the characteristics of information stored in a database, allowing machines to interpret and process data effectively.
- Programming languages like JavaScript and Python associate data types with variables, specifying the kind of data a variable can hold.
- While data types can differ across databases, common examples include VARCHAR(), INT(), DATE(), and DECIMAL(). These types play a vital role in structuring and managing diverse data within a database.
- For data types supported in the postgres, [please click on this link.](https://www.postgresql.org/docs/current/datatype.html)


💡 WHERE Clause



- WHERE clause is used for general filtering purposes and it follows after FROM keyword while writing the sql code
    
    ```sql
    
    SELECT column_name
    FROM table_name
    WHERE condition
    ```
    
- For example,
    - if we need values between any dates
    - if we need values with in any values
    - we can combine more operations using AND, OR, NOT keywords

*Summary ends here*
