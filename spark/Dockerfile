# Version: 1.0
FROM ubuntu:20.04 as builder


#############################
# Owner: Bahaeddine FERIANI #
#############################

# For interactive dependencies 
ARG DEBIAN_FRONTEND=noninteractive

# Hadoop version / Spark environment variables
ENV SPARK_VERSION=3.2.3 \
HADOOP_VERSION=3.2 \
SPARK_HOME=/opt/spark 

# Install OpenJDK-8
RUN apt-get update && \
    apt-get install -y openjdk-8-jdk && \
    apt-get install -y ant && \
    apt-get clean;
    
# Fix certificate issues
RUN apt-get update && \
    apt-get install ca-certificates-java && \
    apt-get clean && \
    update-ca-certificates -f;

# Setup JAVA_HOME 
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME

# Pyspark dependencies and other tools
RUN apt-get update && \
    apt-get install -y curl && \
    apt-get install -y tzdata && \
    apt-get install -y vim && \
    apt-get install -y wget && \
    apt-get install -y software-properties-common && \
    apt-get install -y ssh && \
    apt-get install -y iputils-ping && \
    apt-get install -y net-tools && \
    apt-get install -y ca-certificates && \
    apt-get install -y unzip

# Download and uncompress spark from the apache archive
RUN wget --no-verbose -O apache-spark.tgz "https://dlcdn.apache.org/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz" && \
    mkdir -p /opt/spark && \
    tar -xf apache-spark.tgz -C /opt/spark --strip-components=1 && \
    rm apache-spark.tgz

# Apache spark environment
FROM builder as apache-spark

WORKDIR /opt/spark
    

# Environment variables for spark config
ENV SPARK_MASTER_PORT=7077 \
SPARK_MASTER_WEBUI_PORT=8080 \
SPARK_LOG_DIR=/opt/spark/logs \
SPARK_MASTER_LOG=/opt/spark/logs/spark-master.out \
SPARK_WORKER_LOG=/opt/spark/logs/spark-worker.out \
SPARK_WORKER_WEBUI_PORT=8080 \
SPARK_WORKER_PORT=7000 \
SPARK_MASTER="spark://master:7077" \
SPARK_WORKLOAD="master"

# Exposing ports
EXPOSE 8080 7077 6066

# Logs directory 
RUN mkdir -p $SPARK_LOG_DIR && \
touch $SPARK_MASTER_LOG && \
touch $SPARK_WORKER_LOG && \
ln -sf /dev/stdout $SPARK_MASTER_LOG && \
ln -sf /dev/stdout $SPARK_WORKER_LOG

# Spark startup script
COPY start-spark.sh /
RUN chmod 777 /start-spark.sh
CMD ["/bin/bash", "/start-spark.sh"]



