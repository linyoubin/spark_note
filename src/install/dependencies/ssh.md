# ssh 免密登陆 #

Spark 的执行脚本，如 start-all.sh ，需要通过 ssh 远程登陆所有 worker 的主机去执行命令，所以需要开启免密登陆。

## 免密策略 ##

通常只要让 master 节点所在主机可以免密 ssh 到 worker 所在主机即可。以下说明以 master 所在主机为 server1 ，worker 所在主机为 server2 为例。

## 添加主机 ##

+ 分别在 server1 和 server2 的 /etc/hosts 上添加 各自的 hostname。示例地址如下：

    ``` shell
    server1# cat /etc/hosts
    192.168.20.1  server1
    192.168.20.2  server2
    ```

+ 验证是否成功
    ``` shell
    server1# ping server2
    ```

    需要两台机器互相可以 ping 通对方主机


## 在 server1 中生成私钥公钥 ##

+ 执行 ssh-keygen 生成私钥公钥

    ``` shell
    server1# ssh-keygen
    ```

    需要敲回车确认，默认选项即可。

+ 将 server1 的公钥添加到 server2 的信任列表中

    拷贝 server1 上的公钥串

    ``` shell
    server1# cat ~/.ssh/id_dsa.pub
    ssh-dss xxxxxxxxxxxxxxxxxxxx
    ```

    粘贴到 server2 上的信任列表中

    ``` shell
    server2# vi ~/.ssh/authorized_keys
    ```

    > **Note:**
    >
    > authorized_keys 文件不存在是创建即可

+ 验证是否成功

    在 server1 上远程 ssh 登陆 server2

    ``` shell
    server1# ssh username@server2
    ```

    > **Note:**
    >
    > username 为 server2 上的用户名


   


