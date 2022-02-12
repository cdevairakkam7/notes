# Can Amazon Aurora Replace AWS RDS as Production Database?

## Please consider the following factors before making a decision.

### Cost - 
* Amazon tech support says that the cost of Aurora is ~20% more than MySQL[^1] .
* If things are not implemented as per standard operating procedures the cost might increase significantly.
* This means thorough planning and getting the migration correct on the first attempt is very important. 
* Also Aurora's cost is hard to predict[^2].
  * Instance cost is charged per hour.
  * But I/O operations are metered.
  * Auro does not have fixed IOPS limit like MySQL. 
* Auro read replcias will 2X the cost.

### Throughput- 
* Aurora can deliver up to 5X more throughput than MySQL 5.7.1 on the same hardware[^3] 
    * No extra modifications are needed to increase the throughput
    * Note - This only works if the data size is larger 
    * For small-medium sized datasets they both perform equally 

### Closed Source- 
* Amazon is closed source. This means we have to completely rely on Amazon for support
* If you decide to move from AWS to Linode few years down the line, database migration alone will cost you a lot [e.g. compatiblity issues, indexing and re-tuning ].
* We have to depend on AWS for new Aurora releases, bug fixes and patch updates.
  * A new feature request would take a lot of time but 99% of our tasks can be done with existing stable Aurora releases
* MySQL is open source and has been open source for a long time
    * This means migrating to other clouds will be easier
    * Finding a solution for common problems will be straightforward

### Storage 
* Aurora storage automatically grows in 10 GB increments, starting from 10 GB it can automatically increment  up to 64TiB or 128 TiB 
depending on the  DB engine version[^4]
* Performance is not impacted during scaling and distribution  
* If for some reason data is removed form the database Aurora automatically decreases the storage size
    * This feature helps in reduction of cost    
* MySQL 5.7.1 does not autoscale or downsize automatically

### Table Size - 
* In Aurora the size of the table is only constrained by the size of the cluster volume
    * This means a table size can be as big as 64TiB[^5]
* In MySQL 5.7.1 the maximum size of a table can be only 16 TB 

### Replicas - 
* Aurora can provision up to 15 replicas[^6]
    * MySQL 5.7.1 can provision only 5 replicas
* All Aurora instances share the same underlying volume with the primary instance
    * This means replication can be performed in milliseconds
    * If we decide to insert data on the writer instance and read from another instance - Aurora will support this technique
* Incase of primary writer instance failure one of the replicas can be promoted as the primary instance
    * Note - there might be some data loss when this process happens 
* Replicas double the cost


### Writer - 
* Aurora uses shared storage for writer and reader. This means Aurora replicas are synced with writer instance with minimal replication lag[^6]
* Aurora has multi - writer feature. This means if primary DB writer instance is unavailable another writer instance will be immediately 
available[^7]
    * AWS refers to this as continuous availability
* MySQL just has one writer
* Note - Multi - writer can create write conflict.
    * A robust conflict resolution should be put in place

### Crash Recovery -
* Aurora performs crash recovery asynchronously using parallel threads to quickly make the data available after crash
* It takes 30 seconds for Aurora to recover
    * This includes failure detection, DNS propagation and recovery
* MySQL takes 60 - 120 seconds recover

### Backup - 
* Aurora backups are incremental, so it can be quickly restored to a point within the backup retention period.
    * This means there is no performance impact or interruption of database service during backups
* MySQL has daily backup windows
    * There is a slight performance impact when the backup initiates for single availability zone deployments.

### Instance Classes
* MySQL has T,M and R type instances
* Aurora does not have M type instances

### Additional Features - 
* Database cloning - One can quickly create clones of all databases in the cluster. This is faster than restoring Amazon RDS from MySQL 
from a snapshot.
* Cluster Cache Management - When a writer instance fails, we designate a specific replica as the failover target. This replica will update 
the cache of the writer instance
* Serverless - DB cluster starts, scales and shuts down automatically based on our  application needs. This feature is mainly suitable 
for infrequent, intermittent or unpredictable workloads
* Global Database - A single Aurora database can span across multiple regions to enable fast local reads and quick disaster recovery.
    * Global database, uses storage based replication to replicate database across multiple regions with a typical latency of less than 
1 second
* Query Plan Management - With this we control how and when the execution plan changes .
    * Query optimizer is restricted to our plan[^8]

### References 
[^1]:  Amazon tech support [ via AWS management console ]
[^2]:  https://www.lastweekinaws.com/blog/aurora-vs-rds-an-engineers-guide-to-choosing-a-database/
[^3]:  https://aws.amazon.com/rds/aurora/mysql-features/
[^4]:  https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Overview.StorageReliability.html
[^5]:  https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_Limits.html
[^6]:  https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Replication.html#Aurora.Replication.Replicas
[^7]:  https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-multi-master.html
[^8]:  https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraPostgreSQL.Optimize.html
[^9]:  https://www.percona.com/blog/2018/07/17/when-should-i-use-amazon-aurora-and-when-should-i-use-rds-mysql/

