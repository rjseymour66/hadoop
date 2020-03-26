# Introduction

Hadoop provides storage and computational capabilities for big data. It uses a distributed master-slave architecture that uses the following components described in this section.

## HDFS
The Hadoop Distributed File System is for data storage. It is modeled on the Google File System (GFS). The master is responsible for partitioning the storage across the slave nodes and keeping track of where data is located. 

HDFS works best when reading and writing large files (gigabytes and larger), and does so by using unusually large (for a filesystem) block sizes and data locality optimizations to reduce network I/O.

### NameNode
The NameNode keeps metadata about the filesystem in memory. For example, which DataNodes manage the blocks for each file. 

#### Federation
Allows HDFS metadata to be shared across multiple NameNode hosts, which improves scalability and provides data isolation. This allows different applications or teams to run their own NameNodes without fear of impacting other NameNodes on the same cluster.

### DataNode
Communicate with each other for pipelining file reads and writes. 

#### High Availability (HA)
Removes the single point of failure wherein a NameNode disaster would result in a cluster outage. HDFS HA also offers the ability for failover to be automated. This is the process where a standby NameNode takes over work from a failed primary NameNode.

### Scalability and availability
HDFS replicates files for a configured number of times, is tolerant of both hardware and software failure, and automatically re-replicates data blocks on nodes that have failed. 

## YARN
A general-purpose scheduler and resource manager. Any YARN application can run on a Hadoop cluster. The master performs the actual scheduling of work for YARN applications. 

## MapReduce
A batch-based computational engine. In Hadoop 2, MapReduce is implemented as a YARN application. The master is responsible for organizing where computational work should be scheduled on the slave nodes.  

 