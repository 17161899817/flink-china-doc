---
title: "��������: ���� K-Means ʵ��"
# Top navigation
top-nav-group: quickstart
top-nav-pos: 2
top-nav-title: ����ʵ��
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

* This will be replaced by the TOC
{:toc}
������Ҫ������flink������ʵ��([K-Means clustering](http://en.wikipedia.org/wiki/K-means_clustering))��Ҫ������һϵ�в��衣��һ���棬���ܹ۲�ʵ�����й����еĿ�
�ӻ����桢�Ż������Լ�����ʵ��ִ�н��ȡ�


## ��װ Flink
��鿴 [���ٰ�װ](setup_quickstart.html) ����װflink��������flink��װ�ĸ�Ŀ¼��


## ������������
Flink Ϊ K-Means ��װ������������

~~~bash
# �������Ѿ�λ��flink�İ�װ��Ŀ¼
mkdir kmeans
cd kmeans
# ��������������
java -cp ../examples/batch/KMeans.jar:../lib/flink-dist-{{ site.version }}.jar \
  org.apache.flink.examples.java.clustering.util.KMeansDataGenerator \
  -points 500 -k 10 -stddev 0.08 -output `pwd`
~~~

��������Ҫ���²�����λ��`[]`�Ĳ����ǿ�ѡ���

~~~bash
-points <num> -k <num clusters> [-output <output-path>] [-stddev <relative stddev>] [-range <centroid range>] [-seed <seed>]
~~~

-stddev ��һ����Ȥ�ĵ�����������������������ɵ����ݵ������ĵĽӽ��̶ȡ�

`kmeans/` Ŀ¼�°��������ļ�: `centers` �� `points`. `points` �����˼�Ⱥ�����е����ݵ㣬 `centers` �����˼�Ⱥ��ʼ������������ġ�


## �������
ʹ�� `plotPoints.py` ���������֮ǰ���������ݡ� [Download Python Script](plotPoints.py)

~~~ bash
python plotPoints.py points ./points input
~~~ 

��ע: �������Ҫ��װ [matplotlib](http://matplotlib.org/) (��Ubuntnϵͳ����`python-matplotlib`) ����Python�ű�.

����Դ� `input-plot.pdf` ���鿴��������, ����Evince��ص����ݾʹ���� (`evince input-plot.pdf`)�ļ���.

�����Ƕ� -stddev ���ò�ֵͬ������������ݵ�ֲ�ͼ��չʾ��

|relative stddev = 0.03|relative stddev = 0.08|relative stddev = 0.15|
|:--------------------:|:--------------------:|:--------------------:|
|<img src="{{ site.baseurl }}/page/img/quickstart-example/kmeans003.png" alt="example1" style="width: 275px;"/>|<img src="{{ site.baseurl }}/page/img/quickstart-example/kmeans008.png" alt="example2" style="width: 275px;"/>|<img src="{{ site.baseurl }}/page/img/quickstart-example/kmeans015.png" alt="example3" style="width: 275px;"/>|


## ���� Flink
���㱾�������� Flink �� web�������ύ�ͻ��ˡ�

~~~ bash
# ���ص� Flink �ĸ�Ŀ¼
cd ..
# ���� Flink
./bin/start-local.sh
~~~

## ���� K-Means ʵ��
Flink web �����������û�����ȥ�ύFlink����

<div class="row" style="padding-top:15px">
	<div class="col-md-6">
		<a data-lightbox="compiler" href="{{ site.baseurl }}/page/img/quickstart-example/jobmanager_kmeans_submit.png" data-lightbox="example-1"><img class="img-responsive" src="{{ site.baseurl }}/page/img/quickstart-example/jobmanager_kmeans_submit.png" /></a>
	</div>
	<div class="col-md-6">
		1. ��web���� <a href="http://localhost:8081">localhost:8081</a> <br>
		2. �ڲ˵���ѡ�� "Submit new Job"  <br>
		3. ͨ�����"Add New"�� ѡ��<code>examples/batch</code> Ŀ¼�µ� <code>KMeans.jar</code>��֮���� "Upload"�� <br>
		4. ��һ��������ѡ�� <code>KMeans.jar</code> <br>
		5. �ڵ��������������������: <br>
		    ά�� <i>Entry Class</i> �� <i>Parallelism</i> ��񲻱�<br>
		    Ϊ��ʵ���������²���: <br>
		    (KMeans ���������������: <code>--points &lt;path&gt; --centroids &lt;path&gt; --output &lt;path&gt; --iterations &lt;n&gt;</code>
			{% highlight bash %}--points /tmp/kmeans/points --centroids /tmp/kmeans/centers --output /tmp/kmeans/result --iterations 10{% endhighlight %}<br>
		6. ��� <b>Submit</b> ����������
	</div>
</div>
<hr>
<div class="row" style="padding-top:15px">
	<div class="col-md-6">
		<a data-lightbox="compiler" href="{{ site.baseurl }}/page/img/quickstart-example/jobmanager_kmeans_execute.png" data-lightbox="example-1"><img class="img-responsive" src="{{ site.baseurl }}/page/img/quickstart-example/jobmanager_kmeans_execute.png" /></a>
	</div>

	<div class="col-md-6">
		�鿴����ִ�й��̡�
	</div>
</div>


## ֹͣ Flink
ֹͣ Flink����.

~~~ bash
# ֹͣ Flink
./bin/stop-local.sh
~~~

## �������
�����ٴ����� [Python Script](plotPoints.py) �ű������ӻ�չʾ���.

~~~bash
cd kmeans
python plotPoints.py result ./result clusters
~~~

��������ͼչʾ�˶�֮ǰ�������ݵ㼯����ͳ�ƵĽ��������Զ�ε���������������������Ⱥ���������鿴��Щ�������Ӱ�����ݵ�ķֲ���


|relative stddev = 0.03|relative stddev = 0.08|relative stddev = 0.15|
|:--------------------:|:--------------------:|:--------------------:|
|<img src="{{ site.baseurl }}/page/img/quickstart-example/result003.png" alt="example1" style="width: 275px;"/>|<img src="{{ site.baseurl }}/page/img/quickstart-example/result008.png" alt="example2" style="width: 275px;"/>|<img src="{{ site.baseurl }}/page/img/quickstart-example/result015.png" alt="example3" style="width: 275px;"/>|

