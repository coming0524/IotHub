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

### 服务器
为了简便，实验环境在Windows10中进行，真实环境可以考虑Windows Server 2016等服务器版本。

物理机：

CPU:I7 4770K,内存：32GB，硬盘：2块SSD 256GB，显卡：nvidia 1070 8G

虚拟机：

Server1：Windows 2012R2.（ElasticSearch,LogStash,Kibana）,IP:192.168.1.201

Server2: Windows 2012R2.（ElasticSearch,LogStash）,IP:192.168.1.202

Server3: Windows 2012R2.（ElasticSearch）,IP:192.168.1.203

### 软件安装与设定
Windows环境下安装ELK需要准备软件
Java8安装包（不要安装Java10！,不要安装Java10！，不要安装Java10！目前Logstash还不支持Java10.重要的要说3边）
ElasticSearch安装包，LogStash安装包，Kibana安装包，nssm


#### JAVA安装




#### ElasticSearch安装
安装ELK首先需要
到https://www.elastic.co/cn/products 下载软件。目前为6.2.4版本



