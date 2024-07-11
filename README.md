# Overview

This is a simple example of a Cassandra cluster with Reaper, Prometheus and
Grafana.

*Reaper is running with the `memory` backend, so it will lose all data when the container is restarted.*

Metrics are collected through the `DataStax MCAC` java agent (injected as a JAR
on each cassandra node).

Config files for for the different components can be found in the `config`
directory.

## References

- [Reaper](https://github.com/thelastpickle/cassandra-reaper/tree/master)
- [DataStax MCAC](https://github.com/datastax/metric-collector-for-apache-cassandra/tree/master)

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

## All DataStax MCAC metrics

NOTE: The metrics are filtered to reduce load so that the cluster can run on a
reasonably spec'ed laptop. If you want to get a list of ALL available metrics,
comment the filters in
`config/datastax-mcac-agent-0.3.5-4.1-beta1/metric-collector.yaml` and start the
`cassandra1` node, wait for it to be up and run the below.

```bash
curl http://localhost:9103/metrics -o metrics.txt
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