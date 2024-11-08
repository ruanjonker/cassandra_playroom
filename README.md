# Cassandra Playroom Overview

This is a simple setup of a Cassandra cluster with Reaper, Prometheus,
Grafana as well as Neo4j running in [Docker](https://www.docker.com/).
The purpose of this project is to provide a "playroom" for those who
want to familiarize themselves with Apache Cassandra and Neo4j.

## Noteworthy

Neo4j enterprise is used with the "evaluation" license
agreement which allows you 30 days to evaluate it. Set the
`NEO4J_ACCEPT_LICENSE_AGREEMENT` environment variable in the
`docker-compose.yml` to `yes` (from the default `eval`) if
you whish to do so. Read more [here](https://neo4j.com/docs/operations-manual/current/docker/introduction/#_neo4j_editions)
about the different Neo4j editions and licensing stuff.

This project also has a jupyter notebook (`cassandra.ipynb`) that
has some examples of how to interact with cassandra and neo4j.

You'll need docker preinstalled and you'll need at least `6GB` of
ram that can be dedicated to the docker containers - if one of the
cassandra nodes unexpectedly dies, then it is probably because the
docker daemon OOM killed it, it should automatically be restarted
by the docker daemon (specified in `docker-compose.yml`). You can
adjust the amount of ram used by cassandra changing `MAX_HEAP_SIZE`
and `NEW_HEAPSIZE` in the `docker-compose.yml` file. You can also
tweak the amount of ram allocated to each docker container by
adjusting the `mem_limit` for the respective service entries in the
`docker-compose.yml` files.

Metrics are collected through the `JMX Exporter form Prometheus` java agent
(injected via a JAR on each cassandra node).

Config files for for the different components can be found in the `config`
directory.

## References

- [Cassandra](https://cassandra.apache.org/doc/4.1/index.html)
- [Neo4j](https://neo4j.com/)
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
docker exec -it tools nodetool -h cassandra1 status
docker exec -it tools nodetool -h cassandra2 status
docker exec -it tools nodetool -h cassandra3 status
```

## Neo4j

Login to the [Neo4j Browser](http://localhost:7474/browser/) with username
`neo4j` and password `password`.

## Reaper

Login to the [Reaper UI](http://localhost:8080/webui/) with username `admin`
and password `admin`.

Click on "Add Cluster" and fill in the following information:

- Seed node: cassandra1

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

## Running the stress tool

Even though it does not make a lot of sense to stress test cassandra
in this "playroom" context, below you can find an example of how to run
the `cassandra-stress` tool.

```bash
# Writes only
docker exec -ti tools cassandra-stress user profile=/profiles/simple-stress.yaml ops\(insert=1\) -node cassandra1,cassandra2,cassandra3 -rate threads=8 -graph file=/reports/stress.html title=PlayroomStress revision=insert_run1

# Reads only
docker exec -ti tools cassandra-stress user profile=/profiles/simple-stress.yaml ops\(simple1=1\) -node cassandra1,cassandra2,cassandra3 -rate threads=8 -graph file=/reports/stress.html title=PlayroomStress revision=read_run1

# 10% Write and 90% Read
docker exec -ti tools cassandra-stress user profile=/profiles/simple-stress.yaml ops\(insert=1,simple1=9\) -node cassandra1,cassandra2,cassandra3 -rate threads=8 -graph file=/reports/stress.html title=PlayroomStress revision=rw_run1

# Copy the generated graph & report
docker cp tools:/reports/stress.html stress.html
```
