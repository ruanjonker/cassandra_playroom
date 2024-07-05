# Overview

This is a simple example of a Cassandra cluster with Reaper. Reaper is running
with the `memory` backend, so it will lose all data when the container is
restarted.

# References

- [Reaper in Docker Compose](https://github.com/thelastpickle/cassandra-reaper/blob/master/src/packaging/docker-compose.yml)

# Run cluster

```bash
docker-compose up -d
```

# Check node statuses

NOTE: The cluster may take a few minutes to start up, depending on the machine
running the containers.

```bash
docker exec -it cassandra1 nodetool --username cassandraUser --password cassandraPass  status
```

# Reaper

Login to the Reaper [UI](http://localhost:8080/webui/) with username `admin` and password `admin`.

Click on "Add Cluster" and fill in the following information:

- Seed node: cassandra1
- JMX port: 7199
- JMX username: reaperUser
- JMX password: reaperPass

# Python notebook

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```