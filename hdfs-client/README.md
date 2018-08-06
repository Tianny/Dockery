## 制作 HDFS Client 镜像

## 下载 CDH 二进制包

```bash
http://archive-primary.cloudera.com/cdh5/cdh/5/hadoop-2.6.0-cdh5.13.1.tar.gz
```

## 修改 bin/hdfs

```bash
# 第 34 行改为hadoop-config.sh
# 原来是 hdfs-config.sh，这个脚本其实是去找 hadoop-config.sh 位置，为了简单化，这里直接指定
. $HADOOP_LIBEXEC_DIR/hadoop-config.sh
```

## 修改 bin/hadoop-config.sh

```bash
# 修改第89行
DEFAULT_CONF_DIR="/etc/hadoop/conf"
# 修改第93行，与上面一起指定配置文件位置放在 /etc/hadoop/conf 下，这样就不用再使用 --config 指定配置文件目录
export HADOOP_CONF_DIR="${HADOOP_CONF_DIR:-$DEFAULT_CONF_DIR}"

# 其他修改，hdfs 命令依赖一些jar包，这些jar包路径默认在 share/hadoop下。为了方便，修改了目录，方便管理
# 删除了一些关于 MapReduce 的脚本
```

## 寻找最小化 jar 包

直接执行 hdfs 客户端命令，提示少什么类，找到对应的 jar 包加进去就可以了

hdsf 客户端需要的 jar 包在 common/lib 和 hdfs/lib 下。

比如提示少 <code>org.apache.commons.codec.DecoderException</code> 这个 Class，去 lib 目录下，执行以下命令，就可以找到该类对应的 jar 包

```bash
find . -name '*.jar' -exec bash -c 'jar -tf {} | grep -iH --label {} org.apache.commons.codec.DecoderException' \;
```

## 用到的 jar 包

```bash
common/
|-- hadoop-common-2.6.0-cdh5.13.1.jar
`-- lib
    |-- commons-collections-3.2.2.jar
    |-- commons-configuration-1.6.jar
    |-- hadoop-auth-2.6.0-cdh5.13.1.jar
    |-- slf4j-api-1.7.5.jar
    `-- slf4j-log4j12-1.7.5.jar
```

```bash
hdfs/
|-- hadoop-hdfs-2.6.0-cdh5.13.1.jar
`-- lib
    |-- commons-cli-1.2.jar
    |-- commons-codec-1.4.jar
    |-- commons-daemon-1.0.13.jar
    |-- commons-el-1.0.jar
    |-- commons-io-2.4.jar
    |-- commons-lang-2.6.jar
    |-- commons-logging-1.1.3.jar
    |-- guava-11.0.2.jar
    |-- htrace-core4-4.0.1-incubating.jar
    |-- log4j-1.2.17.jar
    `-- protobuf-java-2.5.0.jar
```

## 构建镜像

```bash
docker build -t hdfs-client:latest
```

## 启动容器

```bash
docker run -dit --name hdfs-client hdfs-client:latest bash
```
