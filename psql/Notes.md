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
```