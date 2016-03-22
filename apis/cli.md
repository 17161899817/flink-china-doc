---
title:  "命令行接口（CLI）"
# Top-level navigation
top-nav-group: apis
top-nav-pos: 5
---
<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

Flink提供了一个命令行接口（CLI）用来运行打成JAR包的程序，并且可以控制程序的执行。
命令行接口可以用于本地单节点或是分布式的部署安装，它是Flink安装工具的一个组件。
这个工具位于 `<flink-home>/bin/flink`，
默认会连接运行中的 Flink master（JobManager），
master 的启动脚本与 CLI 在同一安装目录下。

因此使用命令行接口（CLI）的先决条件是启动一个master (JobManager)
 (通过命令： 
`<flink-home>/bin/start-local.sh` 或
`<flink-home>/bin/start-cluster.sh`) 或是在 Flink YARN 环境中。

命令行可以被用于

- 提交一个job
- 取消正在运行的job
- 提供一个job的信息
- 列出正在运行的和等待的job列表

* This will be replaced by the TOC
{:toc}

## 示例

-   运行示例程序，不传参数：

        ./bin/flink run ./examples/batch/WordCount.jar

-   运行示例程序，带输入输出文件参数：

        ./bin/flink run ./examples/batch/WordCount.jar \
                               file:///home/user/hamlet.txt file:///home/user/wordcount_out

-   运行示例程序，设置16个并发度，并且带输入输出文件参数：

        ./bin/flink run -p 16 ./examples/batch/WordCount.jar \
                                file:///home/user/hamlet.txt file:///home/user/wordcount_out

-   运行示例程序，并禁止 Flink 在标准输出输出日志

            ./bin/flink run -q ./examples/batch/WordCount.jar

-   以独立（detached）模式运行示例程序：

            ./bin/flink run -d ./examples/batch/WordCount.jar

-   运行示例程序，指定 JobManager：

        ./bin/flink run -m myJMHost:6123 \
                               ./examples/batch/WordCount.jar \
                               file:///home/user/hamlet.txt file:///home/user/wordcount_out

-   运行示例程序，指定程序入口类的 class：

        ./bin/flink run -c org.apache.flink.examples.java.wordcount.WordCount \
                               ./examples/batch/WordCount.jar \
                               file:///home/user/hamlet.txt file:///home/user/wordcount_out

