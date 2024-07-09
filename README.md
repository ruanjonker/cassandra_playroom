# Overview

This is a simple example of a Cassandra cluster with Reaper, Prometheus and
Grafana.

*Reaper is running with the `memory` backend, so it will lose all data when the container is restarted.*

## References

- [Reaper setup](https://github.com/thelastpickle/cassandra-reaper/blob/master/src/packaging/docker-compose.yml)
- [DataStax MCAC for metrics](https://github.com/datastax/metric-collector-for-apache-cassandra/tree/master)

## Download & Extract DataStax MCAC Agent for Cassandra Monitoring

```bash
curl -L https://github.com/datastax/metric-collector-for-apache-cassandra/releases/download/v0.3.5/datastax-mcac-agent-0.3.5-4.1-beta1.tar.gz -o datastax-mcac-agent-0.3.5-4.1-beta1.tar.gz

tar -xzf datastax-mcac-agent-0.3.5-4.1-beta1.tar.gz
```

## Run cluster

```bash
docker-compose up -d
```

## Check node statuses

NOTE: The cluster may take a few minutes to start up, depending on the machine
running the containers.

```bash
docker exec -it cassandra1 nodetool --username cassandraUser --password cassandraPass  status
```

## All DataStax MCAC metrics

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

## Python notebook

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```