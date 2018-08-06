FROM ubuntu
MAINTAINER tiannyxie@gmai.com

RUN apt-get update && apt-get install -y krb5-user

ADD hadoop-2.6.0-cdh5.13.1.tar.gz /opt/
ADD jdk-8u172-linux-x64.tar.gz /opt/
COPY krb5.conf /etc/

RUN mkdir /hadoop_conf
COPY core-site.xml hdfs-site.xml /hadoop_conf/

RUN ln -s /opt/jdk1.8.0_172 /opt/java8
RUN ln -s /opt/hadoop-2.6.0-cdh5.13.1 /opt/hadoop-2.6.0

ENV JAVA_HOME /opt/java8
ENV HADOOP_HOME /opt/hadoop-2.6.0
ENV PATH ${PATH}:${JAVA_HOME}/bin:${HADOOP_HOME}/bin
