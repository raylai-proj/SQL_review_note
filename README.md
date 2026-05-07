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
## 20-1. Add row: INSERT, change row: UPDATE, remove row: DELETE<br >
## 21. INSERT INTO \<table name\> VALUES<br >
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
## 22. UPDATE SET WHERE<br >
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
## 23. DELETE<br >
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
## 24. SELECT FROM<br >
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
## 25. AS Alias 別名<br >
example: <br >
```
SELECT
	col1 AS alias1,
	col2 AS alias2
FROM table1 AS alias3;

--	可以設定col跟table的Alias aka. 別名
```
## 26. WHERE<br >
WHERE filter rows: use cols condition to find satisfied rows<br >
```
SELECT *
FROM employees
WHERE salary > 50000;

--	SELECT <col1>, <col2>, <col3>
--	FROM <table_name>
--	WHERE <condition>;
```
## 27. You can, but you don't have to include column in WHERE clause in SELECT list<br >
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
## 30. IN
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
## 60. DISTINCT<br >
```
SELECT DISTINCT department
FROM employees;

--	show all departments
--	DISTINCT show the same department only once (only show the first one)
--	DISTINCT mostly use in SELECT clause
--	1. DISTINCT used when you only want to show unique group from a list
--	2. GROUP BY used when you want to show unique group from a list and do math on those group
```
## 61. DISTINCT usage<br >
1.	find (show) unique category:
```
SELECT DISTINCT department
FROM employees;

--	output each department once
--	same as
--	SELECT department
--	FROM employees
--	GROUP BY department;
```
2. count (show) "how many different values" in that column:
```
SELECT COUNT(DISTINCT department)
FROM employees;

--	output a number: how many (different) department do we have in employees?
```
3. show unique combination:
```
SELECT DISTINCT department, manager_id
FROM employees;

--	output unique combination of department and manager_id
```
## 62. Alias hidden logic<br >
```
SELECT
	department,
	COUNT(*) AS "EmployeeCount"
FROM employees
GROUP BY department
ORDER BY "EmployeeCount" DESC;

--	Sometimes MySQL will treat "EmployeeCount" as text string,
--	and based on hidden logic, it create value "EmployeeCount" and append after each row
--	so the ORDER BY look into "EmployeeCount" value in every row, which is the same word, and didn't sort anything.
--	Fix: cancel double quotes both in Alias and ORDER BY can fix it.
```
## 63. Subquery<br >
```
SELECT
	product_name,
	unit_price
FROM products
WHERE unit_-price > (SELECT AVG(unit_price) FROM products);

--	show product_name with unit_price higher than average unit_price
```
1. subquery = another query in main query<br >
2. subquery must have parenthesis ()<br >
3. subquery 1 line: return 1 value only (filtering by aggregate function, e.g. AVG())<br >
4. subquery multiple lines: used for correlated subquery<br >
5. subquery can return __1 value__, __1 row(with multiple columns)__, or __1 table__<br >
6. subquery usecase:<br >
	1. subquery in WHERE:<br >
	```
 	--	WHERE cannot use aggregate function (AVG(), MIN(), MAX(), COUNT(), SUM())
 	--	but can put subquery in WHERE, and put aggregate function in subquery to filter 1 value
 	SELECT
 		product_name,
 		unit_price
 	FROM products
 	WHERE unit_price > (SELECT AVG(unit_price) FROM products);

 	-- subquery in WHERE doesn't need to set an Alias
 	```
 	2. subquery in FROM: used to create __"Derived table"__ (can do aggregate functions __"Twice"__ in a query)<br >
	```
 	SELECT AVG(dept_total)
 	FROM (
 		SELECT
 			department,
 			COUNT(*) AS dept_total
 		FROM employees
 		GROUP BY department
 	) AS summary_table;

 	--	find average number of employees in each department
 	```
 	3. subquery in SELECT: create a __"Calculated column"__<br >
	```
 	SELECT
 		first_name,
 		last_name,
 		salary,
 		(
 			SELECT AVG(salary)
 			FROM employees e1
 			WHERE e1.department = e2.department
 		) AS average_salary
 	FROM employees e2;

	--	create a calculated column showing average salary of current department
 	-- 	e1, e2 are employees table alias
 	--	1. set FROM table alias: FROM employees e1
 	--	2. set SELECT column alias: SELECT COUNT(*) AS row_number
 	```
 	4. subquery in HAVING:<br>
	5. subquery in CASE:<br>
## 64. Correlated subquery:<br >
```
SELECT
	employee_id,
	first_name,
	last_name,
	salary
FROM employees e1
WHERE salary > (
	SELECT AVG(salary) FROM employees e2 WHERE e2.department = e1.department
);

-- 	show employees(with employee_id, first_name, last_name, salary)
--	whose salary are higher than their department average salary
```
1. In correlated subquery, every row in outer query rerun subquery,<br >
	so `e1` in outer query means __"current row"__ in outer query,<br >
	and `e1.department` means __"current row's department value"__<br >
