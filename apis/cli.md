---
title:  "�����нӿڣ�CLI��"
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

Flink�ṩ��һ�������нӿڣ�CLI���������д��JAR���ĳ��򣬲��ҿ��Կ��Ƴ����ִ�С�
�����нӿڿ������ڱ��ص��ڵ���Ƿֲ�ʽ�Ĳ���װ������Flink��װ���ߵ�һ�������
�������λ�� `<flink-home>/bin/flink`��
Ĭ�ϻ����������е� Flink master��JobManager����
master �������ű��� CLI ��ͬһ��װĿ¼�¡�

���ʹ�������нӿڣ�CLI�����Ⱦ�����������һ��master (JobManager)
 (ͨ����� 
`<flink-home>/bin/start-local.sh` ��
`<flink-home>/bin/start-cluster.sh`) ������ Flink YARN �����С�

�����п��Ա�����

- �ύһ��job
- ȡ���������е�job
- �ṩһ��job����Ϣ
- �г��������еĺ͵ȴ���job�б�

* This will be replaced by the TOC
{:toc}

## ʾ��

-   ����ʾ�����򣬲���������

        ./bin/flink run ./examples/batch/WordCount.jar

-   ����ʾ�����򣬴���������ļ�������

        ./bin/flink run ./examples/batch/WordCount.jar \
                               file:///home/user/hamlet.txt file:///home/user/wordcount_out

-   ����ʾ����������16�������ȣ����Ҵ���������ļ�������

        ./bin/flink run -p 16 ./examples/batch/WordCount.jar \
                                file:///home/user/hamlet.txt file:///home/user/wordcount_out

-   ����ʾ�����򣬲���ֹ Flink �ڱ�׼��������־

            ./bin/flink run -q ./examples/batch/WordCount.jar

-   �Զ�����detached��ģʽ����ʾ������

            ./bin/flink run -d ./examples/batch/WordCount.jar

-   ����ʾ������ָ�� JobManager��

        ./bin/flink run -m myJMHost:6123 \
                               ./examples/batch/WordCount.jar \
                               file:///home/user/hamlet.txt file:///home/user/wordcount_out

-   ����ʾ������ָ������������ class��

        ./bin/flink run -c org.apache.flink.examples.java.wordcount.WordCount \
                               ./examples/batch/WordCount.jar \
                               file:///home/user/hamlet.txt file:///home/user/wordcount_out

