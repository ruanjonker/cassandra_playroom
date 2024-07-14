# Cassandra Playroom Overview

This is a simple setup of a Cassandra cluster with Reaper, Prometheus
and Grafana. The purpose of this project is for those who want to familiarize
themselves with Apache Cassandra.

Metrics are collected through the `JMX Exporter form Prometheus` java agent
(injected via a JAR on each cassandra node).

Config files for for the different components can be found in the `config`
directory.

## References

- [Cassandra](https://cassandra.apache.org/doc/4.1/index.html)
- [Reaper](https://github.com/thelastpickle/cassandra-reaper/tree/master)
- [JMX Exporter (for Prometheus)](https://github.com/prometheus/jmx_exporter/blob/main/docs/README.md)
- [Prometheus](https://prometheus.io/)
- [Grafana](https://grafana.com/)

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

Login to the [Reaper UI](http://localhost:8080/webui/) with username `admin` and password `admin`.

Click on "Add Cluster" and fill in the following information:

- Seed node: cassandra1
- JMX port: 7199
- JMX username: reaperUser
- JMX password: reaperPass

## Prometheus

You can connect to [prometheus using this link](http://localhost:9090/)

## Grafana

Login to the [Grafana UI](http://localhost:3000/) with username `admin` and
password `admin`.

## Setup for python notebook

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```
