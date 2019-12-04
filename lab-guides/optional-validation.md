[Back to main guide](../README.md)|[Next](num-dt.md)

___

# Validation : Web application 

We have created a nodeJS application that connects to the source Oracle **HR** schema and lists all the rows from the **employees**  table. This sample web application allows you to add new employees and update existing employee details. We have also cloned and modified the sample web application to support target PostgreSQL.   Both the applications are already installed on the **OracleXE-SCT** EC2 instance. 

In this activity, you will verify the continuous data replication between the Oracle source and the Aurora target using these two applications. 

## Task 1 - Connect and start the web application connected to the Oracle source.
1. Connect to the **OracleXE-SCT** EC2 instance using the following password, if not already connected.
     **User Name**: administrator   
    **Windows password**: GPSreInvent@321 
2. Click the **Start** button, right click on **Windows PowerShell**, and click **Open new window**. 
3. Start the Oracle web application by executing the following command in **PowerShell**.
```
C:\Lab\oracle-app\start-app.ps1
```
4.  Once the application is successfully started (this could take a minute), open the web application by visiting following URL : http://localhost:4200/ 
5.  Verify that the Oracle Web application is listing all the rows from the **employees** table.

## Task 2 - Configure and start the web application connected to the Aurora PostgreSQL target database
1.  Update the database `config` file to point to the target Aurora PostgreSQL end point. Navigate to `C:\Lab\pgs-app\hr_app\config\`, open the **database.js** config file in TextPad (right click), and update the **host** parameter with the `AuroraPostgreSQLEndpoint` value from the [CloudFormation stack output](./lab-setup-verification.md#cloudformation-stack-outputs).
2. Open another PowerShell window for running PostgreSQL Web application. Click the **Start** button, right click on **Windows PowerShell**, and click **Open new window**.  
3. Start the PostgreSQL web application by executing the following script in **PowerShell**.
```
C:\Lab\pgs-app\start-app.ps1
```
4.  Once the application is successfully started(this could take a minute), open the web application by visiting following URL : http://localhost:4400/ 
5.  Verify that the PostgreSQL Web application is listing all the rows from **employees** table.

## Task 3 - Validate the on going data replication / CDC. 
Now you are running two applications, one connected to the source Oracle database and another connected to the target PostgreSQL databases with DMS migration task configured to replicate the data changes from the source to the target.

1. Add a new employee from your Oracle web application, by clicking **Add Employee** button.
2. Verify that newly added employee details appear in the PostgreSQL web application (refresh the page). 
3. Update an employee in the Oracle web application and verify that change is replicated to the target PostgreSQL web application (refresh the page). 

### Conclusion
This part of the workshop demonstrated a database replication with Data Change Capture in real time.

___

[Back to main guide](../README.md)|[Next](num-dt.md)