-   ����ʾ������ʹ�� [per-job YARN ��Ⱥ]({{site.baseurl}}/setup/yarn_setup.html#run-a-single-flink-job-on-hadoop-yarn) ���� 2 ��TaskManager��

        ./bin/flink run -m yarn-cluster -yn 2 \
                               ./examples/batch/WordCount.jar \
                               hdfs:///user/hamlet.txt hdfs:///user/wordcount_out

-   ����WordCountʾ��������JSON�ĸ�ʽ����Ż����ִ�мƻ���

        ./bin/flink info ./examples/batch/WordCount.jar \
                                file:///home/user/hamlet.txt file:///home/user/wordcount_out

-   �г��Ѿ����ȵĺ��������е�job������JobID��Ϣ����

        ./bin/flink list

-   �г��Ѿ����ȵ�job������JobID��Ϣ����

        ./bin/flink list -s

-   �г��������е�job������JobID��Ϣ����

        ./bin/flink list -r

-   ȡ��һ��job��

        ./bin/flink cancel <jobID>

-   ֹͣһ��job(ֻ��������ʽ�����job)��

        ./bin/flink stop <jobID>

### Savepoints

[Savepoints]({{site.baseurl}}/apis/streaming/savepoints.html) ͨ�������пͻ��������ơ�

#### savepoint��������

{% highlight bash %}
./bin/flink savepoint <jobID>
{% endhighlight %}

����һ���Ѿ�������savepoint��·��������Ҫͨ�����·�����ָ�������savepoints��

#### **�ָ�һ��savepoint**:

{% highlight bash %}
./bin/flink run -s <savepointPath> ...
{% endhighlight %}

��� run �����ύ job ʱ����һ�� savepoint ��ǣ���ʹ�ó�����Դ� savepoint �����״̬�лָ���
savepoint·����ͨ��savepoint��������õ��ġ�

#### **����һ��savepoint**:

{% highlight bash %}
./bin/flink savepoint -d <savepointPath>
{% endhighlight %}

����һ��savepointͬ����Ҫһ��·����
���savepoint·����ͨ��savepoint��������õ��ġ�

## �÷�

�����е��﷨����:

~~~
./flink <ACTION> [OPTIONS] [ARGUMENTS]

�����о�һЩ���õ�actions:

ָ�� "run" ���벢������һ������.

  �﷨: run [OPTIONS] <jar-file> <arguments>
  "run" action ѡ��:
     -c,--class <classname>               ����������("main" ������ "getPlan()" ��������
                                          ֻ�е� JAR �ļ�û���� mainifest ��ָ�������ʱ����Ҫ��
     -C,--classpath <url>                 �ڼ�Ⱥ�е�ÿ���ڵ���Ϊÿ���û���������������һ�� URL ��Դ��
                                          �»�Ӱ�����нڵ�. ·���������Э����Ϣ
                                           (�� file://) �������н�㶼���Է���
                                           (����NFS�����ļ�)�� ����Զ��ʹ�����ѡ��
                                           ����Ӷ����Դ. Э����뱻{@link
                                          java.net.URLClassLoader}֧�֡�
     -d,--detached                        ����������ѡ����ڶ���ģʽ���С�
     -m,--jobmanager <host:port>          ����JobManager (master) �ĵ�ַ������ָ��
                                          'yarn-cluster' ��ΪJobManager��YARN��Ⱥ��
                                          ��job. ʹ�����ѡ���������Ӳ�ͬ��JobManager
                                          ���滻������ָ����JobManager��
     -p,--parallelism <parallelism>       �������еĲ��ж�. ���ѡ������滻������ָ
                                          ���Ĳ��жȡ�
     -q,--sysoutLogging                   �������ѡ���򲻻����׼��������־��
     -s,--fromSavepoint <savepointPath>   �����ָ�һ��savepoint(ʾ����
                                          file:///flink/savepoint-1537).
  ��������� -m yarn-cluster�����Զ���ʹ�����²�����
     -yD <arg>                            ��̬����
     -yd,--yarndetached                   �Զ�����detached��ģʽ����
     -yj,--yarnjar <arg>                  Flink jar�ļ���·��
     -yjm,--yarnjobManagerMemory <arg>    JobManager �������ڴ��С [��λ MB]
     -yn,--yarncontainer <arg>            YARN �������������
                                          (=Task Managers������)
     -ynm,--yarnname <arg>                ��YARN�������Զ����Ӧ�ó�����
     -yq,--yarnquery                      �г����õ�YARN��Դ
                                          (�ڴ�, ����)
     -yqu,--yarnqueue <arg>               ����YARN���ж�.
     -ys,--yarnslots <arg>                ÿ��TaskManager��slot����
     -yst,--yarnstreaming                 ����ģʽ���� Flink
     -yt,--yarnship <arg>                 ���������ļ���Ŀ¼��t ��ʾ transfer��
     -ytm,--yarntaskManagerMemory <arg>   TaskManager �������ڴ��С [��λ MB]


ָ�� "info" ������ʾ�Ż���ĳ���ִ�мƻ� (JSON).

  �﷨: info [OPTIONS] <jar-file> <arguments>
  "info" action ѡ��:
     -c,--class <classname>           ����������("main" ������ "getPlan()" ��������
                                      ֻ�е� JAR �ļ�û���� mainifest ��ָ�������ʱ����Ҫ��
     -m,--jobmanager <host:port>      ����JobManager (master) �ĵ�ַ������ָ��
                                      'yarn-cluster' ��ΪJobManager��YARN��Ⱥ
                                      �ύjob. ʹ�����ѡ���������Ӳ�ͬ��JobManager
                                      ���滻������ָ����JobManager��
     -p,--parallelism <parallelism>   �������еĲ��ж�. ���ѡ������滻������ָ
                                      ���Ĳ��жȡ�


ָ�� "list" �оٳ������л������Ѿ����ȵĳ���.

  �﷨: list [OPTIONS]
  "list" action ѡ��:
     -m,--jobmanager <host:port>   ����JobManager (master) �ĵ�ַ������ָ��
                                   'yarn-cluster' ��ΪJobManager��YARN��Ⱥ
                                   �ύjob. ʹ�����ѡ���������Ӳ�ͬ��JobManager
                                   ���滻������ָ����JobManager��
     -r,--running                  ֻ��ʾ�����еĳ�������ǵ�JobID��
     -s,--scheduled                ֻ��ʾ�ѵ��ȹ��ĳ�������ǵ�JobID��


ָ�� "cancel" ȡ��һ�������еĳ���.

  �﷨: cancel [OPTIONS] <Job ID>
  "cancel" action ѡ��:
     -m,--jobmanager <host:port>   ����JobManager (master) �ĵ�ַ������ָ��
                                   'yarn-cluster' ��ΪJobManager��YARN��Ⱥ
                                   �ύjob. ʹ�����ѡ���������Ӳ�ͬ��JobManager
                                   ���滻������ָ����JobManager��


ָ�� "stop" ֹͣһ�������еĳ��� (ֻ������streaming jobs). ����ֹͣһ�����񣬲�û��ǿһ���Եı�֤��

  �﷨: stop [OPTIONS] <Job ID>
  "stop" action ѡ��:
     -m,--jobmanager <host:port>   ����JobManager (master) �ĵ�ַ������ָ��
                                   'yarn-cluster' ��ΪJobManager��YARN��Ⱥ
                                   �ύjob. ʹ�����ѡ���������Ӳ�ͬ��JobManager
                                   ���滻������ָ����JobManager��


ָ�� "savepoint" ��һ�������е�job����savepoint��������һ��savepoint��

  �﷨: savepoint [OPTIONS] <Job ID>
  "savepoint" action ѡ��:
     -d,--dispose <savepointPath>   ����һ���ִ�� savepoint��
     -m,--jobmanager <host:port>    ����JobManager (master) �ĵ�ַ������ָ��
                                    'yarn-cluster' ��ΪJobManager��YARN��Ⱥ
                                    �ύjob. ʹ�����ѡ���������Ӳ�ͬ��JobManager
                                    ���滻������ָ����JobManager��
~~~
