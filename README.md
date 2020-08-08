# Learning postgresSQL
Tinkering with postgresSQL.


PostgresSQL is an open-source relational database maangement system. 

### Starting postgresSQL
To start/stop/control the server I'll be using **pg_ctl**. 
```
pg_ctl -D /usr/local/var/postgres start
```
I'm using a mac, so the directory **/usr/local/var/postgres** might vary from your machine.


### Using the shell
psql is the PostgreSQL interactive terminal. To run the interactive shell, type
```
psql postgres 
```
It will promt a new cli. The **postgres** is a default database that came with the installation.

To check if everything is alright, we can check the version type
```
select version();
```

and the output should be similar to 

```sql
PostgreSQL 12.3 on x86_64-apple-darwin19.5.0, compiled by Apple clang version 11.0.3 (clang-1103.0.32.62), 64-bit
(1 row)
```

### Creating a database

```
create DATABASE $dbName;

# example 
create DATABASE test_db;
```

the output should be
```
CREATE DATABASE
```

### Listing all databases
To list all database in the interactive shell, type ```\l```, it is not a SQL command so no need to use semi colons.

![Listing all databases](https://i.ibb.co/wrwdkDP/image.png)

### Switching databases
To swithc to database, type ```\c $dbname``` to start using the database. For example
```\c test_db```.
The output should be similar to 
```
You are now connected to database "test_db" as user "prophecy".
```
Again, this is not a SQL command, so no need to add semi-colons.

So now we are in our practice database, let's create some tables. We'll start with ```colours``` table.

### Deleting a database
```sql
DROP DATABASE $database_name;
```

### Connecting to database
The command to connect to a database is 
```bash
psql -h $host_ip_or_domain -p $port -U $username -W $password $dbname
```
So in my case, (password is ommited as my default password blank/I don't have a default password.)
```bash
psql -h localhost -p 5432 -U username dbname
```

### Creating a table
To create a table, our syntax is 
```sql
CREATE TABLE $tableName (
	$column_1 $data_type $optional_meta_information, 
	$column_2 $data_type $optional_meta_infromation,
	$column_N $data_type $optional_meta_information,
	$table_constraints
);
```
Lets start simple, 
So our command will be 
```sql
CREATE TABLE colours (
	id int PRIMARY KEY NOT NULL, 
	name varchar(20) NOT NULL, 
	hex varchar(7)
);
```
To know if it worked, we can check tables,  by typeing ```\dt``` while we are in the database.
![List of all tables](https://i.ibb.co/L5Gwg2t/image.png)

### Inserting data
As of now, we have our table and we can store some data in it. To store data, we use a **insert** query. The insert query is structured as follows 
```sql
INSERT INTO $table_name ($column_1, $column_2, ... $column_N) VALUES ($value_of_column_1, $value_of_column_2, ... $value_of_column_N);
```
This will insert a single row. If we want to insert multiple rows at once, we can append to the values.

```sql 
INSERT INTO $table_name ($column_1, $column_2, ... $column_N) VALUES ($row_1_column_1, $row_1_column_2, ... $row_1_column_N),($row_2_column_1, $row_2_column_2, ... $row_2_column_N),($row_N_column_1, $row_N_column_2, ... $row_N_column_N);
```
#### Example 
So for creating a single row 

```sql
INSERT INTO colours (id, name) VALUES (1, 'red');
```
For inserting multiple rows 
```sql
INSERT INTO colours (id, name, hex) VALUES (2, 'green',''), (3, 'blue','3399ff');
```

The output should be ```INSERT 0 2```  or similar.

To see if everything worked correctly, we can list all our records from our table by using the ```SELECT``` command.
```sql
SELECT * FROM $table_name;
```
So our command will be,
```sql
SELECT ALL FROM colours;
```
The result should be a list data.
![Selecting all records from our colours table.](https://i.ibb.co/VJgLCGB/image.png)
Now, with the ``*`` , we have selected all the columns. To select a specific column, we can specificy them in place of asterisk.
```sql
SELECT column_a, column_b FROM $table_name;
``` 
So our example will be
```sql
SELECT name, hex from colours;
```
![Returning specific columns](https://i.ibb.co/qMTRsHQ/image.png)



### Comparison operators
|syntax| what it means  | example |
|--|--|--|
| = | equals to | id = 2 |
| > | greater than | id > 2 |
| < | less than | id < 10 |
| >= | greater than or equals to | id >= 10 |
| <= | less than or equals to | id <= 100 |
| <> | not equals to | id <> 102 |
| \|\| | contact strings | 'hello ' \|\| 'world' |
| !!= | not in | 3 !! = i |
| ~~ | like | 'scrappy,marc,hermit' ~~ '%scrappy%' |
| !~~ | not like | 'bruce' !~~ '%al%' |
| ~ | match (regex), case sensitive | 'thomas' ~ '\*.thomas\*.' |
| ~* | match (regex), case insensitive | 'thomas' ~\* '\*.thomas\*.' |
| !~ | Does not match (regex), case sensitive | 'thomas' !~ '\*.Thomas\*.'|
| !~*| Does not match (regex), case insensitive | 'thomas' !~ '\*.vadim\*.' |

### Arithmetic Operators
|syntax| what it means  | example |
|--|--|--|
| + | addition | SELECT 2 + 5 |
| - | subtraction | SELECT 5 -2 |
| / | division | 10 / 2 |
| * | multiplication | 5 * 3 |
| % | modulo | 12 % 5 |
| % | truncate | % 4.5 |
| ! | factorial | 3! |
| !! | factorial (left operation) | !! 5 |
| : | natural Exponentiation | : 3.0 |
| ; | natural Logarithm | (; 5.0) |
| @ | absolute value | @ -5.0 |
| \|/ | square root | \|/ 25.0 |
| \|\|/ | cubic root | \|\|/ 27.0 |


### Sorting or Ordering data
To order our data by a column in a ascending order,
```sql
SELECT * FROM $table_name ORDER BY $column_name $ASC_OR_DESC;
```
so to order our data by name, 
```sql
SELECT * FROM users ORDER BY name ASC;
```
Our result looks like this,
![user's list](https://i.ibb.co/sw5ZScQ/image.png)
As you see the id doesn't add up to the next row's id.  If we try it in descending order, 
![enter image description here](https://i.ibb.co/JrPsxRh/image.png)

#### Combining multiple columns in sorting

```sql
SELECT * FROM $table_name ORDER BY $column_1, $column_2, ... $column_N $ASC_OR_DESC;
```
for example 
```sql
SELECT * FROM users ORDER BY id, name ASC;
```
**Note:** If you do not specify the order (ascending or descending), the default will be Ascending.
### Distinct or selecting the unique
```sql
SELECT DISTINCT $column_name from $table_name;
```
### WHERE & AND Conditionals 
```sql
SELECT * FROM users WHERE $column = $value;
```
For adding more than 1 condition, we use **AND**.
```sql
SELECT * FROM users WHERE $column = $value AND $another_column = $some_value AND ... $some_other_column_n = $some_value;
```
for example 
```sql
SELECT * FROM users WHERE id > 30 and gender = 'Male' ORDER BY name ASC;
```
And we have our results.
![enter image description here](https://i.ibb.co/NjM1VHj/image.png)

### OR 
syntax 
```sql
SELECT * FROM users WHERE $column = $value OR $column = $value; # columns can be same or different.
```
example.
```sql
SELECT * FROM users WHERE country = 'Bangladesh' OR country = 'Singapore' OR country = 'France';
```

### IN 
If you are quering in a single column, you can shorten the code by using in keyword.
syntax 
```sql
SELECT * FROM users WHERE country IN ('Bangladesh', 'Singapore','France'); 
```
**Note :** You can obviously use multiple *OR*, even with *IN* clause.

### Between
The between keyword is used to select data from a range.
syntax 
```sql
SELECT * FROM $table_name WHERE $column_name BETWEEN $starting_point AND $ending_point;
```
example
```sql
SELECT * FROM users WHERE id BETWEEN 1 AND 10;
```

### LIKE
Like is used to match patterns.
syntax
```sql
SELECT * FROM $table_name WHERE $column_name LIKE $pattern;
```
So, lets query out all the users who has 'United' in their country name.
query
```sql
SELECT name, country FROM users WHERE country LIKE '%United%' order by name;
```
![enter image description here](https://i.ibb.co/GWJgmrP/image.png)

#### patterns 

|like| description  |
|--|--|--|
| WHERE CustomerName LIKE 'a%' | Finds any values that start with "a"|
| WHERE CustomerName LIKE '%a'| Finds any values that end with "a"|
| WHERE CustomerName LIKE '%or%' | Finds any values that have "or" in any position|
|WHERE CustomerName LIKE '_r%' |Finds any values that have "r" in the second position|
|WHERE CustomerName LIKE 'a_%'|Finds any values that start with "a" and are at least 2 characters in length|
|WHERE CustomerName LIKE 'a__%'|Finds any values that start with "a" and are at least 3 characters in length|
|WHERE ContactName LIKE 'a%o'|Finds any values that start with "a" and ends with "o"|

**Note to self** underscores are used to match single characters.
### ILIKE
ILIKE is used to search with case insensitivity.

### Limit
syntax 
```sql
SELECT * FROM users LIMIT $limit;
```
example
```sql
SELECT * FROM users LIMIT 10;
```

![limiting](https://i.ibb.co/m6m8fp8/image.png)

**Note to self** Limit is not a SQL standard. We can achieve the same result by using **FETCH**.
syntax 
```sql
... FETCH FIRST N ROWS; # n is positive integer, can be ommited if n = 1.
```

### Offset
We often need to select records after a specific number of results. We might need to select 10 items from 30th. Meaning 30th to 40th elements of the results.Pagination is a good example of it.
If such scenarios, *OFFSET* comes into play. 
syntax 
```sql
SELECT * FROM $table_name OFFSET $n; # n = positive integer.
```
example
```sql
SELECT * FROM users OFFSET 5, LIMIT 10 ORDER BY id;
```

![enter image description here](https://i.ibb.co/Bj30Fzx/image.png)


### Data types
PostgresSQL supports a various number of data types. You can find them [here in the PostgresSQL supported data types document.](https://www.postgresql.org/docs/9.5/datatype.html)

### Relations


**Note to self** We don't need to memorize every bit but build up the queries from a simple statement and adding up more and more. 
