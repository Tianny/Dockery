## 寻找最小化 jar 包方法

直接执行 hdfs 客户端命令，提示少什么类，找到对应的 jar 包加进去就可以了

hdsf 客户端需要的 jar 包在 common/lib 和 hdfs/lib 下。

比如提示少 <code>org.apache.commons.codec.DecoderException</code> 这个 Class，去 lib 目录下，执行以下命令，就可以找到该类对应的 jar 包

```bash
find . -name '*.jar' -exec bash -c 'jar -tf {} | grep -iH --label {} org.apache.commons.codec.DecoderException' \;
```

## 构建镜像

```bash
docker build -t hdfs-client:latest
```

## 启动容器（无 Kerberos 认证）

```bash
docker run -dit --name my-hdfs hdfs:latest bash
```

## 启动容器（Kerberos 认证）

用户需要将自己的 keytab 文件挂载到容器中指定的文件 <code>/root/kerberos.keytab</code>，并通过变量 <code>PRINCIPAL_NAME</code> 传递用户的 principal

> Tips: 挂载的文件需要写绝对路径，否则在容器中会挂载为创建目录

```bash
docker run -dit --name my-hdfs -v /tmp/hdfs.keytab:/root/kerberos.keytab -e PRINCIPAL_NAME="hdfs/kt1@DEV.DXY.CN" hdfs:latest bash
```
