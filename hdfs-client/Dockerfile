FROM registry.uc.host.dxy/library/java
MAINTAINER xietian@dxy.cn

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -yq krb5-user

COPY krb5.conf /etc/
COPY conf /opt/conf/
COPY jars /opt/jars/
COPY hdfs /usr/bin/
