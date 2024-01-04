#key points

How the data storage machines have evolved over the years
Why SQL is important and relvant in today’s industry
difference between sql and flat files(excel, google sheets etc)
How to query using SELECT and FROM keywords
why do we follow a sequence while querying
#Action items

 Install postgres db
 download the relation db paper
 create a leetcode and hackerrank free account
#Notes 💡 History and basics of databases

SEQUEL or sql was first developed by scientists from the IBM.

Database is a collection of interconnected tables

dbsample.png

We use sql to communicate with databases

Tables, Rows and Columns

In databases like spreadsheets we have rows and columns.
Columns in each table exist as headers and their data types are defined
Rows are dynamic than columns
relational-databases-for-dummies-fig1.jpg

Today there are several database products and vendors, we will stick to standard SQL

💡 Querying the database
For querying an existing table we use the below syntax

SELECT column_name
FROM table_name
SELECT keyword selects the necessary column names

FROM keywords defines which table we are using for querying

In the realm of databases, it's essential to treat them like machines that require precise details to yield the desired output. Therefore, we must provide accurate column names and ensure the table includes the necessary columns; otherwise, errors may occur.
