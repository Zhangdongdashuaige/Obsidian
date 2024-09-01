---
created: 2024-08-20
modified: 2024-08-20
tags:
  - Flink
---

## 基于时间的合流-双流连结（Join）

在interval-join时，只要数据的**事务时间**小于当前的**watermark**，则该条数据为迟到数据，不对其进行处理，可以在测输出流中将其输出。
a大大大s/


