[Back to main guide](../README.md)|[Next](optional-validation.md)

___

# Run a DMS Replication Task for Change Data Capture (CDC)

You can use AWS Database Migration Service to perform continuous data replication. This helps you migrate your databases to AWS with virtually no downtime. All data changes to the source database that occur during the migration are continuously replicated to the target, allowing the source database to be fully operational during the migration process. After the database migration is complete, the target database will remain synchronized with the source for as long as you choose, allowing you to switchover the database at a convenient time.

Oracle offers two methods for reading the redo logs when doing change processing, Oracle LogMiner and Oracle Binary Reader. Oracle LogMiner provides a SQL interface to Oracle’s online and archived redo log files. Binary Reader is an AWS DMS feature that reads and parses the raw redo log files directly.

By default, AWS DMS uses Oracle LogMiner for change data capture (CDC).

To capture change data, the source database must be in ARCHIVELOG mode and supplemental logging must be enabled.

Similarly, you need to enable table-level supplemental logging for each table that you want to migrate.

Refer to [Using Oracle LogMiner or Oracle Binary Reader for Change Data Capture (CDC)](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.Oracle.html#CHAP_Source.Oracle.CDC) for more details.  

_**Note: For this workshop we have already made these changes in the source Oracle database.**_

In this activity, you perform the following tasks:

- Run a DMS Replication Task for Change Data Capture (CDC)
    - Enable CDC on the source database
    - Set up and run a Replication Task
    - Introduce changes at the source database
    - Validate the CDC result at the target database
  
___

## Task 1 - Configure and run a CDC Replication Task
In this part of the lab you are going to create another Database Migration Task for capturing data changes from the source Oracle database and migrate to target Aurora PostgreSQL.

1. Click on **Database migration tasks** on the navigation menu, then click on the **Create task** button.

![Create replication task](images/create_task.png)


2. Create a data migration task with the following values for migrating the `HR` database.

Parameter | Value
--- | ---
Task identifier | oracle-migration-task-cdc
Replication instance | your replication instance
Source database endpoint | oracle-source
Target database endpoint | aurora-postgresql-target
Migration type | Replicate data changes only
Start task on create | Checked
CDC start mode | Don’t use custom CDC start mode
CDC stop mode | Don’t use custom CDC stop mode
Target table preparation mode | Do nothing
Include LOB columns in replication | Limited LOB mode
Max LOB size (KB) | 32
Enable validation | Unchecked
Enable CloudWatch logs | Checked

*Enabling the logging would help debugging issues that DMS encounters during data migration*

3. Expand the Table mappings section, and select Guided UI for the editing mode
4. The Table mappings are as below which are same as the mappings of the previous full-load replication task. Click on Add new selection rule button and enter the following values.

Parameter | Value
----- | -----
Schema name | HR
Table name| %
Action | Include

_Hint:click “Enter a schema” from drop down to enter the schema name._

5. Next, expand the Transformation rules section, and click on Add new transformation rule. Then, create the following rules:

Parameter | Value
-------- | --------
Target | Schema
Schema name | HR
Action | Make lowercase

Parameter | Value
-------- | --------
Target | Table
Schema Name | HR
Table Name | %
Action | Make lowercase

Parameter | Value
-------- | --------
Target | Column
Schema Name | HR
Table Name | %
Column Name | %
Action | Make lowercase

![Create task mappings](images/create_task_mappings_cdc.png)

6. After entering the values click on **Create task**.

7. Wait till the task status changes to **Replication ongoing**, this may take a couple of minutes.
    
    ![Migration Task Progress](images/migration_complete_cdc.png)

___

## Task 2 - Validate the on going data replication / CDC. 

1. Log in to the SQL Developer connecting to the source Oracle database on **OracleXE-SCT** EC2 instance. 
2. Verify records in existing `REGIONS` table in `HR` schema.

````
SELECT * FROM HR.REGIONS;
````

![Initial verification](images/initial_verification.png)

3. Add additional two rows to `REGIONS` table

````
INSERT INTO HR.REGIONS VALUES (5,'APAC');

INSERT INTO HR.REGIONS VALUES (6,'LATIN AMERICA');

COMMIT WORK;

````

4. Log in to the SQL Developer connecting to the target Aurora PostgreSQL database on **OracleXE-SCT** EC2 instance.

5. Verify whether the changes are migrated to `REGION` table in target Aurora PostgreSQL database.

````
SELECT * FROM hr.regions;
````

![Final verification](images/final_verification.png)

6. You can further verify the number of inserts, deletes, updates, and DDLs by viewing the **Table statistics** of **oracle-migration-task-cdc** task in AWS console.

![Table statistics](images/table_statistics_cdc.png)

___ 

## Conclusion
This part of the workshop demonstrated a database replication with Data Change Capture in real time.
___

[Back to main guide](../README.md)|[Next](optional-validation.md)