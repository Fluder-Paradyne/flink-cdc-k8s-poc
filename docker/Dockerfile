FROM apache/flink:1.20.3-java17

# Set working directory
WORKDIR /opt/flink

# Install required dependencies
USER root

# Install Python for Flink Python API (if needed)
RUN apt-get update && \
    apt-get install -y python3 python3-pip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create directories for custom JARs
RUN mkdir -p /opt/flink/usrlib && \
    chown -R flink:flink /opt/flink/usrlib

ADD *.jar $FLINK_HOME/lib/
ADD flink-cdc-3.5.0-bin.tar.gz $FLINK_HOME/
RUN mv $FLINK_HOME/flink-cdc-3.5.0/lib/flink-cdc-dist-3.5.0.jar $FLINK_HOME/lib/

# Switch to flink user
USER flink

RUN wget -P /opt/flink/lib/ \
      https://repo1.maven.org/maven2/org/apache/flink/flink-cdc-pipeline-connector-mysql/3.5.0/flink-cdc-pipeline-connector-mysql-3.5.0.jar

RUN wget -P /opt/flink/lib/ \
      https://repo1.maven.org/maven2/org/apache/flink/flink-cdc-pipeline-connector-postgres/3.5.0/flink-cdc-pipeline-connector-postgres-3.5.0.jar

RUN wget -P /opt/flink/lib/ \
      https://repo1.maven.org/maven2/org/apache/flink/flink-cdc-pipeline-connector-iceberg/3.5.0/flink-cdc-pipeline-connector-iceberg-3.5.0.jar

RUN wget -P /opt/flink/lib/ \
      https://repo1.maven.org/maven2/org/apache/flink/flink-sql-connector-mysql-cdc/3.5.0/flink-sql-connector-mysql-cdc-3.5.0.jar

RUN wget -P /opt/flink/lib/ \
      https://repo1.maven.org/maven2/org/apache/flink/flink-sql-connector-postgres-cdc/3.5.0/flink-sql-connector-postgres-cdc-3.5.0.jar

RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/org/apache/flink/flink-sql-connector-mongodb-cdc/3.5.0/flink-sql-connector-mongodb-cdc-3.5.0.jar

# MySQL Connector
RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/com/mysql/mysql-connector-j/8.0.33/mysql-connector-j-8.0.33.jar

# Iceberg Flink Runtime (using 1.19 for Flink 1.20 compatibility)
RUN wget -P /opt/flink/lib/ \
    https://repo.maven.apache.org/maven2/org/apache/iceberg/iceberg-flink-runtime-1.20/1.9.2/iceberg-flink-runtime-1.20-1.9.2.jar

# Hive Metastore Dependencies
RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/org/apache/hive/hive-metastore/3.1.3/hive-metastore-3.1.3.jar

RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/org/apache/hive/hive-exec/3.1.3/hive-exec-3.1.3.jar

RUN wget -P /opt/flink/lib/ \
    https://jdbc.postgresql.org/download/postgresql-42.6.0.jar

# AWS/Hadoop Dependencies (from /hive-lib/aws/)
RUN wget -P /opt/flink/lib/ \
    https://repo.maven.apache.org/maven2/org/apache/hadoop/hadoop-aws/3.4.2/hadoop-aws-3.4.2.jar
RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/software/amazon/awssdk/bundle/2.29.52/bundle-2.29.52.jar
RUN wget -P /opt/flink/lib/ \
    https://repo.maven.apache.org/maven2/org/apache/hadoop/hadoop-common/3.4.2/hadoop-common-3.4.2.jar
RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/org/wildfly/openssl/wildfly-openssl/1.0.7.Final/wildfly-openssl-1.0.7.Final.jar

# Commons Dependencies (from /hive-lib/commons/)
RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/commons-configuration/commons-configuration/1.10/commons-configuration-1.10.jar
RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/commons-lang/commons-lang/2.6/commons-lang-2.6.jar
RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/commons-collections/commons-collections/3.2.2/commons-collections-3.2.2.jar
RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/commons-logging/commons-logging/1.2/commons-logging-1.2.jar

#RUN #wget -P /opt/flink/lib/ \
#    https://repo1.maven.org/maven2/org/apache/flink/flink-shaded-hadoop-2-uber/2.8.3-10.0/flink-shaded-hadoop-2-uber-2.8.3-10.0.jar

RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/org/apache/thrift/libfb303/0.9.3/libfb303-0.9.3.jar

RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/com/fasterxml/woodstox/woodstox-core/7.1.1/woodstox-core-7.1.1.jar

RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/org/codehaus/woodstox/stax2-api/4.2.2/stax2-api-4.2.2.jar

RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-client-runtime/3.4.2/hadoop-client-runtime-3.4.2.jar

# Corrected S3 deployment using the proper version (1.20.3)
RUN mkdir -p $FLINK_HOME/plugins/s3-fs-hadoop
RUN wget -P $FLINK_HOME/plugins/s3-fs-hadoop/ \
    https://repo1.maven.org/maven2/org/apache/flink/flink-s3-fs-hadoop/1.20.3/flink-s3-fs-hadoop-1.20.3.jar

RUN mkdir -p $FLINK_HOME/plugins/s3-fs-presto
RUN wget -P $FLINK_HOME/plugins/s3-fs-presto/ \
    https://repo1.maven.org/maven2/org/apache/flink/flink-s3-fs-presto/1.20.3/flink-s3-fs-presto-1.20.3.jar

#RUN wget -P /opt/flink/lib/ \
#    https://repo1.maven.org/maven2/org/apache/flink/flink-s3-fs-hadoop/1.20.3/flink-s3-fs-hadoop-1.20.3.jar

RUN wget -P /opt/flink/lib/ \
    https://repo.maven.apache.org/maven2/org/apache/hadoop/hadoop-mapreduce-client-core/3.4.2/hadoop-mapreduce-client-core-3.4.2.jar

RUN wget -P /opt/flink/lib/ \
    https://repo.maven.apache.org/maven2/org/apache/hadoop/hadoop-mapreduce-client-common/3.4.2/hadoop-mapreduce-client-common-3.4.2.jar

RUN wget -P /opt/flink/lib/ \
    https://repo.maven.apache.org/maven2/org/apache/hadoop/hadoop-mapreduce-client-jobclient/3.4.2/hadoop-mapreduce-client-jobclient-3.4.2.jar

RUN wget -P /opt/flink/lib/ \
    https://repo1.maven.org/maven2/org/apache/commons/commons-configuration2/2.9.0/commons-configuration2-2.9.0.jar

RUN wget -P /opt/flink/lib/ \
    https://repo.maven.apache.org/maven2/org/apache/hadoop/hadoop-hdfs/3.4.2/hadoop-hdfs-3.4.2.jar \
    https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-hdfs-client/3.4.2/hadoop-hdfs-client-3.4.2.jar

RUN wget -P /opt/flink/lib/ \
    https://repo.maven.apache.org/maven2/org/apache/commons/commons-collections4/4.5.0/commons-collections4-4.5.0.jar

RUN wget -P /opt/flink/lib/ \
    https://repo.maven.apache.org/maven2/org/apache/hadoop/hadoop-auth/3.4.2/hadoop-auth-3.4.2.jar

RUN wget -P /opt/flink/lib/ \
    https://repo.maven.apache.org/maven2/org/apache/iceberg/iceberg-hive-runtime/1.7.2/iceberg-hive-runtime-1.7.2.jar

RUN wget -P /opt/flink/lib/ \
    https://repo.maven.apache.org/maven2/org/apache/iceberg/iceberg-hive-metastore/1.9.2/iceberg-hive-metastore-1.9.2.jar \
    https://repo.maven.apache.org/maven2/org/apache/iceberg/iceberg-hive/0.9.1/iceberg-hive-0.9.1.jar \
    https://repo.maven.apache.org/maven2/org/apache/thrift/libthrift/0.9.3/libthrift-0.9.3.jar \
    https://repo.maven.apache.org/maven2/org/apache/iceberg/iceberg-hive3/1.7.2/iceberg-hive3-1.7.2.jar \
    https://repo1.maven.org/maven2/org/apache/flink/flink-core/1.20.3/flink-core-1.20.3.jar \
    https://repo.maven.apache.org/maven2/org/apache/iceberg/iceberg-aws/1.9.2/iceberg-aws-1.9.2.jar


ENV FLINK_HOME=/opt/flink
ENV PATH=$FLINK_HOME/bin:$PATH

# Healthcheck
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
    CMD curl -f http://localhost:8081/overview || exit 1

WORKDIR /opt/flink
