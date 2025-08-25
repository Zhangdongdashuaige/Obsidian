---
created: 2025-08-25
---
# 什么是Hive
## Hive简介
Hive是由Facebook开源，基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张表，并提供类SQL查询功能。
## Hive本质
Hive是一个Hadoop客户端，用于将<span class="yellow-bold">HQL转化成MapReduce程序。</span>
1. Hive中每张表的数据存储在HDFS
2. Hive分析数据底层的实现是MapReduce（也可配置为Spark或者Tez）
3. 执行程序运行在Yarn上
## Hive架构
  ![[Hive架构.png]]

# Hive语法
 