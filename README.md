# Introduction

Hadoop provides storage and computational capabilities for big data. It uses a distributed master-slave architecture that uses the following components:

**YARN master**<br>A general-purpose scheduler and resource manager. Any YARN application can run on a Hadoop cluster. The master performs the actual scheduling of work for YARN applications. 

**MapReduce master**<br>A batch-based computational engine. In Hadoop 2, MapReduce is implemented as a YARN application. The master is responsible for organizing where computational work should be scheduled on the slave nodes.  

**HDFS master**<br>The Hadoop Distributed File System is for data storage. The master is responsible for partitioning the storage across the slave nodes and keeping track of where data is located.  