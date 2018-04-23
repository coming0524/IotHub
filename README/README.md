<!-- begin merge (remove this line to resolve the conflict) -->
~ Begin Remote
# IotHub
Use ELK to Built a IotHub
~ End Remote
# 构建IoTHub及智能应用

## 项目背景
自从2016年AlphaGo击败人类围棋选手，将时代带入了人工智能时代。无论机器机器学习还是深度学习，都依赖于数据。所以我们不得不回顾下这个10年在IT业中重要的名词。

云计算（Cloud）：对云计算的定义有多种说法。现阶段广为接受的是美国国家标准与技术研究院定义：云计算是一种按使用量付费的模式，这种模式提供可用的、便捷的、按需的网络访问， 进入可配置的计算资源共享池（资源包括网络，服务器，存储，应用软件，服务），这些资源能够被快速提供，只需投入很少的管理工作，或与服务供应商进行很少的交互。 

大数据（BigData）：对于大数据的定义业界通常用4个V来解释 (即Volume、Variety、Value、Velocity)，数据体量大，数据类型多，价值密度低，数据产生快。随着设备日志数据的剧增，日志数据本身也渐渐的往大数据的形态靠拢。

物联网（IoT）：从最初在1999年提出：即通过射频识别（RFID）到现在物联网（Internet of Things）,把所有物品通过信息传感设备与互联网连接起来，进行信息交换，即物物相息，以实现智能化识别和管理。

人工智能（AI）：意图了解智能的实质，并生产出一种新的能以人类智能相似的方式做出反应的智能机器，该领域的研究包括机器人、语言识别、图像识别、自然语言处理和专家系统等。


## 项目内容
会使用Elastic公司的三款开源工具来进行IoTHub的构建。Elastic提供Logstash，Elasticsearch，Kibana这3款开源工具。其中Logstash为数据收集，转换，传输的工具，Elasticsearch为数据存储，搜索，分析的工具，Kibana为数据可视化展示的工具。

![001]

[001]: images/001.JPG "001" { width:auto; max-width:90% }

### 系统架构
Elasticsearch是分布式数据库，所以建立多台服务器做ES集群。Logstash主要起到传输作用，构建并行服务器。Kibana主要用于显示，构建单台。智能分析通过Python来进行。传感器部分通过Raspberry Pi 3等

![002]

[002]: images/002.JPG "002" { width:auto; max-width:90% }

### 硬件准备清单
为了简便，实验环境在Windows10中进行，真实环境可以考虑Windows Server 2016等服务器版本。   
物理机：   
CPU:I7 4770K,内存：32GB，硬盘：2块SSD 256GB，显卡：nvidia 1070 8G   
虚拟机：   
Server1：Windows 2012R2.（ElasticSearch,LogStash,Kibana）,IP:192.168.1.201   
Server2: Windows 2012R2.（ElasticSearch,LogStash）,IP:192.168.1.202   
Server3: Windows 2012R2.（ElasticSearch）,IP:192.168.1.203

### 软件准备清单
Windows环境下安装ELK需要准备软件
Java8安装包（不要安装Java10！,不要安装Java10！，不要安装Java10！目前Logstash还不支持Java10.重要的要说3边）
ElasticSearch安装包，LogStash安装包，Kibana安装包，nssm安装包。
另外还有node安装包与head插件（之后会说明）

### 第一步JAVA安装JAVA安装
Java8下载地址，请下载Windows x64版，目前为（jdk-8u171-windows-x64.exe）   
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html   
按默认安装。分别在虚拟Server1到3中安装。

### 第二部ElasticSearch安装
1.安装ELK首先需要到   
https://www.elastic.co/cn/products   
下载软件。目前为6.2.4版本建议使用MSI版本软件包

2.首先在Server1中安装第一台ElasticSearch，双击MSI软件包安装。   
![004]

[004]: images/004.JPG "004" { width:auto; max-width:90% }

![005]

[005]: images/005.JPG "005" { width:auto; max-width:90% }

![006]

[006]: images/006.JPG "006" { width:auto; max-width:90% }

之后向导中不用选插件。下一步进行安装。   

3.修改ES配置文件
修改以下配置文件   
C:\IotProject\elasticsearch\Node\config\elasticsearch.yml   

``` javascript
bootstrap.memory_lock: false
cluster.name: IotProject
network.host: 192.168.1.201 //绑定本地IP，默认尾127.0.0.1
http.port: 9200
http.cors.enabled: true  //head插件用
http.cors.allow-origin: "*" //head插件用
node.data: true
node.ingest: true
node.master: true
node.max_local_storage_nodes: 1
node.name: ELKIOTSERVER01
path.data: C:\IotProject\elasticsearch\Node\data
path.logs: C:\IotProject\elasticsearch\Node\logs
transport.tcp.port: 9300
discovery.zen.ping.unicast.hosts: ["192.168.1.201","192.168.1.202","192.168.1.203"] //集群节点所有IP地址
discovery.zen.minimum_master_nodes: 2 //宣告master节点数，一般是节点数/2+1
```

细节可以参考以下URL
https://blog.csdn.net/zxf_668899/article/details/54582849   

4.Head插件安装（监控ElasticSearch状态）   
参考以下URL安装   
https://blog.csdn.net/u012270682/article/details/72934270   
elasticsearch 5以上版本安装head需要安装node和grunt   
下载地址：https://nodejs.org/en/download/    根据自己系统下载相应的msi，双击安装。   

确认node版本，并安装grunt，用npm install -g grunt -cli命令 
![007]

[007]: images/007.JPG "007" { width:auto; max-width:90% }
确认grunt版本，用grunt -version命令
![008]

[008]: images/008.JPG "008" { width:auto; max-width:90% }

完成后，修改elasticsearch.yml配置文件   
http.cors.enabled: true  //head插件用   
http.cors.allow-origin: "*" //head插件用   
重启服务器。并到https://github.com/mobz/elasticsearch-head 下载zip文件，解压缩到文件夹   
进入解压缩文件夹，修改Gruntfile.js文件   
![011]

[011]: images/011.JPG "011" { width:auto; max-width:90% }
cmd进入E:\elasticsearch-5.4.1\elasticsearch-head-master文件夹   
执行 npm install
![009]

[009]: images/009.JPG "009" { width:auto; max-width:90% }
安装完成查看结果192.168.1.201:9100确认结果。目前没数据的话，如图
![010]

[010]: images/010.JPG "010" { width:auto; max-width:90% }

6.按上述2-3内容，在server2，与server3中安装。其他ES集群节点
其他节点node.data: true，node.ingest: true，node.master: true。
但是head插件用配置不用追加。

















<!-- end merge -->
