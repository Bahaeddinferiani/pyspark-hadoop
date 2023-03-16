# Version: 1.0
FROM ubuntu:20.04
USER root


#############################
# Owner: Bahaeddine FERIANI #
#############################

ENV PYSPARK_VERSION=3.2.3
# Install OpenJDK-8 and dependencies
RUN apt-get update && \
    apt-get install -y openjdk-8-jdk && \
    apt-get install -y ant && \
    apt-get install -y wget && \
    apt-get install -y unzip && \
    apt-get clean;
    
# Fix certificate issues
RUN apt-get update && \
    apt-get install ca-certificates-java && \
    apt-get clean && \
    update-ca-certificates -f;

# Setup JAVA_HOME 
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME

# Other dependencies and tools
RUN apt-get -y update && \
    apt-get -y upgrade && \
    apt-get install -y python3.8 && \
    apt-get install -y python3-pip && \
    apt-get install -y build-essential && \
    apt-get install -y libssl-dev && \
    apt-get install -y libffi-dev && \
    apt-get install -y python3-dev && \
    apt-get install -y iputils-ping && \
    apt-get install -y netcat;

# Mysql driver setup
RUN wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-8.0.32.zip && \
    unzip mysql-connector-j-8.0.32.zip && \
    cd mysql-connector-j-8.0.32 && \
    mv mysql-connector-j-8.0.32.jar /opt

# Jupyter lab & pyspark
RUN pip3 install jupyterlab && \
    pip3 install pyspark==${PYSPARK_VERSION}     
# Jupyter startup script
COPY start-jupyter.sh /
RUN chmod 777 /start-jupyter.sh


# Jupyter start with the container 
CMD ["bin/bash","/start-jupyter.sh"]
