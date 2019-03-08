> 寻找最小化 jar 包方法举例:

> 直接执行 hdfs 客户端命令，提示少什么类，找到对应的 jar 包加进去就可以了

> hdsf 客户端需要的 jar 包在 common/lib 和 hdfs/lib 下。

> 比如提示少 <code>org.apache.commons.codec.DecoderException</code> 这个 Class，去 lib 目录下，执行以下命令，就可以找到该类对应的 jar 包

```bash
find . -name '*.jar' -exec bash -c 'jar -tf {} | grep -iH --label {} org.apache.commons.codec.DecoderException' \;
```
## 准备工作

- 将各客户端子目录 jars 中的 jar 包替换为实际的 CDH 版本（目前为 CDH 5.13.1）
- 将各客户端子目录 conf 中的配置文件替换为实际环境的配置文件
- kr5.conf 替换为实际环境的

## 构建镜像

```bash
docker build -t hadoop-client:latest
```

## 启动容器（无 Kerberos 认证）

```bash
docker run -dit --name my-hadoop hadoop-client:latest bash
```

## 启动容器（Kerberos 认证）

用户需要将自己的 keytab 文件挂载到容器中指定的文件 <code>/root/kerberos.keytab</code>，并通过变量 <code>PRINCIPAL_NAME</code> 传递用户的 principal

> Tips: 挂载的文件需要写绝对路径，否则在容器中会挂载为创建目录

```bash
docker run -dit --name my-hadoop -v /tmp/tianny.keytab:/root/kerberos.keytab -e PRINCIPAL_NAME="tianny@DEV.CN" hadoop-client:latest bash
```

## 使用示例

```bash
beeline -u jdbc:hive2://kt1:10000/default\;principal=hive/kt1@DEV.CN

hdfs dfs -ls /

sqoop import --connect jdbc:mysql://kt1/mysql --username xxx --password xxx --table db --hive-import --hive-table db -m 1
```
