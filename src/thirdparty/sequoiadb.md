# 通过 Spark 连接 SequoiaDB #

使用 Spark-SequoiaDB 连接器，就可以通过 Spark 对 SequoiaDB 的数据进行查询、统计操作。 详细介绍可以参考[ Spark-SequoiaDB 官网说明文档](http://doc.sequoiadb.com/cn/SequoiaDB-cat_id-1432190587-edition_id-0)

## 连接器说明 ##

Spark-SequoiaDB 连接器一共包含两个 jar 包：

|jar|说明|版本说明|
|----|----|---|
|spark-sequoiadb_2.11-3.2.0-SNAPSHOT.jar|Spark 连接 SequoiaDB 的连接器|scala 版本 2.11；SequoiaDB版本 3.2|
|sequoiadb-driver-3.2-SNAPSHOT.jar|访问 SequoiaDB 的驱动包|SequoiaDB 驱动包版本 3.2 |

## 安装连接器 ##

将上述两个 jar 包拷贝到 spark 安装目录 jars 下
``` shell
server1# cp spark-sequoiadb_2.11-3.2.0-SNAPSHOT.jar /opt/spark/jars/
server1# cp sequoiadb-driver-3.2-SNAPSHOT.jar /opt/spark/jars/
```

将两个 jar 包同步到所有 spark 节点的主机上（server2）
``` shell
server1# scp spark-sequoiadb_2.11-3.2.0-SNAPSHOT.jar username@server2:/opt/spark/jars/
server1# scp sequoiadb-driver-3.2-SNAPSHOT.jar username@server2:/opt/spark/jars/
```

重启 spark 节点
``` shell
server1# /opt/spark/sbin/stop-all.sh
server1# /opt/spark/sbin/start-all.sh
```

## 访问 SequoiaDB ##

SequoiaDB 数据库的安装使用请参考[ SequoiaDB 官网](http://www.sequoiadb.com/cn/)，此文档不再赘述。假设 SequoiaDB 数据库中已经建立好集合为 cs.cl 。

+ 启动 spark-sql
``` shell
server1# /opt/spark/spark-sql --master spark://server1:7077
```

+ 通过 spark 建立与 SequoiaDB 的映射表
``` scala
scala> create table test(name string, age int) using com.sequoiadb.spark options(host 'sdbserver1:11810', collectionspace 'cs', collection 'cl');
```

> **Note:**
>
> sdbserver1:11810 为 SequoiaDB 的 coord 节点服务地址
>
> cs 为 SequoiaDB 中已经存在的 collectionspace；cl 为已经存在的 collection

+ 通过 spark 查询 SequoiaDB 的数据

``` scala
scala> select * from test;
peppa 4
```

> **Note:**
>
> peppa 4 为 SequoiaDB 中集合 cs.cl 唯一的一条记录
