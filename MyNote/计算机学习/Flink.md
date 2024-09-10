---
created: 2024-08-20
modified: 2024-08-20
tags:
  - Flink
---

# Flink是什么

**Flink** 是 ***”数据流上的有状态计算“*** ，Apache Flink是一个 ***框架*** 和 ***分布式处理引擎*** ，用于对 ***无界*** 和 ***有界*** 数据流进行有状态计算。

![[Pasted image 20240830090512.png]]
#### 无界数据流：
1. 有定义流的开始，但没有定义流的结束；
2. 他们会无休止的产生数据；
3. 无界流的数据必须持续处理，即数据被摄取后需要立刻处理。我们不能等到所有数据都到达再处理，因为输入是无限的。
   
#### 有界数据流
1. 有定义流的开始，也有定义流的结束；
2. 有界流可以在摄取所有数据后再进行计算；
3. 有界流所有数据可以被排序，所以并不需要有序摄取；
4. 有界流处理通常被成为批处理。

#### 有状态流处理
把流处理需要的 ***额外数据保存成一个”状态”***，然后针对这条数据进行处理，并且 ***“更新状态”***。这就是所谓的 ***“有状态的流处理”***。

- 状态在内存中：优点，速度快；缺点：可靠性差。
- 状态在分布式系统中：有点。可靠性高；缺点，速度慢。

#### Flink特点
处理数据的目标是：***低延迟、高吞吐、结果的准确性和良好的容错性*** 。
- ***高吞吐和低延迟*** ：每秒处理数百万个事件，毫秒级延迟。
- ***结果的准确性*** ：Flink提供了 ***事件时间(event time)*** 和 ***处理时间(processing time)*** 语义。对于乱序事件流，时间时间语义仍然能提供一致且准确的结果。
- ***精确一次（exactly once）*** 的状态一致性保证。
- ***可以连接到最常用的外部系统*** ：如Kafka、Hive、JDBC、HDFS、Redis等。
- ***高可用*** ：本身高可用的设置，加上K8s，YARN和Mesos的紧密集成，在加上从故障中快速恢复和动态扩展任务的能力，Flink能做到以极少的停机时间7 X 24全天候运行。

#### Flink分层API
![[Pasted image 20240830092338.png]]

# Flink中的流类型以及相互转化

- ***DataStream：*** 
  Flink流处理api中最核心的数据结构。它代表一个运行在多个分区上的并行流。可以从 <font color="red"><b>StreamExecutionEnvironment</b></font> 通过 <font color="red"><b>env.addSource(SourceFunction)</b></font> 获得。
  DateStream上的转换操作都是逐条的，比如map(),flatmap(),filter()。DataStream也可以执行<font color="red"><b>rebalance</b></font> （再平衡，用于减轻数据倾斜）和 <font color="red"><b> broadcaseted </b></font> （广播）等分区转换。
  
- ***KeyedStream：***
  ***KeyedStream***用来表示根据指定的key进行分组的数据流。一个KeyedStream可以通过调用***DataStream.keyby()*** 来获得。而在KeyedStream上进行任何的transformation都将转变回DataStream。在实现中KeyedStream是把key的信息写入到了transformation中。每条记录只能访问所属key的状态，其上的聚合函数可以方便地操作和保存对应key的状态。
  
- ***WindowedStream & AllWindowedStream***
  ***WindowedStream***代表了根据key分组，并且基于***WindowAssigner***切分窗口的数据流。所以***WindowedStream***都是从***KeyedStream***衍生而来的。而在***WindowedStream***上进行任何操作也都将转变回***DataStream***。
  Flink的窗口实现中会将到达的数据缓存在对应的窗口buffer中（一条数据可能对应多个窗口）。当到达窗口发送的条件时（由Trigger控制），Flink会对整个窗口中的数据进行处理。Flink在聚合类窗口有一定的优化，即不会保存窗口中的所有值，而是每到一个元素执行一次聚合函数，最终只保存一份数据即可。
  在key分组的流上进行窗口切分是比较常用的场景，也能够很好地并行化（不同的key上的窗口聚合可以分配到不同的task去处理）。不过有时候我们也需要在普通流上进行窗口的操作，这就是 ***AllWindowedStream***。***AllWindowedStream****是直接在DataStream上进行***windowAll(...)*** 操作。***AllWindowedStream*** 的实现是基于 ***WindowedStream*** 的（Flink 1.1.x 开始）。Flink 不推荐使用***AllWindowedStream***，因为在普通流上进行窗口操作，就势必需要将所有分区的流都汇集到单个的Task中，而这个单个的Task很显然就会成为整个Job的瓶颈

- ***JoinedStreams & CoGroupedStreams***
  

# Flink部署

## Flink集群角色

#Flink Flink提交作业和执行任务，需要几个关键组件：
- <font color="red"><b> 客户端(Client) </b></font> ：代码由客户端 ***获取*** 并做转换，之后提交给JobManager
- <font color="red"><b> JobManager </b></font> ：对作业进行中央调度管理；而它获取到要执行的作业后，会进一步处理转换，然后分发任务给众多的Task Manager。
- <font color="red"><b> TaskManager </b></font>：实际工作角色

## Flink集群搭建

- 进入conf路径，修改flink-conf.yaml文件，指定JobManager节点
```
# JobManager节点地址
jobmanager.rpc.address: hadoop102
jobmanager.bind-host: 0.0.0.0
rest.address: hadoop102
rest.bind-address: 0.0.0.0

# TaskManager节点地址.需要配置为当前机器名
taskmanager.bind-host: 0.0.0.0
taskmanager.host: hadoop102
```
- 修改workers文件，指定TaskManager节点主机名
- 修改masters文件，指定master节点以及端口号

#### Flink常用命令
```bash
# Flink集群客户端启动命令
bin/start-cluster.sh

# 使用命令行提交flink任务
bin/flink run -m linux1:port -c 主类名 jar包路径


```


# Flink运行时架构

![[Pasted image 20240903163156.png]]

## 基于时间的合流-双流连结（Join）

在interval-join时，只要数据的**事务时间**小于当前的**watermark**，则该条数据为迟到数据，不对其进行处理，可以在测输出流中将其输出。

# 处理函数

# 8. 状态

## 8.1 什么是状态

Flink中算子可以分为 ***有状态*** 和 ***无状态*** 两种。基本转换算子计算时不依赖其他数据，属于无状态算子；而有状态的算子，除了当前数据之外，还需要一些其他数据来得到计算结果，这里的其他数据，就是 ***状态*** 。

有状态算子的一般处理流程，具体步骤如下：

1. 算子任务接收到上游发来的数据；
2. 获取当前状态；
3. 根据业务逻辑进行计算，更新状态；
4. 得到计算结果，输出发送到下游任务

## 8.2 状态的分类

#### 1. 托管状态（Managed State）和原始状态（Raw State）

## 8.4 状态后端（HashMapStateBackend/RocksDB）

(1)  

(2)




# 容错机制

## 检查点（Checkpoint）

[Flink中Barrier对齐机制_flink barrier 对齐-CSDN博客](https://blog.csdn.net/qq_42009405/article/details/122850469)

Barrier对齐的精准一次
Barrier不对齐的至少一次
Barrier不对齐的至少一次


## 状态一致性

