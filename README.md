# Learning postgresSQL
Tinkering with postgresSQL.


PostgresSQL is an open-source relational database maangement system. 

## starting postgresSQL
To start/stop/control the server I'll be using **pg_ctl**. 
```
pg_ctl -D /usr/local/var/postgres start
```
I'm using a mac, so the directory **/usr/local/var/postgres** might vary from your machine.


## Using the shell
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

## Creating a database

```
create DATABASE $dbName;

# example 
create DATABASE test_db;
```

the output should be
```
CREATE DATABASE
```

## listing all databases
To list all database in the interactive shell, type ```\l```, it is not a SQL command so no need to use semi colons.



