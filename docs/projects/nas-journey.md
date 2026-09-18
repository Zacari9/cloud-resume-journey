# Home NAS: From ZimaOS to Unraid

**Status:** In use / evolving  
**Build period:** August–September 2026  
**Documentation added:** September 18, 2026  
**Environment:** HP EliteDesk, Unraid, Docker, USB-attached storage, Windows and macOS clients

## Overview

I expanded my existing home lab into a NAS for shared files, photo storage, Windows backups, and macOS Time Machine backups. The project also involved moving my existing Palworld server and management dashboard from ZimaOS to Unraid.

My initial budget target was under $500, using the HP EliteDesk I already owned. This is a record of the build and troubleshooting journey, rather than a claim that every planned resilience feature has been completed.

## Hardware and Storage

| Component | Role in the build |
| --- | --- |
| HP EliteDesk | Host for the NAS and Docker services |
| 16 GB Verbatim USB drive | Unraid boot device |
| Internal 256 GB Samsung NVMe SSD | Original ZimaOS installation; retained during migration for rollback |
| Samsung T5 500 GB SSD | Single-device Btrfs cache pool for Docker and application data |
| Four-bay USB drive enclosure | Expansion for NAS hard drives |
| Three 4 TB Seagate hard drives | Two array data disks and one dedicated parity disk |
| Deco mesh network | Home network used by NAS clients |

The array started with one 4 TB data disk and no parity. As of September 18, 2026, my setup has three 4 TB Seagate drives: two data disks and one dedicated parity disk. That provides 8 TB of nominal data capacity from 12 TB of raw disk capacity, before filesystem overhead.

## 1. Migrating Without Losing the Existing Setup

On August 16, I booted Unraid headlessly from USB while leaving the original ZimaOS NVMe installation intact. That preserved a rollback option while I brought up the replacement environment.

I mounted the old ZimaOS data partition read-only and migrated application data and Compose configuration. The migration work used `rsync` with metadata-preserving options and a checksum-based dry run to compare source and destination. These checks addressed the copied application data; they were not a full backup restore test.

I adapted the container configuration for Unraid, validated the Compose configuration, and started the Palworld server and dashboard. A restart initiated through the dashboard worked after migration.

### Troubleshooting: Docker Would Not Start

The new cache pool existed, but the `system` and `appdata` shares were missing. After creating those shares on the cache pool, Docker started.

**What I learned:** Bringing up the storage device is only part of the setup. Services also depend on the expected paths, shares, and persistent application data.


## 2. Building Shared Storage and Client Backups

After migrating services, I configured XFS array storage and SMB shares with user access controls.

The NAS work included:

- Windows backup configuration using Veeam, including an entire-computer backup workflow.
- macOS Time Machine backup shares over SMB.
- A separate 1 TB Time Machine allocation for a family member's Mac mini.
- Centralized file and photo storage, including RAW photographs used in my editing workflow.

These are distinct uses of the NAS: some shares hold copies of client data, while others hold working files. A file moved onto the NAS is not automatically backed up simply because it is on shared storage.

**Validation boundary:** Backup configuration and use are documented, but a successful bare-metal Windows recovery or complete Mac restore is not established here.

## 3. Troubleshooting the Network

Large transfers and backups made network stability more noticeable. Running new Ethernet cabling was not practical in my setup, so I worked within the existing Deco mesh network.

Troubleshooting included node placement, roaming behavior, power settings, and transfer load. Moving a mesh node and disabling roaming for the affected connection improved the reported connectivity issue.

**What I learned:** Storage performance depends on the entire path between the client and the disks. A slow or interrupted copy does not, by itself, identify a drive problem.


## 4. Checking Drive Health and Planning Expansion

I worked with SMART health checks, including a short test and investigation of extended testing. The available record does not establish a final extended-test result, so I am not treating that as a completed validation milestone.

The build expanded from a single data disk to the current three-drive configuration:

| Disk layout | Nominal data capacity | Status in this write-up |
| --- | --- | --- |
| One 4 TB data disk, no parity | 4 TB | Initial documented configuration |
| Two 4 TB data disks + one 4 TB parity disk | 8 TB | Current configuration, confirmed September 18, 2026 |
| Three 4 TB data disks + one 4 TB parity disk | 12 TB | Later expansion plan |

Capacities are nominal decimal totals before filesystem overhead. Parity is intended to support recovery from a disk failure; it does not replace separate backups of important files.

## Results and Skills Practiced

The project brought NAS storage and existing container services onto Unraid and added backup workflows for both Windows and macOS.

Practical skills included:

- Headless Linux-based server administration.
- OS migration with an original installation retained for rollback.
- Read-only mounts, file migration, and comparison of copied data.
- Docker persistence, Compose configuration, and service troubleshooting.
- XFS array storage and a Btrfs cache pool.
- SMB shares, authentication, and user permissions.
- Windows and macOS backup configuration.
- Wi-Fi troubleshooting and disk-health investigation.
- Capacity planning and separating redundancy from backup.

## Next Steps

- Record parity synchronization/check results for the current three-drive array.
- Test recovery of representative files from both backup workflows, then document broader recovery testing.

## Related Project

[Dedicated Palworld Server](palworld-server.md) — the existing container workload migrated as part of this build.

## Documentation Note

This retrospective was assembled from my build discussions and troubleshooting history with assistance from ChatGPT. Completed work, reported outcomes, and unverified plans are separated above. It is not a live audit of the server.

