# SQL
SQL-Query-Log
Here's a log of my SQL skillset!

Aggregate Functions
AVG() - returns an average value
ROUND() to specify precision after a decimal
COUNT() - returns number of values
MAX() - returns maximum value
MIN() - returns minimum value
SUM() - returns the sum of all values

ALTER Table
Allows for changes to an existing table structure

Adding columns
ALTER TABLE table_name
ADD COLUMN new_col TYPE

Alter constraints
ALTER TABLE table_name
ALTER COLUMN col_name
SET DEFAULT value

(Can also ADD CONSTRAINT constraint_name in place of SET DEFAULT value)

Example Query:
ALTER TABLE information
RENAME TO new_info

ALTER TABLE new_info
RENAME COLUMN person TO people

ALTER TABLE new_info
ALTER COLUMN people DROP NOT NULL

BETWEEN
Used to match a value against a range of values.

SELECT COUNT column
FROM table
WHERE column BETWEEN 8 AND 9

Can input NOT BETWEEN to return values not inbetween the designated values.

CASE
Executes an SQL code when certain conditions are met, (similar to an IF/ELSE statement)

General CASE Example query:
SELECT customer_id,
CASE
WHEN (customer_id <= 100) THEN 'Premium' WHEN (customer_id BETWEEN 100 AND 200) THEN 'Plus'
ELSE 'Normal'
END AS customer_class
FROM customer

Another example:
SELECT
SUM(CASE rental_rate
WHEN 0.99 THEN 1
ELSE 0
END) AS number_of_bargains
FROM film

This would return a summed number of everything given the value 1
CAST
Converts one data type to another

Must be reasonable conversion
Ex: '5' to an integer will work, 'five' to an integer will not
Syntax:
SELECT CAST('5' AS INTEGER)

Can use in a SELECT query with a column name instead of a single instance.
Example:
SELECT CAST(date AS TIMESTAMP)
FROM table

Changing the case of a string
Upper Case
SELECT UPPER(email)
FROM customer

Lower Case
SELECT LOWER(email)
FROM customer

Title Case
SELECT INITCAP(title)
FROM film

CHECK
Creates more customized constraints that adhere to a certain condition.
Example: Making sure all inserted integer values fall below a certain threshold.

Example query:
CREATE TABLE employees(
emp_id SERIAL PRIMARY KEY,
first_name VARCHAR(50) NOT NULL,
last_name VARCHAR(50) NOT NULL,
birthdate DATE CHECK (birthdate > '1900-01-01'),
hire_date DATE CHECK (hire_date > birthdate),
salary INTEGER CHECK (salary > 0)
)

COALESCE
Accepts an ulimited number of arguments. It returns the first argument that is not null. If all arguments are null, the COALESCE function will return null.

Useful when querying a table that contains null values and substituting it with another value.
Example query:
SELECT item,(price - COALESCE(discount,0))
AS final FROM table

Example query:
SELECT COALESCE(country, 'Both countries') AS country,
COALESCE(medal, 'All medals') AS medal,
COUNT(* ) AS awards
FROM summer_medals
WHERE year = 2008 AND COUNTRY IN ('CHN, 'RUS')
GROUP BY ROLLUP(country, medal)
ORDER BY country ASC, medal ASC);

COUNT/COUNT DISTINCT
The COUNT function returns the number of input rows that match a specific condtion of a query.
COUNT DISTINCT will return only the distinct number of values from a column.

SELECT COUNT (name) FROM table

SELECT COUNT(DISTINCT name)
FROM table

CREATE TABLE
General syntax:

CREATE TABLE players(
player_id SERIAL PRIMARY KEY,
age INTEGER NOT NULL,
)

Another example:
CREATE TABLE account(
user_id SERIAL PRIMARY KEY,
username VARCHAR(50) UNIQUE NOT NULL,
password VARCHAR(50) NOT NULL,
email VARCHAR(250) UNIQUE NOT NULL,
created_on TIMESTAMP NOT NULL,
last_login TIMESTAMP
)

SERIAL - typical for the primary key type as it logs unique integer entries automatically upon insertion

DELETE
Removes rows from a table

Syntax:
DELETE FROM table
WHERE row_id = 1

Delete rows based on their presence in other tables
Example:
DELETE FROM tableA
USING tableB
WHERE tableA.id=tableB.id

Delete all rows from a table
DELETE FROM table

DROP
Removes a column in a table along with its indexes and constraints.
Does not remove columns used in views, triggers, or stored procedures without the CASCADE clause.

General syntax:
ALTER TABLE table_name
DROP COLUMN col_name

(Insert CASCADE at the very end to remove all dependencies)

Check for existence to avoid error:
ALTER TABLE table_name
DROP COLUMN IF EXISTS col_name

(Can drop multiple columns as well)

FRAMES
ROWS BETWEEN
ROWS BETWEEN [START] AND [FINISH]

n PRECEDING: n rows before the current row

CURRENT ROW: the current row

n FOLLOWING: n rows after the current row

Examples:
ROWS BETWEEN 3 PRECEDING AND CURRENT ROW
ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
ROWS BETWEEN 5 PRECEDING AND 1 PRECEDING

RANGE BETWEEN:
RANGE BETWEEN [START] AND [FINISH]
Similar to ROWS BETWEEN
RANGE treats duplicates in OVER's ORDER BY subclause as a single entity

GROUP BY
Aggregates columns per category.

Example syntax:
SELECT customer_id,SUM(amount)
FROM payment
GROUP BY customer_id
ORDER BY SUM(amount)