2. Correlated subquery is slow because every row runs subquery 1 time<br >
## 65. SELECT 1<br>
```
SELECT 1
FROM orders;

--	show 1 on every rows
--	SELECT 1 append 1 for each exist row
```
## 65-2. EXISTS()<br >
EXISTS() use to check existence of certain record<br >
syntax: EXISTS(subquery)<br >
EXISTS() only accepts subquery, and EXISTS mostly use in WHERE<br >
EXISTS() return __"True"__ if subquery return 1+ row<br >
EXISTS() return __"False"__ if subquery return 0 row<br >
EXISTS() like AND, OR, NOT is logical operator, and it almost always used in WHERE<br >
```
SELECT
	c.customer_id
	c.customer_name
FROM
	customers c
WHERE EXISTS(
	SELECT 1
	FROM orders o
	WHERE o.customer_id = c.customer_id
);

--	show customer_id, customer_name if customer_id exists in orders table
--	SELECT 1 = spit out a 1 for every matches row
--	c and o are alias for avoiding ambiguous
```
## 66. MAX() aggregate function<br >
```
SELECT
	department,
	MAX(salary) AS max_salary
FROM employees
GROUP BY department;

--	show the largest salary in each department
--	MAX() return the largest value
--	MAX, MIN, SUM, AVG, COUNT are aggregate function
--	MAX, MIN, SUM, AVG, COUNT exclude NULL value when computing
```
## 67. Compare multiple column values in WHERE<br >
Q: Retrieve the employee details (employee_id, first_name, last_name, department, salary) of the employee with the highest salary in each department.<br >
1. employee_id, first_name, last_name, department, salary<br >
2. Retrieve employee with the highest salary in each department<br >
3. highest salary in each department<br >
```
SELECT
	employee_id,
	first_name,
	last_name,
	department,
	salary
FROM employees
WHERE (department, salary) IN (
	SELECT
		department,
		MAX(salary) AS salary
	FROM employees
	GROUP BY department
);

--	1. subquery: get the highest salary in each department
--	2. match (department, salary) pair to find the rest of employee info
--	3. Why this fail?
--		which makes this fail:
--		SELECT employee_info, department, MAX(salary) AS salary
--		FROM employees
--		GROUP BY department;

--		GROUP BY department doesn't have employee_info
--		This will collapse employee info in each department,
--		which disconnect employee info and (department, salary) info
--	4. when compare multiple columns, you have to paarenthesis them, e.g. (department, salary) IN...
--	5. "=" expect subquery return 1 row, to compare with multiple rows: use IN
```
- IN can always replace "="<br >
## 68. MIN aggregate function<br >
```
SELECT
	employee_id,
	first_name,
	last_name,
	department,
	salary
FROM employees
WHERE salary = (SELECT MIN(salary) FROM employees);

--	show employee detail (employee_id, first_name, last_name, department, salary)
--		who has the lowest salary
```
1. MIN() return lowest value<br >
2. Cannot do MIN(salary) at outer query because:<br >
	MIN, MAX, AVG, SUM etc. are aggregate functions, which return result of MANY rows<br >
	V.S.<br >
	employee_id, first_name etc. are SINGLE row<br >
Problem: put MIN(salary) at outer query cause MANY rows vs. SINGLE row conflict<br >
Solution:
	1. either GROUP BY other single row column in SELECT
	2. or put MANY ROWS result (aka. aggregate function MIN(salary)) in subquery in WHERE to compare single row from outer query<br >
## 69. HAVING + SUM aggregate function<br >
```
SELECT
	department,
	SUM(salary) AS total_salary
FROM employees
--	WHERE salary > 60000
GROUP BY department
HAVING SUM(salary) > 60000
ORDER BY total_salary DESC

--	show sum of salary as total_salary in each department in descending order
--	WHERE salary > 60000 filter individual employee salary
--	HAVING SUM(salary) > 60000 filter department total_salary after GROUP BY
```
## 70. EXTRACT()<br >
syntax: EXTRACT(keyword FROM column), e.g. EXTRACT(YEAR FROM order_date)<br >
common keyword:<br >
1. YEAR(2026)<br >
2. QUARTER(1~4)<br >
3. MONTH(1~12)<br >
4. WEEK(1~52)<br >
5. DAY(1~31)<br >
6. DOW(0: Sunday ~ 6: Saturday)<br >
7. HOUR/MINUTE/SECOND<br >
```
SELECT
	EXTRACT(YEAR FROM order_date) AS order_year,
	AVG(order_amount) AS AverageOrderAmount
FROM orders
GROUP BY order_year
ORDER BY order_year ASC;

--	FROM in EXTRACT is part of syntax and won't confuse SQL (SELECT...FROM)
--	execute order: GROUP BY -> SELECT:
--		so this is exception when engine go through GROUP BY
--		and see an alias, it will check SELECT and understand
--		which alias
```
## 71. CTE<br >
__CTE__ (common table expression) create temporary table during query<br >
syntax: WITH <table_name> AS ( \<query\> )
```
WITH yearlyData AS (
	SELECT
		EXTRACT(YEAR FROM order_date) AS order_year
		order_amount
	FROM orders
)
SELECT
	order_year,
	AVG(order_amount) AS AverageOrderAmount
FROM yearlyData
GROUP BY order_year
ORDER BY order_year ASC;

--	; means end of query => 1 query 1 ";"
--	so no ";" after CTE
```
## 72. SELECT all columns with additional column<br >
```
SELECT
	EXTRACT(YEAR FROM order_date)
	o.*
FROM orders o;

--	put * after additional column need to specify table name,
--	or SQL won't know which table for *
```
## 73. YEAR() scalar function<br >
syntax: YEAR( \<column_name\> ) return year of date format<br >
Common scalar function: YEAR(order_date), UPPER(name), ROUND(price, 2), LENGTH(email)
```
SELECT
	YEAR(order_date),
	orders.*
FROM orders;

--	scalar function doesn't have "single row vs. many rows" conflict,
--	so it doesn't need to have GROUP BY or be in subquery
```
## 74. HAVING + COUNT() aggregate function<br >
```
SELECT
	category,
	COUNT(*)
FROM products
GROUP BY category
HAVING COUNT(*) > 2;

--	show product category has more than 2 products
--	HAVING usually use with aggregate function
```
- HAVING use aggregate function to filter result after GROUP BY; WHERE filter result before GROUP BY<br >
## 75. DATE_FORMAT<br >
syntax: DATE_FORMAT(\<column\>, \<format\>) AS alias<br >
format:<br >
```
%Y: 2026, %y: 26
%M: February, %m: 02 (leading zero), %b: Feb
%D: 03st (with suffix), %d: 03 (leading zero), %e: 3 (no leading zero)
%H: 00~23, %h: 01~12
%i: 00~59
%s: 00~59
%p: AM, PM
%W: Monday~Sunday, %a: Mon~Sun
```
common use case: '%Y-%m', ISO format: '%Y-%m-%d'<br >
DATE_FORMAT change "Date" format to "String" format, so sorting will be different<br >
Solution: Always use '%Y-%m' or '%Y-%m-%d' which makes sorting the same<br >
```
SELECT
	month,
	SUM(amount) AS monthly_sales
FROM (
	SELECT
		DATE_FORMAT(order_date, '%Y-%m) AS month,
		order_amount AS amount
	FROM orders
)
GROUP BY month;

--	show monthly_sales in orders for each month
```
## 76. Window function OVER<br >
```
SELECT
	year,
	AVG(average_birth_year) OVER (ORDER BY year) AS cumulative_average_birth_year
FROM (
	SELECT
		EXTRACT(YEAR FROM customer_since) AS year,
		AVG(birth_year) AS average_birth_year
	FROM dim_customers_walmart
	GROUP BY EXTRACT(YEAR FROM customer_since)
) AS stage1;
```
1. What is OVER?<br >
Unlike aggregate function AVG, SUM, COUNT collapse rows into 1 row (Many to 1), OVER keeps Rows and add "running total"<br >
OVER:<br >
	1. turn aggregate function become Window function<br >
	2. keep rows (Many to Many)<br >
	3. calculate Cumulative result: every next row include result of previous row<br >
