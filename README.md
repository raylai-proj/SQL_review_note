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
