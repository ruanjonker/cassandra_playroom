FROM cassandra:5.0

# Copy the configuration file, all custom configs are tagged with ++RJ++
RUN mv /etc/cassandra/cassandra.yaml /etc/cassandra/cassandra.yaml.orig
COPY config/cassandra.yaml /etc/cassandra/cassandra.yaml
RUN chown cassandra:cassandra /etc/cassandra/cassandra.yaml

# Copy the cassandra-env.sh file, all custom configs are tagged with ++RJ++
RUN mv /etc/cassandra/cassandra-env.sh /etc/cassandra/cassandra-env.sh.orig
COPY config/cassandra-env.sh /etc/cassandra/cassandra-env.sh
RUN chmod a+rw /etc/cassandra/cassandra-env.sh
RUN chown cassandra:cassandra /etc/cassandra/cassandra-env.sh

ADD https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/1.0.1/jmx_prometheus_javaagent-1.0.1.jar /opt/jmx_prometheus/jmx_prometheus_javaagent-1.0.1.jar
RUN chmod a+r /opt/jmx_prometheus/jmx_prometheus_javaagent-1.0.1.jar

RUN apt-get update
RUN apt-get install python3-pip -y
RUN python3 -m pip install cassandra-medusa
