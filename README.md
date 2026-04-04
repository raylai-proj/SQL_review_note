# SQL_review_note
This repo documents notes from reviewing SQL which the author took the notes while following SQL tutorial from [Data Engineer Academy](https://my.dataengineeracademy.com/course/view.php?id=26). The review note was taken based on MySQL and sometime included other platforms, e.g. PostgreSQL, SQL server, for difference comparison. This note included exercise answer from the tutorial as well as various methods which the authors prefered to. <br >
## Introduction: <br >
1. What is a database? Database is a digital collection of data for search, management, and analyze. <br >
2. Database analogy: database = bookstore, shelf = table, row = record, column = field (details e.g. title, author, genre) <br >
3. Database hierarchy: <br >
- Database = many tables <br >
  - table = many rows = data entries = data points <br >
	  - row = many columns = many fields (id, title, author, genre, primary key) <br >
4. Primary key = a column with ID REPRESENT a unique combination of columns <br >
-  primary key是ID 代表著 部分重要的column組合是唯一的(是部分column組合, 不是全部columns的組合都要唯一) <br >
5. 一個table的Primary key column 會作為foreign key column在別的table, 用來讓別的table可以藉由foreign key拿到原本primary key的table中其他column的資料 <br >
6. __Spreadsheet__ for small scale, non-shared, not secured <br >
7. __Database__ for large scale data, can shared and more secured <br >
8. Relational database = several related tables.<br >
-  Each related tables focus on unique combination entries (e.g. Customer, Device, Type).<br >
-  Each related tables linked using Keys (e.g. customer_id, book_id).<br >
-  E.g. __Customer table__ has __customer_id__. __Summary table__ has __customer_id__ and __book_id__ to combine an unique record. <br >
9. Database stores real-world data from social media to bank account. To work with data, first we learn database.<br >
10. Basic SQL data type: __VARCHAR__ (string,text=Name), __INT__ (number=age), __DATE__ (birthday, release date)<br >
11. SQL = Structured Query Language<br >
12. SQL is querying, not programming<br >
13. SQL has DML (data manipulation) and DDL (data definition)<br >
14. DML work with data inside tables: SELECT (get data), INSERT (add data), UPDATE (change data), Delete (delete data)<br >
15. DDL define database structure: CREATE (create table), ALTER (change table structure), DROP (remove table)<br >
16. DBA = database administrator<br >
## 17. SELECT FROM<br >
Syntax: `SELECT <column> FROM <table_name>` e.g. `SELECT Name FROM Customers_prc;` <br >
## 18. CREATE TABLE<br >
example:
```
CREATE TABLE Employees (
	Employee_ID INT PRIMARY KEY,
	Name CHARVAR(100),
	Department CHARVAR(50),
	Hire_Date DATE
);

--	CREATE TABLE must include:
--		table name (Students)
--		column name (ID, Name, Major),
--		data type (INT, VARCHAR, DATE)
--		#	VARCHAR(50): 50 = Max length
--		Option: PRIMARY KEY
```
## 19. ALTER TABLE<br >
example: <br >
```
ALTER TABLE Students
	ADD Email VARCHAR(100);

--	Add new column "Email": need:
	1. ALTER TABLE,
	2. table name: Students,
	3. action: ADD, REMOVE,
	4. new col name + data type
```
## 20. DROP TABLE<br >
example: <br >
```
DROP TABLE Students;

--	delete table and delete all data in that table
```
## Add row: INSERT, change row: UPDATE, remove row: DELETE
## 21. INSERT INTO <table name> VALUES<br >
syntax:
```
INSERT INTO <table name> VALUES
	(<column1 value1>, <column2 value1>,..., <columnN value1>),
	(<column1 value2>, <column2 value2>,..., <columnN value2>);
```
example: <br >
```
INSERT INTO students VALUES
	(1, 'Alice', 'Biology'),
	(2, 'Bob', 'History'),
	(3, 'Cathy', 'Math');

--	"INSERT INTO" match type, primary key no duplicate, ; in the end
```
## 22. UPDATE SET WHERE
syntax:
```
UPDATE <table_name>
	SET <target_column> = <new_value>
	WHERE <other_column> = <current_value>;
```
example: <br >
```
UPDATE students
	SET Major = 'Chemistry', Name = 'Alicia'
	WHERE ID = 1;

--	WHERE is filter to select matched rows
--	Always include WHERE to prevent select all rows
```
## 23. DELETE
DELETE = delete a row<br >
syntax: <br >
```
DELETE FROM <table_name> WHERE <column_name> = <current_value>
```
example: <br >
```
DELETE FROM students WHERE ID = 1;

--	if ID = 1 row has foreign key columns,
--	we have to first delete referenced row, which has primary key, in sub-table 
```
## 24. SELECT FROM
syntax: <br >
```
SELECT <column1>, <column2>
FROM <table_name>;
```
example: <br >
```
SELECT email AS "Customer_Email"
FROM customers;

--	columns will be listed in order which we specify,
--	e.g. SELECT 2, 1, 3 FROM table; => 2,1,3
```
## 25. AS Alias 別名
example: <br >
```
SELECT
	col1 AS alias1,
	col2 AS alias2
FROM table1 AS alias3;

--	可以設定col跟table的Alias aka. 別名
```
## 26. WHERE
WHERE filter rows: use cols condition to find satisfied rows<br >
```
SELECT *
FROM employees
WHERE salary > 50000;

--	SELECT <col1>, <col2>, <col3>
--	FROM <table_name>
--	WHERE <condition>;
```
### 27. You can, but you don't have to include column in WHERE clause in SELECT list<br >
```
SELECT product_name, unit_price 
FROM products 
WHERE unit_price < 50;
```
V.S.
```
SELECT product_name
FROM products 
WHERE unit_price < 50;

--	don't have to show the filter condition column unit_price
```
## 28. single quote V.S. double quote <br >
Only use __single quote__ 'sales' for __Values__, e.g. VARCHAR, TIMESTAMP<br >
Only use __double quote__ or __no quote__ for Column name, Table name, or Alias<br >
1. double quote for __space__ in name: "Product Name"<br >
2. double quote for __reserved word__: "Where"<br >
3. double quote for __forcing case sensitivity__: "Product_Name" no matching on product_name<br >
```
SELECT *
FROM employees
WHERE salary > 50000 AND departments = 'Sales';

--	filter departments has to be 'Sales' in WHERE clause
--	WHERE can use <, >, <=, >=, <>, =
--	<>: column not this value: for filtering Value, e.g. salary <> 50000
--	NOT: Logical operator for condition: NOT <condition>
```
## 29. NOT<br >
Use __NOT__ in WHERE filter: <br >
1. NOT country = 'USA';<br >
2. department NOT IN ('Sales', 'HR', 'IT');<br >
	- __IN__ is broader than =<br >
3. salary NOT BETWEEN 30000 AND 50000;<br >
	- __BETWEEN__ ... __AND__: BETWEEN always followed by AND with two values<br >
```
SELECT *
FROM customers
WHERE NOT country = 'USA';

--	NOT <column_name> = <value>
```
## 30.
```
SELECT *
FROM employees
WHERE department NOT IN ('Sales', 'HR', 'IT') AND salary NOT BETWEEN 30000 AND 50000;
	
--	IN must follow by (), the () here is a Set, usually () is for parse order
--	1 or 2 condition: AND at the same line,
--	3 or more conditions: break AND into multiple lines
--		Leading operator style: Put AND, OR at the start of new line
--		SQL parser has operator precedence, so it won't confuse at two ANDs
```
## 31. Escape from ' by '<br >
```
SELECT *
FROM employees
WHERE name = 'O''Relly';

--	when value has ', add ' for escape, so 'Relly => ''Relly, and single quote for value 'O''Relly'
```
## 32. LIKE % _ pattern matching<br >
```
SELECT *
FROM customers
WHERE customer_name LIKE 'joh%';

--	LIKE 'joh%' is pattern matching:
--	1. LIKE 'joh%' match customer_name start with 'joh', and can have any number characters after it
--	2. LIKE 'joh_' match only 1 character after 'joh'
--		% match any number characters
--		_ match 1 character
--		each _ replace 1 char (J___ match John)
```
## 33. LIKE _ pattern matching<br >
```
SELECT *
FROM customers
WHERE customer_name LIKE 'Sm_th';

--	LIKE 'Sm_th' match 'Smith'
```
## 34. LIKE % pattern matching<br >
```
SELECT *
FROM products
WHERE product_name LIKE '%phone%';

--	LIKE '%phone%' match 'smartphone' and 'phone_screen'
```
## 35. IS NULL, IS NOT NULL<br >
```
SELECT *
FROM employees
WHERE manager_id IS NULL;

--	IS NULL / IS NOT NULL filtering column doesn't have / have value
--	NULL = Unknown = missing information
--	IS NULL / IS NOT NULL = missing data check = data quality check
--	NULL is not 0, NULL is not empty string ''
--	can only use IS NULL, others are wrong, e.g. = NULL, <> NULL
```
## 36. DEFAULT<br >
```
CREATE TABLE orders(
	order_id INT,
	status VARCHAR(20) DEFAULT 'pending'
);

--	DEFAULT give default value if original value = NULL, or didn't provide original value
```
## 37. Operator precedence<br >
```
SELECT *
FROM orders
WHERE (order_amount > 1000 AND order_status = 'Pending')
OR (order_status = 'Processing');

--	operator precedence = logical order: NOT > AND > OR
--	parenthesis () used to run inside first
```
## 38. Operator precedence 2<br >
```
SELECT
	product_id,
	product_name,
	category
FROM products
WHERE product_name LIKE '%e'
AND (category = 'Electronics' OR category = 'Appliances');

--	operator precedence AND > OR: so need () to run OR first
--	if know full string, use =, e.g. WHERE product_name = 'smartphone'
--	if only know partial string, use LIKE, e.g. WHERE product_name LIKE '%e'
```
## 39. IN replace = + OR<br >
```
SELECT
	product_id,
	product_name,
	category
FROM products
WHERE product_name LIKE '%e' AND category IN ('Electronics', 'Appliances');

--	IN can match multiple strings, so IN can replace a lot of = + OR, e.g.
--	simple: (category = 'Electronics' OR category = 'Appliances')
--	better: category IN ('Electronics', 'Appliances')
```
## 40. IN subquery<br >
```
SELECT *
FROM orders
WHERE customer_id IN (
	SELECT customer_id
	FROM customers
	WHERE customer_name = 'John Smith'
);

--	IN is perfect for subquery in WHERE clause, e.g. find 'customer_id' where 'customer_id' match in customers table
```
## 41. IN combine operators<br >
```
SELECT *
FROM products
WHERE unit_price > 100 AND category IN ('Electronics', 'Appliances')
OR category = 'Office supplies';

--	IN can use along with >, AND, OR
```
## 42. LIKE vs IN vs BETWEEN<br >
LIKE: pattern match<br >
IN: match multiple values<br >
BETWEEN...AND: match range-based value<br >

## 43. BETWEEN...AND<br >
```
SELECT *
FROM products
WHERE unit_value BETWEEN 50 AND 100;

--	BETWEEN 50 AND 100 include 50 and 100
--	BETWEEN...AND is syntax. the server won't confuse with logical operator AND
```
## 44. Date Format<br >
```
SELECT
	order_id,
	order_date,
	order_amount
FROM orders
WHERE order_date BETWEEN '2023-01-01' AND '2023-06-01';

--	Date format: 'YYYY-MM-DD' or 'YYYY-MM-DD HH:MM:SS'
```
## 45. BETWEEN subquery<br >
```
SELECT
	product_name,
	unit_price
FROM products
WHERE unit_price BETWEEN (
	SELECT AVG(unit_price) FROM products
) AND (
	SELECT MAX(unit_price) FROM products
);

--	BETWEEN A AND B: A, B accept single value => need aggregate function in subquery => not common in subquery
--	IN accept a set of value => more common in subquery
```
## 46. NOT BETWEEN<br >
```
SELECT *
FROM employees
WHERE salary NOT BETWEEN 40000 AND 60000 AND department = 'Sales';

--	NOT at front: NOT BETWEEN, NOT IN
--	NOT at mid: IS NOT NULL
```
## 47. ANY subquery<br >
```
SELECT
	product_name,
	unit_price
FROM products
WHERE unit_price > ANY (
	SELECT unit_price
	FROM products
	WHERE catergory = 'Toy'
);

--	find all product_names that is more expensive than the CHEAPEST Toy product
--	it's > return True/False
--	ANY doesn't return True/False, ANY help > return True/False
--	ANY = LOWEST
```
## 48. ALL subquery<br >
```
SELECT
	product_name,
	unit_price
FROM products
WHERE unit_price > ALL (
	SELECT unit_price
	FROM products
	WHERE category = 'Toy'
);

--	find all product_names that is more expensive than the MOST EXPENSIVE Toy product
--	it's > return True/False
--	ALL doesn't return True/False, ALL help > return True/False
--	ALL = HIGHEST
```
## 49. SQL Comment format<br >
SQL comment example (ctrl + space): --	\<comment\><br >

## 50. IS NULL slows SQL server<br >
```
SELECT
	order_id,
	order_date AS date
FROM orders
WHERE customer_id IS NULL;

--	We can rename column using AS (order_date AS date)
--	We don't search the whole table for missing data row.
--	It has to go through whole table which makes application VERY SLOW!
```
## 51. ORDER BY, ASC, DESC<br >
```
SELECT
	first_name,
	last_name
FROM employees
ORDER BY first_name ASC, last_name DESC;

--	sort first_name ascending, if tie, sort last_name descending
--	ORDER BY = sorting, ASC = ascending, DESC = descending, comma , = next sorting order
```
## 51-2. ORDER BY IS NULL<br >
```
SELECT *
FROM employees
ORDER BY manager_id IS NULL, manager_id ASC;

--	Sort manager_id ascending, but NULL manager_id at bottom
--	In MySQL: NULL = -infinity = smallest
--	In PostgreSQL: NULL = infinity = highest
--	ORDER BY manager_id will put NULL at the top (NULL = smallest)
--	In MySQL, if we want to hide NULL in ascending: we need ascending but NULL at bottom:
--		1. ORDER BY manager_id IS NULL, manager_id ASC;
--		2. null manager_id = 1, non null manager_id = 0 => null at bottom, non null at top
--		3. then, ORDER BY manager_id ASC sort manager_id ascending
--	In MySQL, if we want Descending but NULL at top:
--		1. ORDER BY manager_id IS NOT NULL, manager_id DESC;
--	clause order: SELECT, FROM, WHERE, ORDER BY
```
## 52. LIMIT<br >
```
SELECT
 	product_name,
	unit_price
FROM products
ORDER BY unit_price DESC, LIMIT 5;

--	LIMIT 5 = pick first 5 rows
--	clause order: SELECT, FROM, WHERE, ORDER BY, LIMIT
```
## 53. ORDER BY TIME<br >
```
SELECT *
FROM orders
ORDER BY order_date DESC;

--	latest order_date on top
```
## 54. OFFSET<br >
```
SELECT *
FROM orders
LIMIT 4 OFFSET 2;

--	OFFSET 2 = skip first 2 rows
--	LIMIT 4 OFFSET 2; = LIMIT 2, 4;
--	use WHERE to replace OFFSET because WHERE use B-tree search faster than OFFSET
--	e.g. OFFSET 2; = WHERE order_id > 2;
```
## 55. aggregate function: COUNT() + GROUP BY clause<br >
```
SELECT
	department,
	COUNT(*)
FROM employees
GROUP BY department;

--	1. show how many employees in each department
--		how many employees: COUNT(*) or COUNT(employee_id)
--		in each department: GROUP BY department
--		show department for its COUNT: SELECT department
--	2. clause order: SELECT, FROM, WHERE, GROUP BY, HAVING, ORDER BY, LIMIT, OFFSET
--	3. GROUP BY is a clause to group output from WHERE by certain column, or multiple columns
--	4. AVG, COUNT, MAX, MIN, SUM are aggregate function (Group summary), only use in:
		SELECT (see the summary)
		HAVING (filter the summary)
		ORDER BY (sort the summary)
--	5. aggregate function cannot use in WHERE, so aggregate function has to be in subquery if in WHERE
--	6. syntax: COUNT(<column>)
--		COUNT(*) => count all rows
--		COUNT(column_name) => count rows "have value" in that column 
```
## 56. aggregate function: AVG() + GROUP BY clause<br >
```
SELECT
	department,
	AVG(salary) AS average_salary
FROM employees
GROUP BY department;

--	1. show average salary in each department
--		average salary: AVG(salary) AS average_salary
--		in each department: GROUP BY department
--		show department for its average salary: SELECT department
--	2. execute order: FROM, WHERE, GROUP BY, HAVING, SELECT
```
## 57. HAVING<br >
```
SELECT
	department,
	AVG(salary) AS average_salary
FROM employees
WHERE manager_id IS NOT NULL
GROUP BY department
HAVING AVG(salary) > 50000
ORDER BY average_salary DESC LIMIT 2 OFFSET 2;

--	1. find average salary of each department that over $50,000 in descending order.
--		Only calculate employees that are not managers.
--		Skip the highest 2 and show the next 2 departments and average salary.
--	2. clause order: SELECT, FROM, WHERE, GROUP BY, HAVING, ORDER BY, LIMIT, OFFSET
--	3. execute order: FROM, WHERE, GROUP BY, HAVING, "SELECT", ORDER BY, LIMIT, OFFSET
--	4. WHERE filter single rows => filter column value IS NOT NULL
--	5. HAVING filter group after GROUP BY
--	6. you have to write aggregate function 2 times in SELECT and HAVING (HAVING no Alias, and SELECT execute after HAVING),
		but SQL server only compute it once (type twice for clear)
--	7. MySQL: let HAVING use alias from SELECT
		PostgreSQL: have to type 2 times
```
## 58. aggregate function SUM()<br >
```
SELECT
	department,
	SUM(salary)
FROM employees;
-- GROUP BY department;

--	find the sum of salary of whole department (or of each department)
--	the whole table is default a group if no GROUP BY
--	without GROUP BY, the output will be one row showing all salary sum up,
--		and the department value is meaningless (the server random pick one)
```
## 59. COUNT + WHERE<br >
```
SELECT COUNT(*) AS high_salary
FROM employees
WHERE salary > 50000;

--	count number of employees with salary over 50000 as high_salary
--	COUNT(*) works with WHERE to output the number of employees whose salary higher than 50000
```
