---
title: 'Mapreduce: Start from Word Count'
date: 2016-01-17 15:11:08
tags: [mapreduce,introduction]
---
As a fan of big data, the most basic concept of big data is hadoop.
What is hadoop?
Hadoop is a open-souce software framework. containing storage part: Hadoop Distributed File System (HDFS) and a processing part called MapReduce.
<!-- more -->
# Hadoop Distributed File System (HDFS)
HDFS splits files into large blocks and distribute them across nodes. HDFS will usually have three copies of a data block, if any one of them failed, the rest of them can make a replication quickly.

The components of HDFS, mainly includes one name node and multiple data node.
{% asset_img Fig1-StoragesOnDataNode.png [HDFS Storage Architecture] %}

You can see that, there is only one name node, which acts like an interface to the client/applications. It stores the meta data of the entire file system of HDFS. If the name node failed, the whole HDFS will disappear. So if you have limited money for HDFS, please set the namenode on your best machine.

Question: Why there are 3 replications instead of 1, 2, or 5?
Answer:
1. Using 1 block provides no redundancy, the data will be lost completely if that disk failed.
2. Using 2 blocks may cause a tie, if data from one disk is corrupted, we have no way to know which one is the right one.
3. Using 3 blocks is just right, if data from one disk is corrupted, we can use the majority vote to determine which of the disk is failing. It is very unlikely that two of the disk fail at the same time.
4. Using 5 blocks will not generate a tie, but it takes much longer time to update all copies for a write operation. And it will takes much more space than 3 replications.

Question: Do we still need Redundant Array of Independent Disks(RAID) for HDFS?
Answer: RAID is a very low level mechanism which ensures the redundancy of the hard disk.  We don't need RAID here because HDFS already provides redundancy itself. Add RAID to it is just double redundant. But there is one exception: the Name Node. We can not afford to loose the name node, because if the name node fails, we will loose the entire system. So we might want to add extra redundancy to it. RAID 1 is a good choice because it provides very high reliability.


## MapReduce
### Level of Parallelism
