[Back to main guide](../README.md)|[Next](cleanup.md)

___

# Validation : Web application 

We have created a nodeJS application that connects to source Oracle **HR** schema and lists all the rows from **employees**  table. This sample web application allows you to add new employees and update existing employee details. We have also cloned and modified the sample web application to support target PostgreSQL.   Both the applications are already installed on the **OracleXE-SCT** Ec2 instance. 
In this activity you will verify the Change Data Capture using these two applications by following the following steps. 

In case you are interested to know the changes made to target application to make application compatible from Oracle to PostgresQL refer to [To be updated]

## Task 1 - Connect and start Oracle web application
1. Connect to the **OracleXE-SCT** EC2 instance using the following password, if not already connected.
     **User Name**: administrator   
    **Windows password**: GPSreInvent@321 
2. On the Start screen, click **Windows PowerShell**. 
3. Start the Oracle web application by executing the following script in **PowerShell**.
```
C:\Lab\oracle-app\start-app.ps1

```
4.  Once the application is successfully started, open the web application by visiting following URL : http://localhost:4200/ 
5.  Verify Oracle Web application is listing all the rows from **employees** table.

## Task 2 - Configure and start PostgreSQL web application
1.  Update the database `config` file to point to target Aurora PostgreSQL cluster end point. Update **host** parameter in `C:\Lab\pgs-app\hr_app\config\database.js` config file with `AuroraPostgreSQLEndpoint` from [CloudFormation stack output](./lab-setup-verification.md#cloudformation-stack-outputs).
2. Open another PowerShell window for running PostgreSQL Web application. On the Start screen, click **Windows PowerShell**. 
3. Start the PostgreSQL web application by executing the following script in **PowerShell**.
```
C:\Lab\pgs-app\start-app.ps1

```
4.  Once the application is successfully started, open the web application by visiting following URL : http://localhost:4400/ 
5.  Verify PostgreSQL Web application is listing all the rows from **employees** table.

## Task 3 - Validate the on going data replication / CDC. 
Now you are running two applications, one connected source Oracle database and another connected to target PostgreSQL databases with DMS migration task configured to capture data changes from source.

1. Add a new employee from your Oracle web application, by clicking **Add Employee** button.
2. Verify that row employee appears in PostgreSQL web application. 
3. Update an employee in Oracle web application and verify same is replicated to target PostgreSQL web application. 

### Conclusion
This part of the workshop demonstrated a database replication with Data Change Capture in real time.

___

[Back to main guide](../README.md)|[Next](cleanup.md)