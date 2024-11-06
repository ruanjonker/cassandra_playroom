FROM cassandra:4.1.7

COPY config/loop.sh /opt/loop.sh
RUN chmod a+rx /opt/loop.sh

COPY config/*.yaml /profiles/
RUN chmod a+r /profiles/*

ENV PATH=$CASSANDRA_HOME/tools/bin:$PATH

CMD ["/opt/loop.sh"]