2. OVER syntax: AVG(\<column\>) OVER (ORDER BY \<column\>) AS \<alias\> <br >
3. What is Window function?
	1. Window means the calculation compute each row based on "a set of rows" (like a window), instead of whole table<br >
	So it doesn't like basic aggregation to collapse all rows to 1 result row<br >
	it creates Many results to Many rows<br >

4. What is running total? running means "Cumulative",<br >
As you move down current row, the window expand to include more rows (include current row) for calculation<br >
e.g. accumulated customer number every year<br >
## 77. SUM + OVER<br >
```
SELECT
	join_year,
	SUM(annual_new_customer) OVER (
		ORDER BY join_year
	) AS customer_so_far
FROM (
	SELECT
		EXTRACT(YEAR FROM customer_since) AS join_year
		COUNT(*) AS annual_new_customer
	FROM dim_customers_walmart
	GROUP BY EXTRACT(YEAR FROM customer_since)
) AS stage1;

--	inner first: get new customer for each year
--	outer later: calculate cumulative customer number every year
```
In `SUM(annual_new_customer) OVER (ORDER BY join_year) AS customer_so_far`:<br >
	1. The function: `SUM(annual_new_customer)`: how to calculate and what to calculate<br >
	2. The window: `OVER`: tell SQL to use window function<br >
	3. The rule: `ORDER BY join_year`: the cumulative function grow row-by-row based on time (join_year)<br >
## 78. GROUP BY, SELECT Alias conflict<br >
```
SELECT
	EXTRACT(YEAR FROM customer_since) AS year,
	--	year,
	COUNT(customer_id) AS annual_customer
FROM
	dim_customers_walmart
GROUP BY EXTRACT(YEAR FROM customer_since);
--	GROUP BY EXTRACT(YEAR FROM customer_since) AS year;
```
Question: GROUP BY execute eariler than SELECT, why SELECT cannot use Alias (AS) created by GROUP BY?<br >
Answer: the GROUP BY doesn't allow `AS`, only SELECT and FROM can use `AS`<br >
Solution: repeat same expression in GROUP BY and SELECT
## 79. subquery vs. CTE, + CASE<br >
subquery version:<br >
```
SELECT
	region,
	revenue_2022,
	revenue_2023,
	(revenue_2023 - revenue_2022) / revenue_2022 AS growth_rate
FROM (
	SELECT
		region,
		SUM(CASE WHEN year=2022 THEN revenue END) AS revenue_2022,
		SUM(CASE WHEN year=2023 THEN revenue END) AS revenue_2023
	FROM sales
	GROUP BY region
) AS stage1;

--	outer query do "calculation", inner query do "condition check"
```
CTE version:<br >
```
WITH stage1 AS (
	SELECT
		region,
		SUM(CASE WHEN year=2022 THEN revenue END) AS revenue_2022,
		SUM(CASE WHEN year=2023 THEN revneue END) AS revenue_2023
	FROM sales
	GROUPU BY region
)
SELECT
	region,
	revenue_2022,
	revenue_2023,
	(revenue_2023 - revenue_2022) / revenue_2022 AS growth_rate
FROM stage1;
```
1. subquery execute order: inner first, then outer<br >
2. SELECT __"create new column"__ from other columns:<br >
	1. ex: (revenue_2023 - revenue_2022) / revenue_2022 AS growth_rate<br >
	2. ex: SUM(CASE WHEN year=2022 THEN revenue END) AS revenue_2022<br >
3. CASE syntax:<br >
```
SELECT
	CASE
		WHEN <condition1> THEN <current column value1 return>
		WHEN <condition2> THEN <current column value2 return>
		ELSE <exception value return>
	END AS <new column value> 
```
4. Pivot pattern example:<br >
```
SELECT
	SUM(CASE WHEN year=2022 THEN revenue) AS revenue_2022
```
After group same region, different cities' revenue in 2022 are sumed up for "certain region 2022 revenue"<br >
This makes original "vertical long" table become "horizontal wide" table (called Pivot pattern)<br >
Practical usage: generate YoY (year over year) report<br >

5. Multiple CASE example:<br >
```
SELECT
	CASE
		WHEN salary >= 70000 THEN 'Executive'
		WHEN salary >= 60000 THEN 'Senior'
		WHEN salary >= 50000 THEN 'Mid-level'
		ELSE 'Entry-level'
	END AS career_tier

--	create career tier column by salary
```
6. Avoid dividing by 0:<br >
ex: `(revenue_2023 - revenue_2022) / revenue_2022`<br >
use NULLIF:<br >
syntax: NULLIF(\<expression1\>, \<expression2\>)<br >
NULLIF return NULL if expression1 = expression2<br >
NULLIF return expressioin1 if expression1 != expression2<br >
## 80. CASE with different column condition<br >
```
SELECT
	order_id,
	order_amount,
	status,
	CASE
		WHEN status = 'cancelled' THEN 'ignore',
		WHEN order_amount > 5000 THEN 'high value'
		WHEN order_date < '2025-01-01' THEN 'legacy'
		ELSE 'normal'
	END AS order_priority
FROM orders;

--	CASE 可以用不同 column 決定 data
```
## 81. GROUP BY + HAVING<br >
```
-- Retrieve the customer_name
-- customers who have placed orders with a total amount
-- greater than the average amount of all orders.

SELECT customer_name
FROM customers
WHERE customer_id IN (
	SELECT customer_id
	FROM orders
	GROUP BY customer_id
	HAVING SUM(order_amount) > (
		SELECT AVG(order_amount FROM orders)
	)
);
```
1. HAVING can directly use:<br >
	1. aggregate function, e.g. COUNT(*), SUM(order_amount)<br >
	2. subquery, e.g. (SELECT AVG(order_amount FROM orders))<br >
2. aggregate function can be SELECT or HAVING to work with GROUP BY<br >
	1. aggregate function in SELECT: __show__ the columns after GROUP BY<br >
	2. aggregate function in HAVING: __filter__ the rows after GROUP BY<br >
