---
title: Flink（一）Flink初体验
date: 2022-04-24 09:41:29
tags:
    - Flink
description: 本地下载Flink稳定开发版并试运行
---

# 运行前检查
为了运行Flink，只需要提前按转好 `Java8` 或 `Java11`。可以通过 `java -version` 命令检查是否已经安装正确。
``` sh
➜  ~ java -version
java version "1.8.0_181"
Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)
```

# 下载
这里使用当前最新的稳定版本 `v1.14.0`，下载Flink：[传送门](https://flink.apache.org/zh/downloads.html)
下载并解压
``` sh
➜  ~ cd flink  
➜  flink tar -zxvf flink-1.14.0-bin-scala_2.12.tgz
➜  flink cd flink-1.14.0/
```
> **验证文件的完整性**
在下载完成后，我们还需要验证下载文件的完整性（这里仅简单说明，详情请戳：[传送门](https://www.apache.org/info/verification.html)）
由Apache Software Foundation发布的所有正式版本的代码都由发布经理签署，PGP签名和SHA/MD5校验和随发行版一起提供，签名和校验和只能从Apache软件基金会的官方网站获得。
> - 文件哈希用于检查文件是否已正确下载。他们不对文件的真实性提供任何保证。只有检查哈希值，您才能确定您的下载没有被修改或不完整或有错误。
> - 一个正确的文件签名意味着文件没有被篡改。
> - 由于公钥密码学的性质，我们还需要对签名公钥的真实性进行验证。

# 目录结构
```
.
|____bin                        # 二进制程序目录，启动脚本
|____conf                       # 配置文件
|____examples                   # 示例
|____plugins                    # 插件
|____lib                        # 依赖库
|____opt
|____log                        # 日志
|____licenses                   # 许可
|____NOTICE
|____LICENSE
|____README.txt
```

# 启动集群
``` sh
➜  flink-1.14.4 ./bin/start-cluster.sh 
Starting cluster.
Starting standalonesession daemon on host localhost.
Starting taskexecutor daemon on host localhost.
```

# 访问 Flink Web Dashboard
程序启动之后，我们可以打开Flink的Web仪表盘，默认端口为 `8081`。Web面板可以查看正在运行的Job、已经完成的Job、任务管理、后台提交Job程序等。
![Flink Web Dashboard](Flink-Web-Dashboard.png)

# 提交作业
Flink 的 Releases 附带了许多的示例作业。你可以任意选择一个，快速部署到已运行的集群上。
``` sh
➜  flink-1.14.0 ./bin/flink run examples/streaming/WordCount.jar 
Executing WordCount example with default input data set.
Use --input to specify file input.
Printing result to stdout. Use --output to specify output path.
Job has been submitted with JobID d5998b738b9aa17c26de042d8624c964
Program execution finished
Job with JobID d5998b738b9aa17c26de042d8624c964 has finished.
Job Runtime: 949 ms

➜  flink-1.14.0 tail  log/flink-*-taskexecutor-*.out
(nymph,1)
(in,3)
(thy,1)
(orisons,1)
(be,4)
(all,2)
(my,1)
(sins,1)
(remember,1)
(d,4)
```
另外，可以通过 Flink 的 Web Dashboard 来监视集群的状态和正在运行的作业：
![Completed Jobs](Completed-Jobs.png)

# 停止集群
``` sh
➜  flink-1.14.0 ./bin/stop-cluster.sh 
Stopping taskexecutor daemon (pid: 57039) on host localhost.
Stopping standalonesession daemon (pid: 56796) on host localhost.
```