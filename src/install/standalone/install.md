# Spark集群独立安装 #

Spark除了可以在Mesos和YARN集群上运行之外，还支持一种简单的独立部署模式。以在两台机器上安装 Spark 集群为例，说明安装步骤。

|主机名|Spark节点|安装路径
| ----|---------| --------
|server1|master|/opt/spark/|
|server1|worker|同一主机下只需要安装一次|
|server2|worker|/opt/spark/|

## 安装 Spark ##

### 解压安装包 ###

在 servre1 中使用 tar 命令解压 Spark 安装包，并安装到 /opt/spark 目录下。

``` shell
tar -zxvf spark-2.2.3-bin-hadoop2.7.tgz -C /opt/
mv /opt/spark-2.2.3-bin-hadoop2.7 /opt/spark
```

### 配置集群 ###

+ 配置 master 节点

    将 spark-env.sh.template 改名为 spark-env.sh
    ``` shell
    mv /opt/spark/conf/spark-env.sh.template /opt/spark/conf/spark-env.sh
    ```

    在 spark-env.sh 中增加 master 节点的主机名
    ``` shell
    SPARK_MASTER_HOST=server1
    ```

    > **Note:**
    > 
    > 其它配置均使用默认配置，master 的端口号默认为 7077 ， WebUI端口号为 8080 

+ 配置 worker 节点

    将 slaves.template 改名为 slaves
    ``` shell
    mv /opt/spark/conf/slaves.template /opt/spark/conf/slaves
    ```

    添加 worker 节点的主机名到 slaves 配置文件中
    ``` shell
    server1
    server2
    ```

+ 同步安装包到所有主机上

    将 /opt/spark 下所有文件同步到 server2 上
    ``` shell
    scp -r /opt/spark username@server2:/opt/
    ```

    > **Note:**
    > 
    > username 为 主机 server2 上的用户名

### 启动集群 ###

在主机 server1 上执行脚本启动 Spark 集群

``` shell
/opt/spark/sbin/start-all.sh
```

## 常见问题 ##

+ 启动远程节点失败

    + 请确保主机之间的安装路径、配置和 Java JDK 版本一致
    + 在 master 节点所在主机执行 start-all.sh 时，是通过 ssh 远程执行命令，加载的 shell 和 profile 配置可能与直接使用客户端工具登陆的不一样。可以在 server1 上尝试执行 ssh 命令确保 java 版本一致。
        ```
        ssh root@server2 java -version
        ```

        不一致时，可以通过 which 命令得到远程主机的 java 所在目录。直接登陆远程主机修正 java 的软链接
        ```
        ssh root@server2 which java
        ```