##   82. SHOW TABLES + LIKE<br >
```
SHOW TABLES;	--	show all table names on SQL
SHOW TABLES LIKE 'o%';	--	show all table names start with o on SQL
```
## 83. CASE TYPE<br >
CASE has 2 types:<br >
1. searched CASE (No specific expression)<br >
2. the simple CASE (with specific expression)<br >
- searched CASE:<br >
```
CASE
	WHEN order_amount > 1000 THEN 'high priority'
	ELSE 'average priority'
END AS order_status_2025
```
pros: can compare different columns, write column after each WHEN<br >
cons: have to write different column names multiple times<br >
<br >
- simple CASE:<br >
```
CASE order_status
	WHEN 'completed' THEN 'drop'
	ELSE 'keep'
END AS order_status_2025
```
pros: only compare 1 column value, write column after CASE only once<br >
cons: can only use 1 column, and can only do check if value is the same, no >, < comparison<br >
## 84. CASE create column<br >
```
SELECT
	product_name,
	CASE
		WHEN unit_price > 500 THEN 'high_profit'
		ELSE 'average_profit'
	END AS profitibility
FROM products;
```
CASE always happens in SELECT clause (but not immediately after SELECT)<br >
CASE always creates a new column, e.g. `END AS profitibility`<br >
## 85. Always add ELSE in CASE<br >
```
SELECT
	*,
	CASE
		WHEN DATE_FORMAT(order_date, '%Y-%m') < '2023-05' THEN 'outdated
		WHEN order_status = 'Completed' THEN 'in drop quere'
		ELSE 'processing'
	END AS order_status_2025
FROM orders;

--	always add ELSE in CASE, otherwise, rows not matching WHEN will be set NULL (empty)
```
## 86. LEFT JOIN & JOIN<br >
```
SELECT
	c.customer_id,
	o.order_id,
	o.order_date
FROM customers AS c
LEFT JOIN orders AS o
	ON c.customer_id = o.customer_id;
```
1. JOIN syntax:<br >
```
FROM <table1> AS <alias1>
[JOIN TYPE] <table2> AS <alias2>
	ON <condition>
```
2. SQL clause order: SELECT, FROM, "<ins>JOIN</ins>", "<ins>ON</ins>", WHERE, GROUP BY, HAVING, ORDER BY, LIMIT, OFFSET<br >
3. SQL execute order: FROM, "<ins>JOIN</ins>", "<ins>ON</ins>", WHER, GROUP BY, HAVING, "<ins>SELECT</ins>", ORDER BY, LIMIT, OFFSET<br >
4. How "<ins>LEFT JOIN</ins>" expand "<ins>rows</ins>"?<br >
	- LEFT JOIN expand rows when table1 \<-\> table2 is __one-to-many__<br >
	- e.g. 1 customer_id in customers table matches 2 customer_id's orders in orders table<br >
5. How "<ins>All JOIN</ins>" expand "<ins>columns</ins>"?<br >
	- All JOIN add table2 columns to table1 columns, and do SELECT in the end.<br >
	- e.g. table customers has 3 columns, and table orders has 4 columns<br >
	The JOIN outputs table with 7 columns (2 customer_id columns: customers.customer_id, orders.customer_id)<br >
## 87. JOIN multiple tables<br >
```
SELECT
	c.customer_id,
	c.customer_name,
	o.order_id,
	oc.status
FROM customers AS c
LEFT JOIN orders AS o
	ON c.customer_id = o.customer_id
LEFT JOIN orders_cat AS oc
	ON o.order_id = oc.order_id;
```
1. There can be many JOINs in a query, the execute order is:<br >
	FROM table1 JOIN table2, then result table JOIN table3<br >
2. Indentation format: JOIN align with FROM, then ON indented<br >
3. multiple JOINs can also use CTEs<br >
	e.g. <ins>A</ins> LEFT JOIN <ins>B</ins> LEFT JOIN <ins>C</ins> LEFT JOIN <ins>D</ins><br >
	=> CTE1 = <ins>A</ins> LEFT JOIN <ins>B</ins>, CTE2 = CTE1 LEFT JOIN <ins>C</ins>, CTE3 = CTE2 LEFT JOIN <ins>D</ins><br >
## 88. LEFT JOIN vs. INNER JOIN vs. RIGHT JOIN<br >
```
SELECT
	c.customer_id,
	c.customer_name,
	COUNT(o.order_id) AS order_count,
	SUM(o.order_amount) AS total_amount
FROM orders o
LEFT JOIN customers c
	ON o.customer_id = c.customer_id
GROUP BY c.customer_id, c.customer_name;
```
1. how to know when to use LEFT JOIN, or INNER JOIN?<br >
	Ask: for customers don't have orders, should I output them?<br >
	Output them: LEFT JOIN, not output them: INNER JOIN<br >
2. always use LEFT JOIN, because human read code from left to right<br >
	If need RIGHT JOIN, just swap order of tables and use LEFT JOIN<br >
3. Difference between <ins>first COUNT(), SUM() in subquery, then LEFT JOIN</ins> vs. <ins>first LEFT JOIN, then COUNT(), SUM()</ins>:<br >
	1. COUNT() return <ins>0</ins> if NULL<br >
	2. SUM(), AVG(), MIN(), MAX() return <ins>NULL</ins> if NULL<br >
	3. LEFT JOIN return <ins>NULL</ins> if mismatch<br >
	- <ins>first COUNT(), SUM() in subquery, then LEFT JOIN</ins> mismatch:<br >
		First 0, NULL, then both NULL => output NULL, NULL<br >
	- <ins>first LEFT JOIN, then COUNT(), SUM()</ins> mismatch:<br >
		First both NULL, then 0, NULL => output 0, NULL<br >
4. always add COALESCE(SUM(order_amount), 0) to make sure be 0<br >
	- COALESCE syntax: COALESCE(\<expression1\>, \<expression2\>)<br >
	- return expression1 if expression1 != NULL<br >
	- return expression2 if expression1 = NULL<br >
	- COALESCE is scalar function, not aggregate function<br >
