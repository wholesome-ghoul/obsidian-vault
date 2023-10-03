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
```
