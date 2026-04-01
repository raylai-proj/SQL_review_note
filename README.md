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

--- CREATE TABLE must include:
---		table name (Students)
---		column name (ID, Name, Major),
---		data type (INT, VARCHAR, DATE)
---		#	VARCHAR(50): 50 = Max length
---		Option: PRIMARY KEY
```
## 19. ALTER TABLE<br >
example: <br >
```
ALTER TABLE Students
	ADD Email VARCHAR(100);

---	Add new column "Email": need:
	1. ALTER TABLE,
	2. table name: Students,
	3. action: ADD, REMOVE,
	4. new col name + data type
```
## 20. DROP TABLE<br >
example: <br >
```
DROP TABLE Students;

---	delete table and delete all data in that table
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

--- "INSERT INTO" match type, primary key no duplicate, ; in the end
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

--- WHERE is filter to select matched rows
--- Always include WHERE to prevent select all rows
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

--- if ID = 1 row has foreign key columns,
--- we have to first delete referenced row, which has primary key, in sub-table 
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

--- columns will be listed in order which we specify,
---	e.g. SELECT 2, 1, 3 FROM table; => 2,1,3
```
## 25. AS Alias 別名
example: <br >
```
SELECT
	col1 AS alias1,
	col2 AS alias2
FROM table1 AS alias3;

---	可以設定col跟table的Alias aka. 別名
```
## 26. WHERE
WHERE filter rows: use cols condition to find satisfied rows<br >
```
SELECT *
FROM employees
WHERE salary > 50000;

--- SELECT <col1>, <col2>, <col3>
---	FROM <table_name>
---	WHERE <condition>;
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

--- don't have to show the filter condition column unit_price
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

--- filter departments has to be 'Sales' in WHERE clause
--- WHERE can use <, >, <=, >=, <>, =
---	<>: column not this value: for filtering Value, e.g. salary <> 50000
---	NOT: Logical operator for condition: NOT <condition>
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

--- NOT <column_name> = <value>
```
## 30.
```
SELECT *
FROM employees
WHERE department NOT IN ('Sales', 'HR', 'IT') AND salary NOT BETWEEN 30000 AND 50000;
	
---	IN must follow by (), the () here is a Set, usually () is for parse order
---	1 or 2 condition: AND at the same line,
---	3 or more conditions: break AND into multiple lines
---		Leading operator style: Put AND, OR at the start of new line
---		SQL parser has operator precedence, so it won't confuse at two ANDs
```
## 31. Escape from ' by '<br >
```
SELECT *
FROM employees
WHERE name = 'O''Relly';

---	when value has ', add ' for escape, so 'Relly => ''Relly, and single quote for value 'O''Relly'
```
