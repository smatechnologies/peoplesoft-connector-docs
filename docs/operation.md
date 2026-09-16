---
sidebar_label: 'Operation'
title: PeopleSoft Connector operation
description: "Define and run PeopleSoft jobs through the SMA OpCon PeopleSoft Connector, including START and TRACK job definitions, completion codes, and logging."
tags:
  - Reference
  - Automation Engineer
  - PeopleSoft Connector
---

# Operation

## What is it?

The PeopleSoft Connector lets you define OpCon jobs that start or track existing PeopleSoft batch processes through the PeopleTools Process Scheduler. Enterprise Manager includes job sub-type definitions for the PeopleTools Process Scheduler. Select the PeopleSoft job sub-type from the list when the Windows job type has been selected.

The PeopleSoft Connector does not create job definitions in the PeopleSoft database. It only uses existing PeopleSoft jobs.

## PeopleSoft job definitions

The PeopleSoft Connector defines two job types that the PeopleTools Process Scheduler can process:

- **START** — Starts a defined PeopleSoft job and tracks the job to completion.
- **TRACK** — Tracks a job already started by the PeopleTools Scheduler to completion.

To define a PeopleSoft job, select a job type of Windows and then a job sub-type of PeopleSoft. The PeopleSoft definition screen appears.

### START job definition

The START job type definition contains the following fields:

| Field | Description |
|---|---|
| **Connector Path** | Required. Contains the installed location of the PeopleSoft Connector. This is a global property value containing the root installation directory. The default value is **PeopleSoftPath**. If more than one connector is installed on the same system, define an additional global property and update this field. |
| **Job Type** | Required. The job type that the connector should process. Select **START** from the list. |
| **Run Control ID** | Optional. A grouping value associated with the PeopleSoft user name. |
| **Process Type** | Required. The type of job to process. Select the value from the list. The connector supports the following job types: **Application Engine**, **COBOL SQL**, **Crystal**, **PSJob**, **SQR Report**. |
| **Process Name** | Required. The name of the job in the Process Scheduler to run. |
| **PeopleSoft User Name** | Required. The name of a PeopleSoft user that has the required privileges to run the defined job. |
| **PeopleSoft User Password** | Required. The password of the PeopleSoft user that has the required privileges to run the defined job. Define this value in an encrypted global property. |

### TRACK job definition

The TRACK job type definition contains the following fields:

| Field | Description |
|---|---|
| **Connector Path** | Required. Contains the installed location of the PeopleSoft Connector. This is a global property value containing the root installation directory. The default value is **PeopleSoftPath**. If more than one connector is installed on the same system, define an additional global property and update this field. |
| **Job Type** | Required. The job type that the connector should process. Select **TRACK** from the list. |
| **Run Control ID** | Optional. A grouping value associated with the PeopleSoft user name. It is not used when tracking an existing process — use **Schedule Date**, **Schedule Time** and **Server Name** to identify the correct process. |
| **Process Type** | Required. The type of job to process. Select the value from the list. The connector supports the following job types: **Application Engine**, **COBOL SQL**, **Crystal**, **PSJob**, **SQR Report**. |
| **Process Name** | Required. The name of the job to find in the Process Scheduler. A TRACK job does not start the process; it locates one that is already running. |
| **Schedule Date** | Optional. The date the job runs in the Process Scheduler. The value can be an OpCon property containing the appropriate value, or a date formatted as `MM/DD/YYYY`. |
| **Schedule Time** | Optional. Used to refine the search for the correct job to track in the Process Scheduler. The format of the time is `hh:MM:SSAM` or `PM`. |
| **Server Name** | Optional. Used to refine the search for the correct job to track in the Process Scheduler. The name of the server where the job runs. |
| **PeopleSoft User Name** | Required. The name of a PeopleSoft user that has the required privileges to run the defined job. |
| **PeopleSoft User Password** | Required. The password of the PeopleSoft user that has the required privileges to run the defined job. Define this value in an encrypted global property. |

## Job finished processing

The successful completion code of PeopleSoft jobs is **9**. To check for a successful completion, set the failure criteria to NE (Not Equal) to 9.

The connector checks the status of the PeopleSoft process five seconds after it is submitted or found, and every ten seconds after that. Neither interval can be configured.

### Completion codes

A job ends with one of these six codes. The connector returns the PeopleSoft status as the OpCon job's return code.

