# Overview

This is a simple example of a Cassandra cluster with Reaper, Prometheus and
Grafana.

*Reaper is running with the `memory` backend, so it will lose all data when the container is restarted.*

Metrics are collected through the `JMX Exporter form Prometheus` java agent (injected as a JAR
on each cassandra node).

Config files for for the different components can be found in the `config`
directory.

## References

- [Reaper](https://github.com/thelastpickle/cassandra-reaper/tree/master)
- [JMX Exporter (for Prometheus)](https://github.com/prometheus/jmx_exporter/blob/main/docs/README.md)

## Run cluster

```bash
docker compose build
docker compose up -d
```

## Check node statuses

NOTE: The cluster may take a few minutes to start up, depending on the machine
running the containers.

```bash
docker exec -it cassandra1 nodetool --username cassandraUser --password cassandraPass  status
```

## Reaper

Login to the Reaper [UI](http://localhost:8080/webui/) with username `admin` and password `admin`.

Click on "Add Cluster" and fill in the following information:

- Seed node: cassandra1
- JMX port: 7199
- JMX username: reaperUser
- JMX password: reaperPass

## Grafana

Login to the Grafana [UI](http://localhost:3000/) with username `admin` and
password `admin`.

## Setup for python notebook

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```