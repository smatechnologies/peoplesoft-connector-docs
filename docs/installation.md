---
sidebar_label: 'Installation'
title: PeopleSoft Connector installation
description: "Install and configure the SMA OpCon PeopleSoft Connector on a Windows Agent host, including supported software levels, prerequisites, the four installation phases, and Connector.config settings."
tags:
  - Procedural
  - System Administrator
  - PeopleSoft Connector
---

# Installation

This page walks you through installing the PeopleSoft Connector on a Windows Agent host. Plan for four short phases:

1. **Prepare** — confirm a supported Windows Agent and PeopleSoft environment.
2. **Install the connector** — extract the install package on the agent host.
3. **Register the job sub-type** — drop the plug-in into Enterprise Manager.
4. **Configure** — create the **PeopleSoftPath** global property and set values in `Connector.config`.

Allow roughly 15–30 minutes once prerequisites are in place.

## What is it?

The PeopleSoft Connector installation places the connector executable, an embedded Java runtime, and a PeopleSoft job sub-type plug-in onto a Windows Agent host. After files are in place, you set values in `Connector.config` so the connector can communicate with the PeopleSoft Application.

The connector requires an SMA OpCon Windows Agent to provide the connection between the PeopleSoft environment and OpCon.

## Before you begin

Confirm the following before you start:

**Supported software**

- OpCon Release 19.0 or higher.
- PeopleSoft version 8.1.
- Java OpenJDK 11 — included as an embedded runtime in the installation package, no separate install required.

**Environment**

- A Windows server that already has an SMA OpCon Windows Agent installed. The connector runs under that agent.
- Network connectivity from the agent host to the PeopleSoft server (address and port).
- Administrator access on the agent host so you can extract files and update Enterprise Manager.

**Files**

- The PeopleSoft Connector install package (`PeopleSoftConnector-win.zip`).
- A copy of the PeopleSoft Tools jar (`psjoa.jar`) — included in the connector build.

:::note
If you plan to display PeopleSoft job status in OpCon (`SEND_STATUS_MESSAGE=True`), the connector calls `SMAStatus.exe` from the Windows Agent. Note the Windows Agent installation path — you will need it during configuration.
:::

## Step 1 — Verify the OpCon Windows Agent

The PeopleSoft Connector runs under the Windows Agent. Confirm an existing agent is installed on the target host, or complete a Windows Agent installation before continuing.

## Step 2 — Install the PeopleSoft Connector

To install the PeopleSoft Connector, complete the following steps:

1. Copy `PeopleSoftConnector-win.zip` to a temporary directory on the agent host (for example, `C:\temp`).
2. Extract the contents, including sub-directories, into the target installation directory.
3. Confirm the root installation directory contains:

   | Item | Purpose |
   |---|---|
   | `SMAPSConnector.exe` | The connector executable. |
   | `Connector.config` | The connector configuration file (you edit this in Step 4). |
   | `emplugins\` | Contains the PeopleSoft job sub-type plug-in for Enterprise Manager. |
   | `java\` | The embedded Java runtime (OpenJDK 11) used by the connector. |
   | `log\` | Where connector log files are written. |

## Step 3 — Register the job sub-type in Enterprise Manager

To make the PeopleSoft job sub-type available to job authors, complete the following steps:

1. Copy the Enterprise Manager plug-in from the ***installation_dir***`\emplugins` directory to the `dropins` directory of the Enterprise Manager installation.
2. If a `dropins` directory does not exist, create it off the Enterprise Manager root directory.
3. Restart Enterprise Manager.

A new Windows job sub-type called **PeopleSoft** appears in Enterprise Manager.

:::tip
If the **PeopleSoft** sub-type does not appear after restart, restart Enterprise Manager using **Run as Administrator**. After it appears, Enterprise Manager can be used normally.
:::

## Step 4 — Configure the connector

Configuration has two parts: create the **PeopleSoftPath** global property in OpCon, and set required values in `Connector.config`.

### Create the PeopleSoftPath global property

Create a global property named **PeopleSoftPath** in OpCon and set its value to the full path of the connector installation directory. PeopleSoft job definitions use this property as the default value for the **Connector Path** field.

:::note
If you plan to install more than one connector on the same system, create an additional global property for each (for example, `PeopleSoftPath2`) and point each job's **Connector Path** field at the appropriate property.
:::

### Edit Connector.config

`Connector.config` lives in the connector installation directory. Open it in a text editor and set the values for both sections, as described in [Configuration reference](#configuration-reference).

A complete starter file looks like this:

```ini
[GENERAL]
APPLICATION_TYPE=PSFT
WINDOWS_AGENT_DIRECTORY=
SEND_STATUS_MESSAGE=False
DEBUG=False