| Code | Text | Description |
|---|---|---|
| 1 | Cancel | Job was cancelled by PeopleSoft. |
| 2 | Delete | Job was deleted by PeopleSoft. |
| 3 | Error | Job errored. |
| 8 | Cancelled | Job was Cancelled by PeopleSoft. |
| 9 | Success | Job ran successfully. |
| 10 | Not Successful | Job run failed. |

### In-progress states

These four states are reported while the job is still being processed. A job never ends with one of these codes.

| Code | Text | Description |
|---|---|---|
| 4 | Hold | Job is placed on Hold by PeopleSoft. |
| 5 | Queued | Job is queued with PeopleSoft. |
| 6 | Initiated | Job is initiated by PeopleSoft. |
| 7 | Processing | Job is running in PeopleSoft. |

When `SEND_STATUS_MESSAGE=True`, these states are reported to OpCon as the job runs so they appear in the job information.

:::caution

The connector waits for the process to reach one of the six completion codes and does not time out. A process placed on **Hold** in PeopleSoft is treated as still running, so the OpCon job keeps running until the process is released or you cancel the OpCon job.

:::

### Connector return codes

The connector uses two of the codes above for its own failures, so a return code of `1` or `3` does not always describe a PeopleSoft outcome.

| Code | Meaning |
|---|---|
| 1 | The connector could not process its command line, or stopped on an unhandled error. No PeopleSoft process was started or tracked. |
| 3 | Either the PeopleSoft process errored, or the connector could not connect to PeopleSoft, or it could not create or find the process request. |

Check the connector log to tell these apart: it records the connection attempt and the process request before any PeopleSoft status is reported.

## Logging

The connector writes its log files into the ***installation_dir***`\log` directory, which it creates the first time it runs. The log files contain information about the PeopleSoft Connector and any jobs it runs, including error messages and return codes.

The active log file is `peoplesoft.log`. Completed files are rolled into a subdirectory named for the month, with the date and an index in the file name, for example `log\2026-09\peoplesoft_2026-09-16.0.log`. A new file starts each day, and the index increments when a file reaches 100 MB.

:::caution

Rolled log files are retained indefinitely. The `log` directory grows until you remove old files, so include it in whatever disk monitoring you apply to the connector host.

:::

All information produced by the OpCon job is available in the job output and can be retrieved using the OpCon JORS capability. When used in conjunction with the Event Notification System, the job output can be attached to emails that can be sent to defined email or group email addresses.

For diagnostic detail from PeopleSoft itself, use the process output in the PeopleTools Process Scheduler. The connector does not collect or forward PeopleSoft process output.

## FAQs

**What completion code means a PeopleSoft job ran successfully?**
The successful completion code is **9**. Set the failure criteria for the OpCon job to NE (Not Equal) to 9.

**What is the difference between START and TRACK?**
**START** starts a defined PeopleSoft job and tracks it to completion. **TRACK** tracks a PeopleSoft job that has already been started by the PeopleTools Scheduler to completion.

**Where are the connector log files?**
The active log file is ***installation_dir***`\log\peoplesoft.log`. Older files are rolled into month-named subdirectories beneath `log`, and they are retained indefinitely.

**How do I retrieve job output in OpCon?**
Use the OpCon JORS capability. For diagnostic detail from PeopleSoft itself, use the process output in the PeopleTools Process Scheduler — the connector does not collect it.

**Where should I store the PeopleSoft user password?**
Define the **PeopleSoft User Password** value in an encrypted global property.

**Which PeopleSoft process types are supported?**
The connector supports **Application Engine**, **COBOL SQL**, **Crystal**, **PSJob**, and **SQR Report**.

## Glossary

> **START job type** — A PeopleSoft Connector job that starts a defined PeopleSoft job and tracks it to completion.

> **TRACK job type** — A PeopleSoft Connector job that tracks a PeopleSoft job already started by the PeopleTools Scheduler to completion.

> **Process Type** — The category of PeopleSoft batch process. Supported values are **Application Engine**, **COBOL SQL**, **Crystal**, **PSJob**, and **SQR Report**.

> **Process Name** — The name of the job in the Process Scheduler to run.

> **Run Control ID** — An optional grouping value associated with the PeopleSoft user name.

> **JORS** — Job Output Retrieval System. The OpCon capability used to retrieve job output and appended log files.

> **peoplesoft.log** — The connector’s active log file, in the ***installation_dir***`\log` directory. Completed files are rolled into month-named subdirectories and retained indefinitely.
