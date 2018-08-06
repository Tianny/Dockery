FROM registry.uc.host.dxy/library/java
MAINTAINER xietian@dxy.cn

RUN export DEBIAN_FRONTEND=noninteractive && apt-get update && apt-get install -yq krb5-user

ADD hdfs-client-2.6.0-cdh5.13.1.tar.gz /opt/
COPY krb5.conf /etc/

RUN mkdir /etc/hadoop/conf -p
COPY core-site.xml hdfs-site.xml /etc/hadoop/conf/

RUN ln -s /opt/hdfs-client-2.6.0-cdh5.13.1 /opt/hdfs-client-2.6.0

ENV JAVA_HOME /usr
ENV HADOOP_HOME /opt/hdfs-client-2.6.0
ENV PATH ${PATH}:${HADOOP_HOME}/bin
