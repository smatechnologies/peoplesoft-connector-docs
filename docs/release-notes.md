---
sidebar_label: 'Release notes'
title: PeopleSoft Connector release notes
description: "Version history and change details for the PeopleSoft Connector, including new features, improvements, and migration considerations."
tags:
  - Reference
  - System Administrator
  - PeopleSoft Connector
---

# PeopleSoft Connector release notes

## 20

### 20.8.1

:eight_spoked_asterisk: Removed log4j and replaced it with slf4j and logback.

:eight_spoked_asterisk: Introduced the new format installer where the files are extracted from the zip file into the desired directory.

:eight_spoked_asterisk: Embedded a Java version with the connector so there is no reliance on installed Java versions.

:eight_spoked_asterisk: Renamed the configuration file from `Agent.config` to `Connector.config`.

### Why this matters

Replacing log4j with slf4j and logback addresses the log4j security concerns that affected many Java components. The new installer format and embedded Java version remove the need to maintain a separate Java installation, simplifying deployment and reducing the chance of version mismatches in customer environments.

### Migration considerations

This release includes the new format installer where the files are extracted from the zip file into the desired directory. It contains an embedded Java version for the connector, so there is no reliance on installed Java versions. The configuration file name has also been changed from `Agent.config` to `Connector.config`.
