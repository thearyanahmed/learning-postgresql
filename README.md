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
	hex varchar(6)
);
```
To know if it worked, we can check tables,  by typeing ```\dt``` while we are in the database.
![List of all tables](https://i.ibb.co/L5Gwg2t/image.png)

As of now, we have our table and we can store some data in it. To store data, we use a **insert** query. The insert query is structured as follows 
```sql
INSERT INTO $table_name VALUES ($value_of_column_1, $value_of_column_2, ... $value_of_column_N);
```
This will insert a single row. If we want to insert multiple rows at once, we can append to the values.

```sql 
INSERT INTO $table_name VALUES ($row_1_column_1, $row_1_column_2, ... $row_1_column_N),($row_2_column_1, $row_2_column_2, ... $row_2_column_N),($row_N_column_1, $row_N_column_2, ... $row_N_column_N);
```
#### Example 
So for creating a single row 

```sql
INSERT INTO colours VALUES (1, 'red');
```
For inserting multiple rows 
```sql
INSERT INTO colours VALUES (2, 'green',''), (3, 'blue','3399ff');
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
