FROM cassandra:4.1.5

# Copy the configuration file, all custom configs are tagged with ++RJ++
ADD config/cassandra.yaml /etc/cassandra/cassandra.yaml

ADD https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/1.0.1/jmx_prometheus_javaagent-1.0.1.jar /opt/jmx_prometheus/jmx_prometheus_javaagent-1.0.1.jar

RUN chmod a+r /opt/jmx_prometheus/jmx_prometheus_javaagent-1.0.1.jar