# Postgresql Database


## Docs links

[master slave configuration](https://github.com/akhilrajmailbox/Postgresql-Database/blob/master/postgresql-master-slave-replication.pdf)

[postgresql cluster configuration](https://github.com/akhilrajmailbox/Postgresql-Database/tree/master/postgresql-cluster)


## Cluster Scripts


[remote server script](https://raw.githubusercontent.com/akhilrajmailbox/Postgresql-Database/master/scripts/remote.sh)

[remote server failover](https://raw.githubusercontent.com/akhilrajmailbox/Postgresql-Database/master/scripts/remotefail.sh)


```
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
                                               Postgresql query
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
```

## for get into db (postgres is default user)

```
sudo -i -u postgres psql
```

## connect to database by a user in remote and localhost

```
psql -h host -U user -d database
```


## create a user

```
CREATE USER odoo WITH PASSWORD 'odoo';
```

## list user

```
SELECT usename FROM pg_user;
```

## drop user

```
DROP USER tom;
```

## drop database

```
DROP DATABASE newone;
```

## to give passwd newly for postgres or changing passwd for users

```
ALTER USER postgres PASSWORD 'postgres';
```

## to give privilage for create bd

```
ALTER USER odoo WITH CREATEDB;
```

## to grant privilages

```
GRANT ALL PRIVILEGES ON DATABASE jerry to tom;
```

## for selection of database

```
SELECT datname FROM pg_database WHERE datistemplate = false;
or \l
or \list

To switch databases:
\connect database_name
or 
\connect database_name username
```


## Readonly user configuration
```
\c databasename;
CREATE USER username WITH PASSWORD 'your_password';
GRANT CONNECT ON DATABASE database_name TO username;
GRANT USAGE ON SCHEMA schema_name TO username;
GRANT SELECT ON ALL TABLES IN SCHEMA schema_name TO username;
ALTER DEFAULT PRIVILEGES IN SCHEMA schema_name
GRANT SELECT ON TABLES TO username;
```

## connect to Database Server
```
psql -h datamall-dev.postgres.database.azure.com -U USER_NAME@SERVER_NAME --dbname=postgres
```

## Create Users and databases for each modules

### Users
```
CREATE USER module1 WITH PASSWORD 'UNIQUE_PASSWORD';
CREATE USER module2 WITH PASSWORD 'UNIQUE_PASSWORD';
```

### databases
```
CREATE DATABASE db_module1;
CREATE DATABASE db_module2;
```

### revoke the default access cpnnect permission for the databases
```
REVOKE connect ON DATABASE db_module1 FROM PUBLIC;
REVOKE connect ON DATABASE db_module2 FROM PUBLIC;
```

## Grant permission
```
GRANT ALL PRIVILEGES ON DATABASE db_module1 to module1;
GRANT ALL PRIVILEGES ON DATABASE db_module2 to module2;
```


## This lists tables in the current database

```
SELECT table_schema,table_name FROM information_schema.tables ORDER BY table_schema,table_name;
```

```
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
```

error:::::

InvalidParameterValue: ERROR:  new encoding (UTF8) is incompatible with the encoding of the template database (SQL_ASCII)
HINT:  Use the same encoding as in the template database, or use template0 as template.

: CREATE DATABASE "my_db_name" ENCODING = 'unicode'.......



solutions::::


1. First, we need to drop template1. Templates can’t be dropped, so we first modify it so t’s an ordinary database:

`        UPDATE pg_database SET datistemplate = FALSE WHERE datname = 'template1';`

2. Now we can drop it:

`        DROP DATABASE template1;`

3. Now its time to create database from template0, with a new default encoding:

`        CREATE DATABASE template1 WITH TEMPLATE = template0 ENCODING = 'UNICODE';`

4. Now modify template1 so it’s actually a template:

`        UPDATE pg_database SET datistemplate = TRUE WHERE datname = 'template1';`

5. Now switch to template1 and VACUUM FREEZE the template:

`        \c template1`

`        VACUUM FREEZE;`

Problem should be resolved.


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


### Error: permission denied to create database ??


You should do following steps:



1. ssh to your server where odoo is installed

2. sudo su postgres
(the idea is to switch to postgresql user)

3. psql

4. ALTER USER odoo WITH CREATEDB;

5. Go back to web page and try create db once again
to retrieve all the rows of table weather, type:
SELECT * FROM weather;


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


## to give permissions for the users with out passwd 

```
edit /etc/postgresql/9.3/main/pg_hba.conf


local   all             all                                     peer
change to 
local   all             all                                     trust 


local   all             postgres                                peer
change to
local   all             postgres                                trust
```

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%



