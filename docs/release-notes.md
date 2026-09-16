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

:::note

The connector has received maintenance since 20.8.1, including updates to the embedded Java runtime and to the build and code-signing pipeline. These changes are not individually versioned in this release history. Check with your support contact to confirm the current release for your environment.

:::

## 20

### 20.8.1

#### What's new

- **New installer format.** The connector files are extracted from a zip file into the directory of your choice rather than being placed by an installer.
- **Embedded Java runtime.** The connector ships with its own Java version, so it no longer depends on the Java installed on the host.
- **Configuration file renamed.** `Agent.config` is now `Connector.config`.

#### Fixes

- **Replaced log4j with slf4j and logback.**

#### Why this matters

Replacing log4j with slf4j and logback addresses the log4j security concerns that affected many Java components. The new installer format and embedded Java version remove the need to maintain a separate Java installation, simplifying deployment and reducing the chance of version mismatches in customer environments.

#### Migration considerations

When upgrading from an earlier release:

- Rename your existing `Agent.config` to `Connector.config`, or recreate it under the new name. The connector does not read the old name.
- Extract the package into the installation directory of your choice. There is no installer to run.
- Remove any dependency this connector had on a host-installed Java version.
