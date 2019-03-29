# 简单操作 #

## spark-shell ##

+ 启动 spark-shell
    ```
    /opt/spark/bin/spark-shell --master spark://server1:7077
    ```

+ 读取共享文件
    ``` scala
    scala> var file = sc.textFile(/netfs/test.txt)
    file: org.apache.spark.rdd.RDD[String] = /netfs/test.txt MapPartitionsRDD[1] at textFile at <console>:24
    ```

    > **Note:**
    >
    > /netfs/test.txt 为远程共享文件，可以是网络文件系统中的目录
    >
    > 必须确保两台主机均能访问到共享文件

+ 获取文件行号
    ``` scala
    scala> file.count()
    res0: Long = 99
    ```