[PEOPLESOFT]
ADDRESS=
PORT=
```

## Configuration reference

`Connector.config` is divided into two sections: `[GENERAL]` and `[PEOPLESOFT]`.

### [GENERAL] section

| Setting | What it does | Default | Notes |
|---|---|---|---|
| `APPLICATION_TYPE` | Identifies the application type for the connector. | — | Required value is `PSFT`. |
| `SEND_STATUS_MESSAGE` | When enabled, the current status of the job running within PeopleSoft is displayed in the OpCon job information. | `False` | Values are `True` or `False`. |
| `WINDOWS_AGENT_DIRECTORY` | Path to the Windows Agent. Used by the `SMAStatus` program to submit PeopleSoft job status changes to OpCon for display in the operations environment. | — | Required when `SEND_STATUS_MESSAGE` is `True`. |
| `DEBUG` | Turns tracing on in the PeopleSoft Connector to assist with fault diagnosis. | `False` | Values are `True` or `False`. |

:::caution
The backslash (`\`) is a special Java character. When you set `WINDOWS_AGENT_DIRECTORY`, enter each backslash twice. For example, write `C:\\Program Files\\SMA Technologies\\OpCon\\Agent` instead of `C:\Program Files\SMA Technologies\OpCon\Agent`.
:::

### [PEOPLESOFT] section

| Setting | What it does | Default | Notes |
|---|---|---|---|
| `ADDRESS` | The address of the PeopleSoft server. | — | Required. |
| `PORT` | The port number associated with the PeopleSoft server. | — | Required. |

## Verify the installation

After the four steps are complete, confirm the connector is ready:

- The connector installation directory contains `SMAPSConnector.exe`, `Connector.config`, and the `emplugins`, `java`, and `log` sub-directories.
- The **PeopleSoft** job sub-type appears under the Windows job type in Enterprise Manager.
- The OpCon global property **PeopleSoftPath** exists and points at the installation directory.
- `Connector.config` has values set for `APPLICATION_TYPE`, `[PEOPLESOFT] ADDRESS`, and `[PEOPLESOFT] PORT`.

## Next steps

- Define a START or TRACK job — see [Operation](./operation.md).
- Review release notes for the version you installed — see [Release notes](./release-notes.md).

## FAQs

**Where do I install the connector?**
On any Windows server where an OpCon Windows Agent is installed. The connector runs under the agent.

**Do I need a separate Java installation?**
No. The connector ships with an embedded Java version (OpenJDK 11) in the `java` directory of the installation.

**What is the PeopleSoftPath global property used for?**
**PeopleSoftPath** holds the full path of the connector installation directory. PeopleSoft job definitions use it as the default value for the **Connector Path** field. If you install more than one connector on the same host, define an additional global property and point those jobs at it.

**How do I send PeopleSoft job status to OpCon?**
Set `SEND_STATUS_MESSAGE=True` and set `WINDOWS_AGENT_DIRECTORY` to the path of the Windows Agent. The connector uses `SMAStatus.exe` from the agent to submit status messages.

**Why do I need to double the backslashes in `WINDOWS_AGENT_DIRECTORY`?**
The backslash (`\`) is a special character in Java. Doubling it (`\\`) escapes it so the path is read correctly.

**The PeopleSoft sub-type didn't appear after I restarted Enterprise Manager. What now?**
Restart Enterprise Manager using **Run as Administrator**. The plug-in registration step requires elevated permissions on some systems.

**Can I install more than one connector on the same host?**
Yes. Define an additional global property for each connector instance and point each job's **Connector Path** field at the matching property.

## Glossary

> **Connector.config** — The configuration file that defines how the PeopleSoft Connector communicates with the PeopleSoft Application. Located in the installation directory.

> **Enterprise Manager** — The OpCon component used to define jobs. The PeopleSoft job sub-type plug-in is installed into the Enterprise Manager `dropins` directory.

> **PeopleSoftPath** — The default OpCon global property name that contains the full path to the PeopleSoft Connector installation directory.

> **SMAStatus.exe** — The Windows Agent program used by the connector to submit PeopleSoft job status messages to OpCon when `SEND_STATUS_MESSAGE` is enabled.

> **psjoa.jar** — The PeopleSoft Tools jar bundled with the connector. Provides the connectivity components used to communicate with the PeopleTools Process Scheduler.
