# Cassandra

```bash
# installation
docker pull cassandra:latest

docker run --name cass_cluster cassandra:latest
# start cql shell to interact with Cassandra node
docker exec -it cass_cluster cqlsh

# config
# https://cassandra.apache.org/doc/latest/cassandra/getting-started/configuring.html

# prod recommendations
# https://cassandra.apache.org/doc/latest/cassandra/getting-started/production.html
```
