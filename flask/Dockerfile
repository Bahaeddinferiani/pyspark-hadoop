FROM ubuntu:20.04

#############################
# Owner: Bahaeddine FERIANI #
#############################


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

# Mysql driver setup
RUN wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j-8.0.32.zip && \
    unzip mysql-connector-j-8.0.32.zip && \
    cd mysql-connector-j-8.0.32 && \
    mv mysql-connector-j-8.0.32.jar /opt

# Python dependencies 
RUN apt-get -y update && \
    apt-get -y upgrade && \
    apt-get install -y python3.8 && \
    apt-get install -y python3-pip && \
    apt-get install -y build-essential && \
    apt-get install -y libssl-dev && \
    apt-get install -y libffi-dev && \
    apt-get install -y python3-dev && \
    apt-get install -y iputils-ping && \
    apt-get clean;

ENV PYSPARK_VERSION=3.2.3

# Flask and Pyspark
RUN pip3 install flask && \
    pip3 install pyspark==${PYSPARK_VERSION}

# Copy web server files
COPY /flask /flask

# Copy startup script
COPY start-app.sh /

WORKDIR /flask
CMD [ "/bin/bash", "/start-app.sh" ]
