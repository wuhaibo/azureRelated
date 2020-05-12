## Benefits of using Azure to store data
- automated backup and recovery
- replication across the globe
- support for data analytics
- encryption capabilities
- multiple data types
- data storage in virtual disks: up to 32TB
- storage tiers: like hot, cool storage tiers

### Types of data supported by azure data storage
- structured data. 
- semi-structured data. like nosql data
- unstructured data. like binary,video etc. 

### azure storage options  
- Azure sql database
- Azure cosmos db,globally distributed database service
- Azure blob. for unstructured data. streaming video possible.
- Azure Data Lake Storage. For big data. 
- Azure Files. Via SMB protocol.
- Azure Queue. Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world.
- Disk Storage. like virtual Disk. 
  
### storage features
- storage tiers
  1. hot storage
  2. cool storage
  3. archive storage

- encryption and replication
  1. azure storage service encryption(SSE) for data at rest. encryption and decryption from azure for data at rest.
  2. client-side encryption. encryption from client lib.
  3. replication possible.

### azure storage vs on-premise storage
- cost effectiveness
- reliability
- storage types
- agility
![alt](img/azureStorageVsOnpremiseStorage.png)


## Provision an Azure SQL database

- why azure sql database
  1. Convenience
  2. Cost
  3. Scale
  4. Security


### Choose performance: DTUs versus vCores
- dtu: database transaction unit. a combined meaure of compute,storage and io.
  1.  https://sqlperformance.com/2017/03/azure/what-the-heck-is-a-dtu
  2. dtu to IOPS(input output per second). https://stackoverflow.com/questions/24915593/translate-sql-azure-dtu-to-iops 
  3. good to read. about db metric https://docs.microsoft.com/en-gb/azure/sql-database/sql-database-service-tiers-dtu
  4. migrating from dtus to vcores needs long down time.
  5. Scaling: Single Azure SQL Database supports manual dynamic scalability, but not autoscale. For a more automatic experience, consider using elastic pools, which allow databases to share resources in a pool based on individual database needs. However, there are scripts that can help automate scalability for a single Azure SQL Database. For an example, see Use PowerShell to monitor and scale a single SQL Database.(https://docs.microsoft.com/en-us/azure/sql-database/scripts/sql-database-monitor-and-scale-database-powershell). You can change DTU service tiers or vCore characteristics at any time with minimal downtime to your application (generally averaging under four seconds).

- vcore: virtual core. in this model, you control the vcore, storage capacity and io throughput seperately. 

![alt](img/dtuVsVcore.png)

### sql elastic pools
sql elastic pools relate to eDTUs. They enable you to buy a set of compute and storage resources that are shared among all the databases in the pool. Each database can use the resources they need, within the limits you set, depending on current load.

### Azure SQL logical server
An administrative container for your databases, like setting firewall white list ip.

### What is collation
Collation refers to the rules that sort and compare data, like if case sensitivity, accent marks, and other language characteristics are important.
**SQL_Latin1_General_CP1_CI_AS** is the default collation. It means

- Latin1_General refers to the family of Western European languages.
- CP1 refers to code page 1252, a popular character encoding of the Latin alphabet.
- CI means that comparisons are case insensitive. For example, "HELLO" compares equally to "hello".
- AS means that comparisons are accent sensitive. For example, "résumé" doesn't compare equally to "resume".


## Azure sql elastic pool

### what is elastic pool
a group of Azure SQL databases. The pool allows the databases within the pool to share the allocated resources.

### When to use an elastic pool?
SQL elastic pools are ideal when you have several SQL databases that have a low average utilization, but have infrequent, high utilization spikes. 

### How many databases to add to a pool?
The general guidance is, if the combined resources you would need for individual databases to meet capacity spikes is more than 1.5 times the capacity required for the elastic pool, then the pool will be cost effective.
At a minimum, it is recommended to add at least two S3 databases or fifteen S0 databases to a single pool.
Depending on the performance tier, you can add up to **100 or 500 databases** to a single pool.

### create elastic pool
create sql server -> db -> elastic pool(link to sql server) -> add db to elastic pool

one sql server can have multiple elastic pool. one db can only in one elastic pool.

## Migrate your relational data stored in SQL Server to Azure SQL Database

### steps
![alt](img/dbMigrationProcess.png)

1. Pre-migration(using DMA Datamigration Assistant, which is client side app from microsoft, to use it, you have to be dba of the server)
    - discovery.
    - assessment: check incompatibilities between azure sql server and on-premise sql server
    - convert: DMA generates the sql script for deploying db schema to azure sql server
  
2. Migration
   -  **schema**, this step can be done using Data Migration Assistant or manually via SQL Server Management Studio with the genreated script from the step 1.
   -  **data**, this step is done via Azure Database Migration Service. Database Migration Service can be run in two modes, **online and offline**(online requests Premium tier, cost more, best practice: try offline first). When it's running in online mode, there are two additional steps. The first is **sync**, in which any changes made to the data in the source system after the migration are brought into the target database. The other is **cutover**, in which the source database is taken offline and the new Azure SQL database becomes available.
   ![alt](img/offlineMode.png)
   ![alt](img/onlineMode.png)

3. Post-migration
   - Validation testing 
   - performance tests

The post-migration phase is critical because it ensures that your data is both accurate and complete. In addition, it alerts you to any performance issues that might arise with the workload in the new environment.

## Develop and configure an ASP.NET application that queries an Azure SQL database

use public endpoint of the db to connect.