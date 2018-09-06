FROM registry.uc.host.dxy/library/java
MAINTAINER xietian@dxy.cn

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -yq krb5-user

COPY krb5.conf /etc/
COPY beeline/ /opt/beeline
COPY hdfs/ /opt/hdfs
COPY hive/ /opt/hive
COPY sqoop/ /opt/sqoop

ENV PATH="${PATH}:/opt/beeline/bin:/opt/hdfs/bin:/opt/sqoop/bin"
