# Installation

The PeopleSoft Connector installation consists of multiple steps that are required to complete the installation successfully. 

The connector requires a SMA OpCon Windows Agent to provide the connection between the PeopleSoft environment and OpCon. 

## Supported Software Levels
The following software levels are required to implement this version (20.8.1) of the PeopleSoft Connector.

- OpCon Release 19.0 or higher.
- Embedded Java OpenJDK 11 (part of installation).
- PeopleSoft version 8.1.

## Requirements
- The SMAPSCOnnector can be installed on any Windows erver where there is an OpCon Agent installed.
- If the SEND_STATUS_MESSAGE capability is enabled, the connector uses the SMAStatus.exe program of the Windows Agent to submit messages to OpCon. 
- The connector requires a copy the Peoplesoft Tools jar (psjoa.jar) that is included in the connector build.

## Installation
The installation process consists of the following steps:

- OpCon Windows Agent Installation.
- PeopleSoft Connector Installation.
- Adding PeopleSoft Connector job subtype to Enterprise Manager.
- PeopleSoft Connector Configuration.
 
### OpCon Windows Agent Installation
The PeopleSoft Connector requires the installation of a Windows Agent. Either use an existing Windows Agent or complete the installation of the Windows Agent.

### PeopleSoft Connector Installation
Copy the downloaded install file PeopleSoftConnector-win.zip and extract it into a temp directory (c:\temp). Extract the information including sub-directories into the required target directory.

After the extraction, the root installation directory contains the connector executable (SMAPSConnector.exe), the Connector.config file and three directories, emplugins, java and log. 
The emplugins directory contains the PeopleSoft Job Subtype, java directory contains the java software required to execute the connector (OpenJDK 11) and the log directory contains the connector log files.

### Job Subtype Installation
Copy the Enterprise Manager plug-in from the ***installation_dir***\\emplugins directory to the dropins directory of the Enterprise Manager installation. 
If the dropins directory does not exist, create the dropins directory off the root directory. 

Restart Enterprise Manager and a new Windows job subtype called PeopleSoft will be visible.

If not restart Enterprise Manager using 'Run as Administrator'. After this Enterprise Manager can be used normally.

### Create PeopleSoftPath Global Property
Create a global property **PeopleSoftPath** that contains the full path of the installation directory.

### PeopleSoft Connector Configuration
The configuration of the PeopleSoft Connector requires setting the required values in the Connector.config file. The Connector.config file contains information for the PeopleSoft Connector that enables the connector to communicate successfully with the PeopleSoft Application. 

#### Connector.config configuration
Configure the Connector.config file in the installation directory setting the required information.
The Connector.config contains the following values

Property Name | Value
--------------------------- | -----------
**[GENERAL]**               | header.
**APPLICATION_TYPE**        | required value is PSFT.
**SEND_STATUS_MESSAGE**     | Enble this feature if the current status of the job being run within PeopleSoft should be displayed in the OpCon Job information. Values are True or False (default **False**).
**WINDOWS_AGENT_DIRECTORY**	| If SEND_STATUS_MESSAGE is set to True, then this value is required so the SMAStatus program can be used to submit Peoplesoft job status changes to OpCon so they can be displayed in the operations environment. NOTE : The backslash character (\\) is a special Java character and if used should be entered twice (\\\\).
**DEBUG**                   | Turns tracing on in the PeopleSoft Connector to assist with fault diagnosis.  Values are True or False (default **False**).
**[PEOPLESOFT]**            | header.
**ADDRESS**                 | The address of the PeopleSoft server.
**PORT**                    | TThe port number associated with the PeopleSoft server.

Example configuration file. 

```

[GENERAL]
APPLICATION_TYPE=PSFT
WINDOWS_AGENT_DIRECTORY=
SEND_STATUS_MESSAGE=False
DEBUG=False

[PEOPLESOFT]
ADDRESS=
PORT=

```
