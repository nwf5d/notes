Hadoop总结
---
## hadoop是什么？

## 什么是Hive？
## 基本概念
## 基础原理
### [HDFS写入和读取流程](http://blog.csdn.net/hguisu/article/details/7259716)
### MapReduce任务的执行流程
[参考](http://blog.csdn.net/hguisu/article/details/7265384)
[Map-Reduce的过程解析](http://blog.csdn.net/wisgood/article/details/13624251)，可看最后的图总结

### [Hadoop的NameNode单点问题？](http://www.aboutyun.com/thread-6799-1-1.html)
正如大家所知，NameNode在Hadoop系统中存在单点故障问题，这个对于标榜高可用性的Hadoop来说一直是个软肋。本文讨论一下为了解决这个问题而存在的几个solution。

1. Secondary NameNode
原理：Secondary NN会定期的从NN中读取editlog，与自己存储的Image进行合并形成新的metadata image
优点：Hadoop较早的版本都自带，配置简单，基本不需要额外资源（可以与datanode共享机器）
缺点：恢复时间慢，会有部分数据丢失

2. Backup NameNode
原理：backup NN实时得到editlog，当NN宕掉后，手动切换到Backup NN；
优点：从hadoop0.21开始提供这种方案，不会有数据的丢失
缺点：因为需要从DataNode中得到Block的location信息，在切换到Backup NN的时候比较慢（依赖于数据量）

3. Avatar NameNode
原理：这是Facebook提供的一种HA方案，将client访问hadoop的editlog放在NFS中，Standby NN能够实时拿到editlog；DataNode需要同时与Active NN和Standby NN report block信息；
优点：信息不会丢失，恢复快（秒级）
缺点：Facebook基于Hadoop0.2开发的，部署起来稍微麻烦；需要额外的机器资源，NFS成为又一个单点（不过故障率低）

4. Hadoop2.0直接支持StandBy NN，借鉴Facebook的Avatar，然后做了点改进
优点：信息不会丢失，恢复快（秒级），部署简单

### Map数和Redce数是如何确定的？如何设定？
Map数是由Hadoop系统计算得到的，不能显式设置；主要考虑HDFS块大小、文件大小、文件个数和splitsize大小。因此可以调节系统的参数达到调节Map数的目的
Reduce数默认是一个，Reduce数与最终输出的文件个数是一样的，每个Reduce对应一个输出文件。可在程序中设置Reduce数。

在map阶段读取数据前，FileInputFormat会将输入文件分割成split。split的个数决定了map的个数。
影响map个数，即split个数的因素主要有：
1）HDFS块的大小，即HDFS中dfs.block.size的值。如果有一个输入文件为1024m，当块为256m时，会被划分为4个split；当块为128m时，会被划分为8个split。
2）文件的大小。当块为128m时，如果输入文件为128m，会被划分为1个split；当块为256m，会被划分为2个split。
3）文件的个数。FileInputFormat按照文件分割split，并且只会分割大文件，即那些大小超过HDFS块的大小的文件。如果HDFS中dfs.block.size设置为64m，而输入的目录中文件有100个，则划分后的split个数至少为100个。
4）splitsize的大小。分片是按照splitszie的大小进行分割的，一个split的大小在没有设置的情况下，默认等于hdfs block的大小。但应用程序可以通过两个参数来对splitsize进行调节。

## 性能优化
### 数据倾斜问题？
### 大量小文件问题？
[参考](http://blog.csdn.net/wisgood/article/details/17081367)
### [提高MapReduce性能的七点建议](http://blog.csdn.net/wisgood/article/details/19411057)
## 经典问题
### 二次排序问题
[参考1](http://blog.csdn.net/cnweike/article/details/6954364)
[参考2](http://blog.csdn.net/heyutao007/article/details/5890103)
### [MapReduce中的两表join几种方案简介](http://blog.csdn.net/wisgood/article/details/17230281)
了解前两个就差不多了
### TOP-N问题？


