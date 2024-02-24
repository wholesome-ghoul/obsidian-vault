```sql
CREATE USER <user> WITH PASSWORD '<password>' CREATEDB;
```

```bash
sudo apt-get install postgresql
sudo systemctl start postgresql
sudo -u postgres psql

createdb <db-name>

psql -U <user> -d <db-name>
psql -U <user> -d <db-name> -a -f <file.sql>

# check clusters' status
pg_lsclusters

# binary locations (initdb, etc)
# /usr/lib/postgresql/<version>/bin

# Creating a cluster
mkdir /usr/local/pgsql
chown <user> /usr/local/pgsql
su <user>
pg_ctl -D /usr/local/pgsql/data initdb
pg_ctl -D /usr/local/pgsql/data -l logfile start

# Stop a cluster
# pg_ctlcluster <version> <cluster> stop
pg_ctlcluster 13 main stop

# load database
pg_restore -U postgres -d dvdrental file.tar
```


```sql
ALTER TABLE <table-name> RENAME COLUMN <old-name> TO <new-name>;

-- \l will show all dbs in psql server

-- switch to db
-- \c databsaename

-- display all tables
-- \dt

-- all available psql cmds
-- \?

-- set table output option.
-- null will be displayed:
-- \pset null null

-- concatenation
SELECT
  first_name || ' ' || last_name,
  -- with column alias
  -- first_name || ' ' || last_name AS column_alias,
  -- first_name || ' ' || last_name column_alias,
  -- first_name || ' ' || last_name "column alias",
  email
FROM
  customer;

SELECT NOW();

SELECT
  first_name,
  LENGTH(last_name) len
FROM
  customer
ORDER BY
  len DESC;

-- ORDER BY sort_expression [ASC | DESC] [NULLS FIRST | NULLS LAST]

SELECT
  distinct first_name,
  last_name,
FROM
  customer
ORDER BY
  first_name,
  last_name
;
```
