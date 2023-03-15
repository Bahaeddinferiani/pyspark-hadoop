FROM debian:9

#############################
# Owner: Bahaeddine FERIANI #
#############################

# For interactive packages
RUN DEBIAN_FRONTEND=noninteractive

# Basic tools
RUN apt-get -y update && \
    apt-get -y upgrade && \
    apt-get install -y  openjdk-8-jdk && \
    apt-get install -y  net-tools && \
    apt-get install -y  curl && \
    apt-get install -y  netcat && \
    apt-get install -y  gnupg && \
    apt-get install -y  libsnappy-dev && \
    apt-get install -y iputils-ping && \
    apt-get-get clean;

# Java home environment variable
ENV JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/

# Hadoop download and checking binary key
RUN curl -O https://dist.apache.org/repos/dist/release/hadoop/common/KEYS
RUN gpg --import KEYS
ENV HADOOP_VERSION 3.2.1
ENV HADOOP_URL https://archive.apache.org/dist/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
RUN set -x \
    && curl -fSL "$HADOOP_URL" -o /tmp/hadoop.tar.gz \
    && curl -fSL "$HADOOP_URL.asc" -o /tmp/hadoop.tar.gz.asc \
    && gpg --verify /tmp/hadoop.tar.gz.asc \
    && tar -xvf /tmp/hadoop.tar.gz -C /opt/ \
    && rm /tmp/hadoop.tar.gz*

# symbolic link for hadoop and configuring directories
RUN ln -s /opt/hadoop-$HADOOP_VERSION/etc/hadoop /etc/hadoop
RUN mkdir /opt/hadoop-$HADOOP_VERSION/logs
RUN mkdir /hadoop-data
ENV HADOOP_HOME=/opt/hadoop-$HADOOP_VERSION
ENV HADOOP_CONF_DIR=/etc/hadoop
ENV MULTIHOMED_NETWORK=1
ENV USER=root
ENV PATH $HADOOP_HOME/bin/:$PATH

# Entrypoint contains config for hadoop daemons
ADD entrypoint.sh /entrypoint.sh
RUN chmod a+x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