-   运行示例程序，使用 [per-job YARN 集群]({{site.baseurl}}/setup/yarn_setup.html#run-a-single-flink-job-on-hadoop-yarn) 启动 2 个TaskManager：

        ./bin/flink run -m yarn-cluster -yn 2 \
                               ./examples/batch/WordCount.jar \
                               hdfs:///user/hamlet.txt hdfs:///user/wordcount_out

-   对于WordCount示例程序，以JSON的格式输出优化后的执行计划：

        ./bin/flink info ./examples/batch/WordCount.jar \
                                file:///home/user/hamlet.txt file:///home/user/wordcount_out

-   列出已经调度的和正在运行的job（包含JobID信息）：

        ./bin/flink list

-   列出已经调度的job（包含JobID信息）：

        ./bin/flink list -s

-   列出正在运行的job（包含JobID信息）：

        ./bin/flink list -r

-   取消一个job：

        ./bin/flink cancel <jobID>

-   停止一个job(只适用于流式计算的job)：

        ./bin/flink stop <jobID>

### Savepoints

[Savepoints]({{site.baseurl}}/apis/streaming/savepoints.html) 通过命令行客户端来控制。

#### savepoint触发命令

{% highlight bash %}
./bin/flink savepoint <jobID>
{% endhighlight %}

返回一个已经创建的savepoint的路径。你需要通过这个路径来恢复或销毁savepoints。

#### **恢复一个savepoint**:

{% highlight bash %}
./bin/flink run -s <savepointPath> ...
{% endhighlight %}

这个 run 命令提交 job 时带有一个 savepoint 标记，这使得程序可以从 savepoint 保存的状态中恢复。
savepoint路径是通过savepoint触发命令得到的。

#### **销毁一个savepoint**:

{% highlight bash %}
./bin/flink savepoint -d <savepointPath>
{% endhighlight %}

销毁一个savepoint同样需要一个路径。
这个savepoint路径是通过savepoint触发命令得到的。

## 用法

命令行的语法如下:

~~~
./flink <ACTION> [OPTIONS] [ARGUMENTS]

下面列举一些可用的actions:

指令 "run" 编译并且运行一个程序.

  语法: run [OPTIONS] <jar-file> <arguments>
  "run" action 选项:
     -c,--class <classname>               程序的入口类("main" 方法或 "getPlan()" 方法。）
                                          只有当 JAR 文件没有在 mainifest 中指定入口类时才需要。
     -C,--classpath <url>                 在集群中的每个节点上为每个用户代码类加载器添加一个 URL 资源。
                                          下会影响所有节点. 路径必须带上协议信息
                                           (如 file://) 并且所有结点都可以访问
                                           (例如NFS共享文件)。 你可以多次使用这个选项
                                           来添加多个资源. 协议必须被{@link
                                          java.net.URLClassLoader}支持。
     -d,--detached                        如果加上这个选项，将在独立模式运行。
     -m,--jobmanager <host:port>          连接JobManager (master) 的地址。可以指定
                                          'yarn-cluster' 作为JobManager向YARN集群提
                                          交job. 使用这个选项用来连接不同的JobManager
                                          以替换配置中指定的JobManager。
     -p,--parallelism <parallelism>       程序运行的并行度. 这个选项可以替换配置中指
                                          定的并行度。
     -q,--sysoutLogging                   加上这个选项则不会向标准输出输出日志。
     -s,--fromSavepoint <savepointPath>   用来恢复一个savepoint(示例：
                                          file:///flink/savepoint-1537).
  如果设置了 -m yarn-cluster，可以额外使用以下参数：
     -yD <arg>                            动态配置
     -yd,--yarndetached                   以独立（detached）模式运行
     -yj,--yarnjar <arg>                  Flink jar文件的路径
     -yjm,--yarnjobManagerMemory <arg>    JobManager 容器的内存大小 [单位 MB]
     -yn,--yarncontainer <arg>            YARN 容器分配的数量
                                          (=Task Managers的数量)
     -ynm,--yarnname <arg>                在YARN中设置自定义的应用程序名
     -yq,--yarnquery                      列出可用的YARN资源
                                          (内存, 核数)
     -yqu,--yarnqueue <arg>               设置YARN的列队.
     -ys,--yarnslots <arg>                每个TaskManager的slot数量
     -yst,--yarnstreaming                 以流模式启动 Flink
     -yt,--yarnship <arg>                 用来传输文件的目录（t 表示 transfer）
     -ytm,--yarntaskManagerMemory <arg>   TaskManager 容器的内存大小 [单位 MB]


指令 "info" 用来显示优化后的程序执行计划 (JSON).

  语法: info [OPTIONS] <jar-file> <arguments>
  "info" action 选项:
     -c,--class <classname>           程序的入口类("main" 方法或 "getPlan()" 方法。）
                                      只有当 JAR 文件没有在 mainifest 中指定入口类时才需要。
     -m,--jobmanager <host:port>      连接JobManager (master) 的地址。可以指定
                                      'yarn-cluster' 作为JobManager向YARN集群
                                      提交job. 使用这个选项用来连接不同的JobManager
                                      以替换配置中指定的JobManager。
     -p,--parallelism <parallelism>   程序运行的并行度. 这个选项可以替换配置中指
                                      定的并行度。


指令 "list" 列举出运行中或者是已经调度的程序.

  语法: list [OPTIONS]
  "list" action 选项:
     -m,--jobmanager <host:port>   连接JobManager (master) 的地址。可以指定
                                   'yarn-cluster' 作为JobManager向YARN集群
                                   提交job. 使用这个选项用来连接不同的JobManager
                                   以替换配置中指定的JobManager。
     -r,--running                  只显示运行中的程序和他们的JobID。
     -s,--scheduled                只显示已调度过的程序和他们的JobID。


指令 "cancel" 取消一个运行中的程序.

  语法: cancel [OPTIONS] <Job ID>
  "cancel" action 选项:
     -m,--jobmanager <host:port>   连接JobManager (master) 的地址。可以指定
                                   'yarn-cluster' 作为JobManager向YARN集群
                                   提交job. 使用这个选项用来连接不同的JobManager
                                   以替换配置中指定的JobManager。


指令 "stop" 停止一个运行中的程序 (只能用于streaming jobs). 对于停止一个任务，并没有强一致性的保证。

  语法: stop [OPTIONS] <Job ID>
  "stop" action 选项:
     -m,--jobmanager <host:port>   连接JobManager (master) 的地址。可以指定
                                   'yarn-cluster' 作为JobManager向YARN集群
                                   提交job. 使用这个选项用来连接不同的JobManager
                                   以替换配置中指定的JobManager。


指令 "savepoint" 对一个运行中的job触发savepoint或是销毁一个savepoint。

  语法: savepoint [OPTIONS] <Job ID>
  "savepoint" action 选项:
     -d,--dispose <savepointPath>   销毁一个现存的 savepoint。
     -m,--jobmanager <host:port>    连接JobManager (master) 的地址。可以指定
                                    'yarn-cluster' 作为JobManager向YARN集群
                                    提交job. 使用这个选项用来连接不同的JobManager
                                    以替换配置中指定的JobManager。
~~~
