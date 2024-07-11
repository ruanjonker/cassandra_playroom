FROM cassandra:4.1.5

# Copy the configuration file, all custom configs are tagged with ++RJ++
ADD config/cassandra.yaml /etc/cassandra/cassandra.yaml

ADD https://github.com/datastax/metric-collector-for-apache-cassandra/releases/download/v0.3.5/datastax-mcac-agent-0.3.5-4.1-beta1.tar.gz /tmp/datastax-mcac-agent-0.3.5-4.1-beta1.tar.gz

RUN command -v tar >/dev/null 2>&1 || { echo "tar is required but not installed. Aborting." >&2; exit 1; } \
    && tar -xzf /tmp/datastax-mcac-agent-0.3.5-4.1-beta1.tar.gz -C /tmp \
    && mv /tmp/datastax-mcac-agent-0.3.5-4.1-beta1 /opt/datastax-mcac-agent \
    && rm /tmp/datastax-mcac-agent-0.3.5-4.1-beta1.tar.gz
