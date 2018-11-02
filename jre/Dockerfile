FROM debian:jessie

RUN apt-get update && apt-get install locales
RUN locale-gen zh_CN.UTF-8
COPY locale.gen /etc/
RUN locale-gen
ENV LANG zh_CN.UTF-8
ENV LANGUAGE zh_CN.UTF-8
ENV LC_ALL zh_CN.UTF-8

ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN mkdir -p /usr/lib/jvm
ADD jre-8u171-linux-x64.tar.gz /usr/lib/jvm
RUN ln -s /usr/lib/jvm/jre1.8.0_171/bin/java /usr/bin/java
RUN cd /usr/lib/jvm/jre1.8.0_171 \
    && find . -type f -name '*.html' -delete \
    && find . -type f -name 'COPYRIGHT' -delete \
    && find . -type f -name 'LICENSE' -delete \
    && find . -type f -name 'README' -delete \
    && find . -type f -name '*.txt' -delete \
