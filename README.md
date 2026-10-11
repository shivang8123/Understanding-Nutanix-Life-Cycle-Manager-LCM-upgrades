# Nutanix Life Cycle Manager (LCM) Study Notes

A concise reference guide for understanding Nutanix Life Cycle Manager (LCM), software and firmware upgrades, inventory, validation, and dark-site update methods.

## Overview

Nutanix **Life Cycle Manager (LCM)** is used to discover, validate, and upgrade supported Nutanix software and hardware firmware components from Prism.

LCM helps administrators identify:

* Installed software and firmware versions
* Available updates
* Supported upgrade paths
* Component dependencies
* Compatibility issues
* Required health checks and prechecks

> **Important:** Inventory identifies supported and recommended updates. It does not mean that every component should automatically be upgraded to the newest version.

## Components Managed or Evaluated by LCM

### Software

* LCM framework
* Nutanix Cluster Check (NCC)
* AOS
* AHV
* Prism Central
* Foundation
* Cluster Maintenance Utilities
* Other Nutanix services and products, depending on the environment

### Firmware

* BIOS
* BMC or management-controller firmware
* NIC firmware
* Disk or SSD firmware
* Host boot-drive firmware
* Other hardware-specific firmware

Firmware should be upgraded only when the version is supported and recommended for the specific hardware model.

## Why Perform Upgrades?

* Maintain a supported environment
* Apply security fixes
* Resolve bugs and stability issues
* Obtain new features
* Maintain compatibility with newer software or hardware
* Address known field issues

Always review the relevant release notes, compatibility matrix, prerequisites, and known issues before upgrading.

## LCM Update Methods

### 1. Connected Site / Nutanix Portal

In a connected environment, LCM obtains update content through the Nutanix download service.

Typical requirements include:

* DNS and network connectivity
* Correct routing, firewall, NAT, or proxy configuration
* Required Nutanix endpoints allowed through the firewall
* Valid time synchronization and certificates

A private or virtual IP may be used for outbound connectivity, depending on the customer network design.

### 2. Dark Site / Local Web Server

This method is used when the cluster cannot directly access the Nutanix Portal.

Basic workflow:

1. Download approved bundles from the Nutanix Support Portal.
2. Place the bundles on an internal web server.
3. Ensure the cluster or Prism Central can reach the web server.
4. Configure LCM to use the local web server as its update source.
5. Run Inventory.
6. Review and perform the required upgrades.

Verify that the files are complete and that their checksums match the values provided by Nutanix.

### 3. Dark Site / Direct Upload

Direct Upload allows administrators to upload approved LCM, software, and firmware bundles through the LCM interface.

Advantages:

* Does not require a dedicated local web server
* Suitable for air-gapped or restricted environments
* Provides control over the exact update bundles used
* Supports staged upgrade processes

Basic workflow:

1. Download approved bundles from the Nutanix Support Portal.
2. Select **Dark Site (Direct Upload)** in LCM Settings.
3. Upload the required LCM framework and compatibility bundles.
4. Upload the required software and firmware bundles.
5. Run Inventory.
6. Review dependencies, available updates, and prechecks.
7. Start the approved upgrade.

Only use bundles obtained from the Nutanix Support Portal or approved by Nutanix Support.

## LCM Inventory

Inventory is the discovery and assessment phase of LCM. It compares installed versions with available and supported versions.

### Inventory Workflow

1. Open LCM from Prism Element or Prism Central.
2. Confirm the update source.
3. Click **Perform Inventory**.
4. Wait for the inventory task to complete.
5. Review available updates and warnings.
6. Resolve critical precheck failures.
7. Select the approved updates.
8. Start and monitor the upgrade.
9. Run Inventory again after major upgrades when recommended.

## Recommended High-Level Upgrade Order

The exact sequence depends on the installed versions, hardware, hypervisor, and Nutanix products in use. Always follow the order recommended by LCM and the applicable Nutanix upgrade guide.

### Prism Central

1. Run LCM Inventory and update the LCM framework if recommended.
2. Upgrade and run NCC.
3. Upgrade Prism Central.
4. Verify that the upgrade completed successfully.
5. Run NCC again and resolve issues.
6. Run LCM Inventory again when required.

### Prism Element Cluster

1. Run LCM Inventory and update the LCM framework if recommended.
2. Upgrade and run NCC.
3. Upgrade Foundation, if applicable.
4. Upgrade Cluster Maintenance Utilities, if applicable.
5. Upgrade AOS.
6. Apply recommended firmware updates.
7. Upgrade AHV or another hypervisor, if applicable.
8. Upgrade additional Nutanix services or products as required.
9. Run NCC and perform a final LCM Inventory.

> Do not begin with firmware simply because a newer version is available. Follow the supported dependency and upgrade path shown by LCM.

## Pre-Upgrade Checklist

* Confirm the source and target versions are compatible.
* Run LCM Inventory.
* Run NCC and resolve critical issues.
* Check cluster health, capacity, alerts, and running tasks.
* Verify that all required bundles are present and valid.
* Confirm access to the configured update source.
* Check Prism Central, AOS, AHV, and firmware compatibility.
* Verify licensing and support entitlement requirements.
* Confirm backup and recovery procedures.
* Schedule an approved maintenance window.
* Identify workloads that cannot live migrate.

## During the Upgrade

* Monitor the LCM task and cluster health.
* Do not manually reboot nodes or interrupt the process unless instructed by Nutanix Support.
* Handle VMs with vGPU, affinity rules, or other migration limitations before host reboot operations.
* Keep relevant logs and support information available.

## Post-Upgrade Validation

* Confirm that the LCM task completed successfully.
* Verify AOS, AHV, Prism Central, NCC, and firmware versions.
* Run NCC and resolve remaining issues.
* Run LCM Inventory again when recommended.
* Verify VM availability and application functionality.
* Check cluster alerts, storage health, and overall environment status.

## Key Takeaways

* LCM provides inventory, compatibility validation, and upgrade orchestration.
* Software and firmware are different types of updates.
* LCM supports connected, local web server, and direct upload methods.
* Inventory should be completed before making upgrade decisions.
* The newest version is not always the correct version; use the supported upgrade path.
* NCC and compatibility checks are essential before and after upgrades.
* Always follow the current Nutanix documentation for the specific AOS, AHV, Prism Central, LCM, and hardware versions.

## References

* [LCM Inventory on PE and PC](https://confluence.eng.nutanix.com:8443/spaces/STK/pages/443220598/LCM+Inventory+on+PE+and+PC)
* [LCM Direct Upload](https://confluence.eng.nutanix.com:8443/spaces/STK/pages/528537408/WF+LCM+Direct+Upload)
* [LCM Dark Site Troubleshooting](https://confluence.eng.nutanix.com:8443/spaces/STK/pages/519580423/CS+LCM+Darksite+Troubleshooting)

## Disclaimer

This repository is intended for learning and reference. Upgrade procedures can vary by Nutanix release, hardware platform, and customer environment. Validate all steps against the latest official Nutanix documentation before performing an upgrade.