5. NULLIF() turn 0 to NULL, COALESCE() turn NULL to 0<br >
6. GROUP BY group multiple columns separated by ","<br >
## 89. INNER JOIN + USING<br >
```
SELECT
	c.customer_id,
	c.customer_name,
	o.order_id,
	o.order_date,
	o.order_amount
FROM customers c
INNER JOIN orders o
	USING(customer_id);

--	ON <condition>, no parenthesis
--	USING(<column>), has parenthesis
```
1. INNER JOIN can create rows as n x m (Cartesian Porduct) (INNER JOIN not always create min(n, m) rows)<br >
	ex: INNER JOIN 1-to-1 => create 0 ~ min(n, m) rows, many-to-many => create 0 ~ n x m rows<br >
2. USING syntax: USING(\<column_name\>)<br >
	1. USING is a shorthand of ON, you can only use it when the column names you are joining to are "<ins>Exactly Same</ins>" in both tables.<br >
	2. USING:<br >
		1. simplify JOIN logic (only look for the same column name).<br >
		2. merge 2 columns into 1 column<br >
3. Why USING won't confuse SQL which table's column it refer?<br >
	<ins>USING</ins> produces only 1 combined column as output, while <ins>ON</ins> keeps 2 separate columns as output,<br >
	so USING WON'T confuse SQL which column it refers.<br >
4. Shorthand:<br >
	1. JOIN: shorthand of INNER JOIN<br >
	2. LEFT JOIN: shorthand of LEFT OUTER JOIN<br >
	3. RIGHT JOIN: shorthand of RIGHT OUTER JOIN<br >
	4. FULL JOIN: shorthand of FULL OUTER JOIN<br >
	5. CROSS JOIN: Cartesian Product<br >
## 90. JOIN ON multiple columns<br >
```
SELECT
	s.store_id,
	s.product_id,
	i.stock_level
FROM sales s
INNER JOIN inventory i
	ON s.store_id = i.store_id
	AND s.product_id = i.product_id;
```
We use <ins>AND</ins> to JOIN multiple columns<br >
## 91. JOIN USING multiple columns<br >
```
SELECT *
FROM sales s
INNTER JOIN inventory i
	USING(store_id, product_id);
```
<ins>USING</ins> use "," to JOIN multiple columns<br >
## 92. FULL JOIN<br >
1. FULL JOIN = OR in logic (left + right - inner join (left, right))<br >
2. What does FULL JOIN do?<br >
	1. FULL JOIN adds all rows from both tables.<br >
	2. If there is a match on column(s), they are in the same row.<br >
	3. If no matches, they become 2 rows with NULLs on each side.<br > 
3. To prevent many to many explosion, we have to use <ins>GROUP BY</ins> before FULL JOIN<br >
4. How many rows created:<br >
	- ex: usecase:<br >
		1. left table = n + i, right table = j + m<br >
		2. n, m are many-to-many, i, j are 1-to-1:<br >
	1. For INNER JOIN, rows: 0 ~ n*m + min(i, j)<br >
	2. For LEFT JOIN, rows: n + i ~ n*m + i<br >
	3. For FULL JOIN, rows: max(n + i, m + j) ~ n*m + i + j<br >
	4. When FULL JOIN without match: rows = n + m + i + j<br >
	5. When will FULL JOIN have min rows?<br >
		FULL JOIN will have min rows when only having 1-to-1 match<br >
		`min rows = max(n+i, m+j)` where n and m have to <= 1 to make both n+i and m+j are 1-to-1<br >
## 93. FULL JOIN in MySQL<br >
```
SELECT
	c.customer_id,
	c.customer_name,
	o.order_id,
	o.order_date
FROM customers c
LEFT JOIN orders o
	USING(customer_id)
UNION
SELECT
	c.customer_id,
	c.customer_name,
	o.order_id,
	o.order_date
FROM customers c
RIGHT JOIN orders o
	USING(customer_id);
```
1. MySQL doesn't have FULL JOIN, so has to use LEFT JOIN, UNION, and RIGHT JOIN to create FULL JOIN<br >
	LEFT JOIN and RIGHT JOIN will duplicate at matched part, and UNION will perform DISTINCT, so the duplicated part only appear once<br >
2. UNION: UNION combine two tables "<ins>Vertically</ins>" and do DISTINCT to keep duplicated rows appear only once<br >
3. JOINs combine tables "<ins>Horizontally</ins>" (expand <ins>columns</ins>),<br >
	UNION combine tables "<ins>Vertically</ins>" (expand <ins>rows</ins>)<br >
4. UNION syntax:<br >
```
SELECT column1 FROM table1
UNION
SELECT column2 FROM table2;
```
5. UNION rules:<br >
	1. two tables must have same number of Columns.<br >
	2. Columns must have "Compatible Data Types in the Same order"<br >
		- common compatible data type:<br >
			1. numeric: INT, BIGINT, DECIMAL, FLOAT<br >
			2. string: CHAR, VARCHAR, TEXT<br >
			3. temporal: DATE, DATETIME, TIMESTAMP<br >
6. UNION vs. UNION ALL: <br >
	1. UNION do DISTINCT: keep duplicated rows appear only once,<br >
	2. UNION ALL stack 2 tables: duplicated rows show twice<br >
## 93-1. UNION<br >
```
SELECT
	customer_id,
	order_date AS transaction_date,
	order_amount AS transaction_amount
FROM orders 
UNION
SELECT
	customer_id,
	invoice_date AS transaction_date,
	total_amount AS transaction_amount
FROM invoices;
```
1. If want to show duplicated customer_id without DISTINCT: UNION ALL<br >
2. Can use more than 1 UNION to combine > 2 SELECTs<br >
3. When column names are different in each SELECT, UNION takes first SELECT column names as column names<br >
4. UNION's keyword: "<ins>consolidate</ins>", e.g. create a consolidated list of transactions<br >
## 94. JOIN + BETWEEN<br >
```
SELECT
	c.customer_id,
	c.customer_name,
	o.order_id,
	o.order_date,
	o.order_amount
FROM customers c
JOIN orders o
	USING(customer_id)
WHERE o.order_date BETWEEN '2023-01-01' AND '2023-06-30';

-- WHERE filter date range after JOIN
```
## 95. Filter in JOIN vs. Filter in WHERE<br >
```
SELECT
	c.customer_name,
	o.order_date
FROM customers c
JOIN orders o
	ON c.customer_id = o.customer_id
	AND o.order_amount > 1000;
```
vs.<br >
```
SELECT
	c.customer_name,
	o.order_date
FROM customers c
JOIN orders o
	USING(customer_id)
WHERE o.order_amount > 1000;
```
1. `USING` only accpet column name, so cannot do AND or other things after it.<br >
2. If it's INNER JOIN, no difference between them.<br >
3. If it's LEFT JOIN,
	1. 1st one will keep all customer rows and let orders' columns NULL if order_amount <= 1000.<br >
	2. 2nd one rows with order_amount <= 1000.<br >
