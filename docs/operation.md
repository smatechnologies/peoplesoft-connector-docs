# Operation

The Enterprise Manager includes job sub-type definitions for the PeopleTools Process Scheduler. The job sub-type can be accessed by selecting the PeopleSoft job subtype from the drop-down list when the Windows Job Type has been selected. 

It should be remembered that the PeopleSoft Connector does not create job definitions in the PeopleSoft database, but rather only uses existing job.

## PeopleSoft Job definitions
The PeopleSoft Connector defines two job types (START, TRACK) that can be processed by the PeopleTools Process Scheduler. 

- START     Start a defined PeopleSoft job and track the job execution until completion.
- TRACK     Track a job already started by the PeopleTools Scheduler to completion. 

When defining a PeopleSoft job, select a Job Type of Windows and then a Job Sub-Type of PeopleSoft. 

The PeopleSoft Definition screen will then appear.

### START Job Definition

The START Job Type Definition contains the following fields

Field | Description
---------------------------- | -----------
**Connector Path**           | Required field that contains the installed location of the PeopleSoft Connector. This consists of a global property value which contains the root installation directory. Default value is **PeopleSoftPath**. If more than one Connector is installed on the same system, then an additional global property should be defined and the entry in this field updated. 
**Job Type**                 | Required field and contains the job type that the connector should process. Select **START** from the drop-down list. 
**Run Control ID**           | Optional field that is a grouping associated with the PeopleSoft Username.
**Process Type**             | Required field that defines what type of job to process. The value can be selected from the drop-down list. Currently the connector supports the following job types (**Application Engine, COBOL SQL, Crystal, PSJob, SQR Report**).
**Process Name**             | Required field and defines the name of the job in the Process Scheduler to run.
**PeopleSoft User Name**     | Required field and contains the name of a PeopleSoft user that has the required privileges to run the defined job.
**PeopleSoft User Password** | Required field and contains the password of the PeopleSoft user that has the required privileges to run the defined job. This value should be defined in an encrypted global property.

### TRACK Job Definition

The TRACK Job Type Definition contains the following fields

Field | Description
---------------------------- | -----------
**Connector Path**           | Required field that contains the installed location of the PeopleSoft Connector. This consists of a global property value which contains the root installation directory. Default value is **PeopleSoftPath**. If more than one Connector is installed on the same system, then an additional global property should be defined and the entry in this field updated. 
**Job Type**                 | Required field and contains the job type that the connector should process. Select **START** from the drop-down list. 
**Run Control ID**           | Optional field that is a grouping associated with the PeopleSoft Username.
**Process Type**             | Required field that defines what type of job to process. The value can be selected from the drop-down list. Currently the connector supports the following job types (**Application Engine, COBOL SQL, Crystal, PSJob, SQR Report**).
**Process Name**             | Required field and defines the name of the job in the Process Scheduler to run.
**Schedule Date**            | The execution date of the job in the Process Scheduler. This can be an OpCon property that contains the appropriate value or a date formatted as MM/DD/YYYY. 
**Schedule Time**            | An optional parameter, which can be used to fine tune the search for the correct job to track in the Process Scheduler. The format of the time is hh:MM:SSAM or PM. 
**Server Name**              | An optional parameter, which can be used to fine tune the search for the correct job to track in the Process Scheduler. It consists of the name of the server where the job will be executed on.
**PeopleSoft User Name**     | Required field and contains the name of a PeopleSoft user that has the required privileges to run the defined job.
**PeopleSoft User Password** | Required field and contains the password of the PeopleSoft user that has the required privileges to run the defined job. This value should be defined in an encrypted global property.


## Job Finished processing 

The successful completion code of PeopleSoft jobs is 9. This means that to check for a successful completion, the Failure Criteria should be set to NE (Not Equal) to 9.

### PeopleSoft Job Codes

Code | Text | Description
---- | -------------- | ----------------------
1	 | Cancel         | Job was cancelled by PeopleSoft.
2	 | Delete         | Job was deleted by PeopleSoft.
3	 | Error          | Job errored.
4	 | Hold           | Job is placed on Hold by PeopleSoft.
5	 | Queued         | Job is queued with PeopleSoft.
6	 | Initiated      | Job is initiated by PeopleSoft.
7	 | Processing     | Job is running in PeopleSoft.
8	 | Cancelled      | Job was Cancelled by PeopleSoft.
9	 | Success        | Job execution was successful.
10	 | Not Successful | Job execution failed.



## Logging
The default logging implemented by the connector consists of a maximum cycle of five log files. The log files contain information about the PeopleSoft Connector and any jobs run by the PeopleSoft Connector. The log files (peoplesoft.log) are located in the ***installation_dir_***\\log directory. Information is appended into the log files and any error messages, return codes can be viewed in these log files.

All information produced by the OpCon job is available in the job output and can be retrieved using the OpCon JORS capability. When the job fails, the jde and jdedebug logs are appended to the OpCon job output and can be retrieved using JORS. When used in conjunction with the Event Notification System, the error logs can be attached to emails that can be sent to define email or group email addresses.

```