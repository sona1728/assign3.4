#IMPORTANCE OF NAMENODE IN HADOOP CLUSTER :
*NAMENODE HIGH AVAILABILITY: FEATURE OVERVIEW

- AUTOMATED FAILOVER: HDP pro-actively detects NameNode host and process failures and will automatically switch to the standby NameNode to maintain availability for the HDFS service. There is no need for human intervention in the process – System Administrators can sleep in peace!

- STANDBY: Both Active and Standby NameNodes have up to date HDFS metadata, ensuring seamless failover even for large clusters – which means no downtime for your HDP cluster!

- FULL STACK RESILIENCY: The entire HDP stack (MapReduce, Hive, Pig, HBase, Oozie) has been certified to handle a NameNode failure scenario without losing data or the job progress. This is vital to ensure long running jobs that are critical to complete on schedule will not be adversely affected during a NameNode failure scenario.

#With HDP 2.0, this entire functionality is built into the HDP product. This means:

- No need for shared storage
- No need for external HA frameworks
- Certified with a secure Kerberos configuration

- In each cluster, two separate machines are configured as NameNodes. In a working cluster, one of the NameNode machine is in the Active state, and the other is in the Standby state.

- The Active NameNode is responsible for all client operations in the cluster. The Standby NameNode maintains enough state to provide a fast failover. In order for the Standby node to keep its state synchronized with the Active node, both nodes communicate through a group of separate daemons called JournalNodes. The file system journal logged by the Active NameNode at the JournalNodes is consumed by the Standby NameNode to keep it’s file system namespace in sync with the Active.

- In order to provide a fast failover, it is also necessary that the Standby node have up-to-date information of the location of blocks in your cluster. DataNodes are configured with the location of both the NameNodes and send block location information and heartbeats to both NameNode machines.

- The ZooKeeper Failover Controller (ZKFC) is responsible for HA Monitoring of the NameNode service and for automatic failover when the Active NameNode is unavailable. There are two ZKFC processes – one on each NameNode machine. ZKFC uses the Zookeeper Service for coordination in determining which is the Active NameNode and in determining when to failover to the Standby NameNode.

- Quorum journal manager (QJM) in the NameNode writes file system journal logs to the journal nodes. A journal log is considered successfully written only when it is written to majority of the journal nodes. Only one of the Namenodes can achieve this quorum write. In the event of split-brain scenario this ensure that the file system metadata will not be corrupted by two active NameNodes.

- In HA setup, HDFS clients are configured with a logical name service URI and the two NameNodes corresponding to it. The clients perform source side failover. When a client cannot connect to a NameNode or if the NameNode is in standby mode, it performs fail over to the other NameNode.
