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

YARN is the scheduling function from MapReduce that was extracted and turned into a generic application scheduler. Now, Hadoop cluseters are no longer limited to running MapReduce workloads.

### Core components:

#### ResourceManager
The ResourceManager is the YARN master process and is resonsible for scheduling and managing resources callled 'containers'.

#### NodeManager
The NodeManager is the slave YARN process that runs on each node. It is responsible for launching and managing containers. 

### Other components

#### Client
A YARN client is responsible for creating the YARN application.

#### ApplicationMaster
The ApplicationMaster is created by the ResourceManager and is responsible for requesting containers to perform application-specific work. 

#### Container
Containers are YARN application-specific processes that perform some function pertinent to the application.

## MapReduce
A batch-based computational engine. In Hadoop 2, MapReduce is implemented as a YARN application. The master is responsible for organizing where computational work should be scheduled on the slave nodes.  

It allows you to parallelize work over a large amount of raw data, such as combining web logs with relational data from an OLTP database to model how users interact with your website. It abstracts away the complexities involved in working with distributed system, such as computational parallelization, work distribution, and dealing with unreliable hardware and software.

(1) map(key1, value2) -------> (2) list(key2, value2)

**NOTE**: The programmer defines the map and reduce functions where the map function outputs key/value tuples, which are then processed by reduce functions to produce the final output.

1. The map function takes as input a key/value pair, which represents a logical record from the input data source. In the case of a file, this could be a line, or if the input source is a table in a database, it could be a row.
2. The map function produces zero or more output key/value pairs for one input pair. For example, if the map function is a filtering map function, it may only produce output if a certain condition is met. Or it could be performing a demultiplexing operation, where a single key/value yields multiple key/value output pairs. 

MapReduce uses a shared-nothing model (where each node is independent and self-sufficient) to remove any parallel execution interdependenices that could add unwanted synchronization points or state sharing.

### Shuffle and sort phases
This occurs between the map output and the reduce input phases.  
The shuffle and sort phases are responsible for two primary activities: 
1. Determining the reducer that should receive the map output key/value pair (called partitioning)
2. Ensuring that all the input keys for a given reducer are sorted. 
Map outputs for the same key go to the same reducer and are then combined to form a single input record for the reducer.

#### Example 

(1) reduce(key2, (2) list(value2's)) --------> (3) list(key3, value3)

1. The reduce function is called once per unique map output key.
2. All of the map output values that were emitted across all the mappers for 'key2' are provided in a list.
3. Like the map function, the reduce can output zero-to-many key/value pairs. Reducer output can write to flat files in HDFS, insert/update rows in a NoSQL database, or wirte to any data sink, depending on the requirements of the job.