4. The speed of two are the same. The 2nd one use more memory because the joined table is larger.<br >
## 96. ON with filter<br >
```
SELECT
	c.customer_id,
	c.customer_name,
	o.order_id,
	o.order_date,
	o.order_amount
FROM customers c
JOIN orders o
	ON c.customer_id = o.customer_id
	AND o.order_amount >= 500;

--	Retrieve a list of customer_id and customer_name
--	and their corresponding order_id, order_date, order_amount
--	but only for orders with a total amount at least 500
```
`ON` is a logical clause and can do filtering using `AND`, `OR`.<br >
## 97. DISTINCT in COUNT<br >
```
SELECT COUNT(DISTINCT department) AS total_department
FROM employees;
```
1. DISTINCT in COUNT() is a common use case to find number of unique columns.<br >
2. Use DISTINCT in aggregate function in SELECT. If DISTINCT not in aggregate function, it can be replaced by GROUP BY.<br >
3. DISTINCT always after SELECT if it's not in aggregate function.<br >
4. DISTINCT immediately after SELECT will see all-column-in-SELECCT as a combination and filter them to output only unique combination.<br >
## 98. USING same column name<br >
```
SELECT
	c.customer_id,
	c.customer_name,
	c.address,
	o.order_id,
	o.order_date,
	o.order_amount,
	o.shipping_address
FROM customers c
JOIN orders o
	ON c.customer_id = o.customer_id
	AND c.address = o.shipping_address;
```
If column names are exactly the same, can use USING, e.g.<br >
```
ON c.customer_id = o.customer_id
AND c.address = o.address
=> USING(customer_id, address)
```
## 99. Self JOIN<br >
```
SELECT
	e.employee_id,
	e.first_name,
	m.first_name AS manager_name,
	ms.first_name AS senior_manager
FROM employees e
JOIN employees m
	ON e.manager_id = m.employee_id
JOIN employees ms
	ON m.manager_id = ms.employee_id;
```
Self JOIN to show hierarchy of employees (show employees' managers and senior managers).<br >
## 100. CONCAT()<br >
```
SELECT
	e1.employee_id AS employee1_id,
	CONCAT(e1.first_name, ' ', e1.last_name) AS employee1_name,
	e2.employee_id AS employee2_id,
	CONCAT(e2.first_name, ' ', e2.last_name) AS employee2_name,
	e1.manager_id
FROM employees e1
JOIN employees e2
	ON e1.manager_id = e2.manager_id
	AND e1.employee_id < e2.employee_id;

--	show employee pair with same manager
```
1. syntax: CONCAT(\<'string1'\>, \<column1\>, \<function1('string1')\>) AS new_column_name<br >
2. usage: CONCAT(column1, ' ', column2) AS new_column_name<br >
3. CONCAT combine <ins>2 column values</ins> into one (horizontally), while UNION combine <ins>2 tables</ins> into one (vertically).<br >
4. CONCAT can combine 1. pure string, 2. string in column, 3. output string from function.<br >
5. To avoid repeating 1. identity case (employee_id=1, employee_id=1), 2. symmetric pair (employee_id=1, employee_id=2), (employee_id=2, employee_id=1):<br >
	use <ins>smaller than</ins> comparison `AND e1.employee_id < e2.employee_id`<br >
6. If manager_id is NULL, JOIN see NULL as UNKNOWN, and see UNKNOWN as False, so it won't show up.<br >
## 101. CURDATE() vs. CURTIME() vs. NOW()<br >
CURDATE(): Year-Month-Date<br >
```
--	in MySQL:
SELECT CURDATE() AS current_date;
--	in PostgreSQL:
SELECT CURRENT_DATE AS current_date;
--	in SQL server:
SELECT CAST(GETDATE() AS DATE) AS current_date;
```
CURTIME():	Hour:Minute:Second<br >
```
--	in MySQL:
SELECT CURTIME() AS current_time;
--	in PostgreSQL:
SELECT CURRENT_TIME AS current_time;
--	in SQL server:
SELECT CAST(GETDATE() AS TIME) AS current_time;
```
Date+Time:	Year-Month-Date Hour:Minute:Second<br >
```
--	in MySQL:
SELECT NOW() AS current_datetime;
--	in PostgreSQL:
SELECT CURRENT_TIMESTAMP AS current_datetime;
--	in SQL server:
SELECT GETDATE() AS current_datetime;
```
## 102. DEFAULT NOW()<br >
```
ALTER TABLES employees
ADD COLUMN create_at TIMESTAMP DEFAULT NOW();

--	add create_at column to log when when was a column born
--	column data type is TIMESTAMP
--	column default value is NOW()
```
## 103. INTERVAL<br >
```
SELECT *
FROM orders
WHERE order_date >= CURDATE() - INTERVAL 30 HOUR - INTERVAL 30 MINUTE;
```
1. syntax: INTERVAL \<quantity\> \<unit\><br >
	\<quantity\>: + or - number, e.g. 30, -7<br >
	\<unit\>: YEAR, MONTH, DAY, WEEK, HOUR, MINUTE<<br >
2. INTERVAL can be in SELECT, WHERE, ON<br >
3. Day + 1 day:<br >
	MySQL: `create_at + INTERVAL 1 DAY;`<br >
	PostgreSQL: `create_at + INTERVAL '1 DAY'`<br >
	SQL server: `DATEADD(DAY, 1, create_at)`<br >
## 104. INSERT INTO + SELECT<br >
```
INSERT INTO factsales (
	order_id,
	customer_id,
	gross_amount,
	tax_amount,
	processed_at
)
SELECT
	order_id,
	customer_id,
	total_price,
	total_price * 0.05,
	NOW(),
FROM staging_orders
WHERE status = 'Completed'
	AND order_date = CURDATE();
```
Use `INSERT INTO` to add rows from <ins>table: staging_orders</ins> to <ins>table: factsales</ins> by `SELECT`<br > 
## 105. DATEDIFF()<br >
```
SELECT
	order_id,
	order_date,
	shipped_date,
	DATEDIFF(shipped_date, order_date) AS days_to_ship
FROM orders
WHERE DATEDIFF(shipped_date, order_date) > 3;

--	find out how many days take to ship after placing orders
--	find shipping takes over 3 days for slow shipment
```
- syntax: `DATEDIFF(<end_date>, <start_date>)`, e.g. `DATEDIFF('2026-03-08', '2026-03-01')`<br >
	=> end_date - start_date = '2026-03-08' - '2026-03-01' = 7<br >
## 106. TIMEDIFF()<br >
```
SELECT
	job_name,
	start_time,
	end_time,
	TIMEDIFF(end_time, start_time) AS duration
FROM job_logs
WHERE job_name = 'Daily_Sales_Sync';

--	find out what's the latency of Daily_Sales_Sync
```
- syntax: `TIMEDIFF(<end_time>, <start_time>)`, e.g. `TIMEDIFF('14:30:05', '14:00:00')`<br >
	=> end_time - start_time = '14:30:05' - '14:00:00' = '00:30:05'<br >
## 107. TIMESTAMPDIFF()<br >
```
SELECT
	u.user_id,
	TIMESTAMPDIFF(u.signup_time, a.first_action_time) AS hours_to_action
FROM users u
JOIN user_actions a
	USING(user_id)
WHERE TIMESTAMPDIFF(HOUR, u.signup_time, a.first_action_time) <= 24;

--	Product Manager wants to find if user performed an action after signning up in 24 hours
```
1. syntax `TIMESTAMPDIFF(unit, start, end)`, e.g. `TIMESTAMPDIFF(HOUR, u.signup_time, a.first_action_time)`<br >
	=> end - start = a.first_action_time - u.signup_time<br >
2. TIMESTAMPDIFF unit: FRAC_SECOND, SECOND, MINUTE, HOUR, DAY, WEEK, MONTH, QUARTER, YEAR<br >
	FRAC_SECOND is microsecond = $$10^{-6}$$ seconds = one millionth<br >
3. TIMESTAMPDIFF do Floor 無條件捨去法<br >
4. Return type:
	1. DATEDIFF return INTEGER<br >
	2. TIMEDIFF return STRING<br >
	3. TIMESTAMPDIFF return INTEGER<br >
## 108. INTERVAL<br >
```
SELECT
	o.order_id,
	o.order_status,
	o.order_amount,
	o.order_date,
	o.order_date + INTERVAL 7 DAY AS expected_delivery_date,
	o.order_date - INTERVAL 3 DAY AS last_cancellation_date,
	c.customer_id,
	c.customer_name
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id;
```
- syntax: `<column> +/- INTERVAL <number> <unit>`, e.g. o.order_date + INTERVAL 7 DAY AS expected_delivery_date<br >
- make sure `<column>` type match `<unit>` type<br > 
## 109. DATE_ADD(), DATE_SUB()<br >
1. `DATE_ADD()` syntax: `DATE_ADD(<column>, INTERVAL <number> <unit>)`, e.g. `DATE_ADD(o.order_date, INTERVAL 7 DAY) AS expected_delivery_date`<br >
2. `DATE_SUB()` syntax: `DATE_SUB(<column>, INTERVAL <number> <unit>)`, e.g. `DATE_SUB(o.order_date, INTERVAL 3 DAY) AS last_cancellation_date`<br >
3. DATE_ADD(), DATE_SUB() can be replaced by INTERVAL<br >
	-`DATE_ADD(NOW(), INTERVAL 20 DAY) AS deadline` => `NOW() + INTERVAL 20 DAY AS deadline`<br >
	-`DATE_SUB(NOW(), INTERVAL 3 DAY) AS last_cancel_date` => `NOW() - INTERVAL 3 DAY AS last_cancel_date`<br >
## 110. DATE_FORMAT<br >
```
SELECT
	c.customer_name,
	DATE_FORMAT(o.order_date, '%d/%m/%Y') AS order_date,
	DATE_FORMAT(o.order_date, '%W') AS weekday
FROM customers c
JOIN orders o
	USING(customer_id);
```
1. DATE_FORMAT for formatting date<br >
2. DATE_FORMAT: <br >
	1. Year: 		%Y: 2026, 		%y: 26<br >
	2. Month: 		%M: February, 	%m: 02, 	%b: Feb<br >
	3. Day: 		%D: 3rd, 		%d: 03, 	%e: 3<br >
	4. Weekday: 	%W: Sunday, 	%a: Sun<br >
	5. Hour(24/12): %H: 13, 		%h: 1<br >
	6. Minute: 		%i: 58<br >
	7. Second: 		%s: 58<br >
	8. AM/PM: 		%p: AM<br >
## 111. Data type<br >
### Common numeric data type:<br >
1. `INT`: integer<br >
2. `DECIMAL(p, s)`: p (precision) = total digits, s (scale) = digits after decimal point<br >
	1. ex: DECIMAL(5, 2): total digit = 5, digit before decimal = 3, digit after decimal = 2<br >
	2. Highest value: 999.99, lowest value: -999.99<br >
	3. Why call <ins>precision</ins> and <ins>scale</ins>:<br >
		- precision (精準) means how detail is it, e.g. 500 m = lower precision, 500.21 m = higher precision<br >
			In SQL, it's how many "Siginificant digit" in DECIMAL = total digit<br >
		- scale (規模) means zoom in level, e.g.<br >
			1. scale 0 = zoom in 0 digit: integer<br >
			2. scale 2 = zoom in 2 digit: 0.01<br >
			3. scale 9 = zoom in 9 digit: 0.000000001<br >
3. `FLOAT`: float point number<br >
	Difference between DECIMAL and FLOAT:<br >
	- DECIMAL is exact number: 0.01 store as 0.01 no change<br >
		Use case: Money, Accounting<br >
	- FLOAT is approximate number: 0.01 can store as 0.010002<br >
		Use case: Scientific data<br >
### Character string data type:<br >
1. `CHAR(n)`: fixed length string,<br >
	e.g. CHAR(10) store 'cat' still use 10 char spaces<br >
2. `VARCHAR(n)`: variable length string, <br >
	e.g. VARCHAR(10) store 'cat' only use 4 char spaces (3 for cat, 1 for string length)<br >
### Date and time data type:<br >
1. `DATE`: store date YYYY-MM-DD<br >
2. `TIME`: store time HH:MM:SS<br >
3. `TIMESTAMP`: store date and time YYYY-MM-DD HH:MM:SS<br >
### Boolean data type:<br >
`BOOLEAN`: store True or False<br >
### Binary data type:<br >
`BLOB` (binary large object): store unstructure data that doesn't fit in table:<br >
	e.g. image: JPG, PNG, GIF, audio: MP3, document: PDF, compiled data: python, java binary code<br >
## 112. CONCAT()<br >
```
SELECT
	customer_name,
	customer_id,
	CONCAT(address, ', ', city) AS full_address
FROM customers; 
```
1. `CONCAT()` connects multiple <ins>columns (horizontal)</ins>, combine pure string, output string from function<br >
2. `UNION()` conncects multiple <ins>rows (vertical)</ins><br >
3. MySQL, PostgreSQL ignore NULL in `CONCAT()`, e.g. CONCAT('hi', NULL) => 'hi'<br >
## 113. CONCAT_WS()<br >
```
SELECT
	customer_id,
	CONCAT_WS(', ', customer_name, address, city)
FROM customers;
```
1. `CONCAT_WS()` = CONCAT with separator<br >
2. syntax: `CONCAT_WS(<separator>, <column1>, <column2>,...)`<br >
	-e.g. `CONCAT_WS(', ', address, city)` = 'address, city'<br >
3. CONCAT_WS will skip NULL and won't add additional separator<br >
## 114. CAST<br >
```
SELECT CONCAT('$', CAST(SUM(order_amount) AS STRING)) AS total_amount_string
FROM orders
WHERE order_status = 'Completed';
```
1. syntax: `CAST(<column> AS <DATA_TYPE>)`: <br >
	- `CAST(<column> AS CHAR)`<br >
	- `CAST(<column> AS SIGNED/UNSIGNED)`<br >
	- `CAST(<column> AS DATETIME/TIMESTAMP)`<br >
	- `CASE(<column> AS BOOLEAN)`<br >
2. `CAST(<column> AS DATETIME/TIMESTAMP)` is faster than `DATE_FORMAT(<column>, '%Y-%m-%d')`<br >
3. CAST DATETIME/TIMESTAMP convert to <ins>Date</ins> type, DATE_FORMAT convert to <ins>String</ins> type<br >
4. `CAST(<column> AS DATETIME)` output 'YYYY-MM-DD HH:MM:SS'<br >
	V.S. `EXTRACT(YEAR FROM order_date)` output 'YYYY', other ex: MONTH, DAY, HOUR, MINUTE, SECOND<br >
5. MySQL doesn't allow `CAST(<column> AS VARCHAR)` and `CAST(<column> AS STRING)`<br >
6. `TRY_CAST()` is function for BigQuery, Snowflake, Azure SQL Database, SQL Server, not for MySQL<br >
## 115. DESCRIBE<br >
How to check data type of columns: `DESCRIBE <table_name>`<br >
## 116. LENGTH, LEFT, RIGHT<br >
```
SELECT
	CASE
		WHEN LENGTH(COALESCE(user_name, '')) > 4 THEN CONCAT(LEFT(COALESCE(user_name, ''), 4), '...'),
		ELSE COALESCE(user_name, '')
		END AS adjusted_username
FROM users;
```
1. syntax: `LENGTH(<column>)` output string length, e.g.`LENGTH('test')`, `LENGTH(c.customer_name)`<br >
2. use CONCAT() to join/connect strings<br >
3. syntax: `LEFT(<string>, number)`: return number of letters in string from left<br >
4. syntax: `RIGHT(<string>, number)`: return number of letters in string from right<br >
5. COALSECE in CONCAT to make sure if cancel the CASE and direct CONCAT user_name won't correpted<br >
6. LEGNTH can be in SELECT, WHERE, JOIN ON, GROUP BY, ORDER BY<br >
## 117. calculate remainder<br >
```
SELECT
	c.customer_id,
	c.customer_name,
	o.order_id,
	o.order_date,
	o.order_amount
FROM customers c
JOIN orders o
	ON c.customer_id = o.customer_id
	AND LENGTH(customer_name) % 2 <> 0;

--	to check LEGNTH is odd, use % (same as python), LENGTH(customer_name) % 2 <> 0
```
## 118. SUBSTRING() & LOCATE()<br >
```
SELECT
	customer_name,
	SUBSTRING(
		customer_name,
		1,
		LOCATE(' ', customer_name)-1
	) AS first_name,
	LEFT(
		customer_name,
		LOCATE(' ', customer_name)-1
	) AS first_name2,
	LOCATE(' ', customer_name) AS space_index
FROM customers;

--	get first_name from customer_name by locate the mid space index
```
1. SUBSTRING syntax: `SUBSTRING(<column>, <start_index>, <length>)`: return substring start at <ins>start_index</ins> with <ins>length</ins>.<br >
2. LEFT syntax: `LEFT(<string>, <number>)`: return number of letters in string from left.<br >
3. RIGHT syntax: `RIGHT(<string>, <number>)`: return number of letters in string from right.<br >
4. if `<number>` in LEFT or RIGHT larger than string, they will return whole string.<br >
5. LOCATE syntax: `LOCATE(<target_string>, <whole_string>, (optional)<start_index>)`: return <ins>index</ins> of target_string in whole_string.<br >
6. Index in SQL start at <ins>1</ins>.<br >
7. LOCATE return <ins>0</ins> when not found (use CASE or IF to handle not found situation).<br >
8. SUBSTRING, LEFT, RIGHT used on get first name, last name, remove unnecessary char, separate word by delimiter.<br >
## 119. SUBSTRING & RIGHT to remove prefix<br >
```
SELECT
	SUBSTRING(
		product_name,
		4,
		LENGTH(product_name)
	) AS cleaned_name,
	RIGHT(
		product_name,
		LENGTH(produuct_name)-3
	)
FROM products;

--	a way to remove first several letters/prefix, e.g. remove first 3 letters
```
## 120. EXTRACT MONTH<br >
```
SELECT
	EXTRACT(MONTH FROM order_date) AS month1,
	SUBSTRING(
		order_date,
		6,
		2
	) AS month2,
	DATE_FORMAT(order_date, '%m') AS month3
FROM orders;

--	3 ways to extract MONTH from date
```
## 121. SUBSTRING find email domain<br >
```
SELECT
	customer_name,
	SUBSTRING(
		email,
		LOCATE('@', email)+1
	) AS email_domain
FROM customers;

--	use SUBSTRING to find email domain
```
