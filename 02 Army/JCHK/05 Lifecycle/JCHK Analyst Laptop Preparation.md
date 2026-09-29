---
tags: [jchk, lifecycle, workstations, deployment]
source_version: "0.6.1"
---
# JCHK Analyst Laptop Preparation

The JCHK analyst laptop is both a user's workstation and, for the designated deployment laptop, the host of the Provisioner virtual machine. A **host** is the physical computer supplying resources to a **virtual machine**, or VM, which behaves like a separate computer in software. Preparing the laptop gives the rest of the kit a consistent starting point.

The full kit has nine laptops. Eight are shown at the Analyst Site; one is shown as the deployment laptop at Hunt Site 1. Do not confuse preparing all nine laptops with placing all nine permanently at the Analyst Site. See [[JCHK Start Here]] and [[JCHK Deployment Roadmap]]. [[JCHK Sources#V2|Volume 2]], PDF pp. 18, 44–45, 79–81.

## What an image installation does

An **image** is a packaged operating-system installation with an expected software baseline. The supplied custom Windows 11 **ISO** is a disk-image file used to create bootable installation media. **Bootable** means the computer can start from that device before loading its existing OS. **Rufus** is the Windows utility used in the guide to write the ISO to a USB drive.

> [!danger] Two different devices can be erased
> Creating the installation USB overwrites the selected USB drive. Installing the custom image wipes the laptop. Identify both targets and preserve needed data before starting either operation. These procedures are initial-build/rebuild procedures, not normal startup steps.

The stated prerequisites are the approved custom ISO, Rufus, a USB drive of at least 32 GB that may be erased, the laptop's AC adapter, stable power, and the firmware settings in [[JCHK Hardware Baselines]]. Keep the laptop on AC power throughout imaging. [[JCHK Sources#V2|Volume 2]], PDF pp. 18–23.

## Preparing the installation media

The guide's workflow is:

1. Open Rufus on a Windows host and decline the update prompt in the documented build workflow.
2. Insert the intended USB drive. Reveal advanced drive properties and enable the option that lists USB hard drives if needed.
3. Select the target USB device and the approved JCHK ISO.
4. Confirm Rufus's intended defaults for this image; the guide instructs the operator to deselect optional Windows User Experience modifications.
5. Start writing only after checking the target device. Wait until Rufus reports `READY`, then safely eject it.

**Safely eject** means asking the OS to finish outstanding writes before physically removing the drive. The media-creation estimate is about 20–25 minutes. The document mentions setting a DataLocker aside for `BOOT` mode; when that particular encrypted drive is used, its own operating instructions still apply. The general procedure also accepts an appropriate USB drive. [[JCHK Sources#V2|Volume 2]], PDF pp. 18–22.

## Installing and verifying Windows

Shut down the target laptop, connect AC power, insert the bootable drive, start the laptop, and use `F12` for the one-time boot menu. Select the intended USB device under UEFI boot devices. **UEFI** is the modern firmware environment that starts the operating system.

The guide describes an automated install with several reboots, taking roughly 45 minutes. Keep the USB installed until the process completes. When Windows starts, use the currently assigned login information; these notes intentionally do not reproduce the training/default passwords printed in the source. A reboot during this process can be expected, so do not assume every restart is a failure. [[JCHK Sources#V2|Volume 2]], PDF p. 23.

A prepared laptop should boot cleanly, accept its intended user login, and have any mission-required optional tools installed. A successful Windows installation alone does not prove that the laptop can reach kit services. Network addressing, trusted certificates, and time synchronization are completed later in [[JCHK Post-Deployment Readiness]]. [[JCHK Sources#V2|Volume 2]], PDF pp. 29, 84–86.

## Optional application setup

Volume 2 explicitly says these applications are optional for deployment and may be installed later. Requirements depend on the mission and available licenses.

| Application | What it contributes | Documented setup implication |
|---|---|---|
| Microsoft Office LTSC and Visio Professional 2024 | Documents, spreadsheets, presentations, and diagrams. LTSC means Long-Term Servicing Channel. | Use the supplied setup files and `config.xml`; apply allocated product keys and activate. The guide describes internet activation and notes telephone activation is possible. |
| PE Explorer | Inspects the structure and resources of Windows executable files. “PE” means Portable Executable. | Run the supplied installer, place the supplied license file appropriately, and enter registration information. |
| JupyterLab | Combines explanatory text, code, and results in notebook documents. | Its bundled Python-environment setup needs internet access according to this guide; use can be offline afterward. |
| NetworkMiner Professional | Extracts and organizes information from captured network traffic. | The image includes the free version. Installing Professional requires the supplied archive, its approved password, and replacing the documented application folder. |

For Office/Visio the source command `setup.exe /configure config.xml` runs the supplied installer with the named configuration file. It changes installed software; it is not a status check. `cd` merely changes the terminal's working directory. The source gives a 29-day period before Office activation is needed; treat this as a statement about the supplied 2024 package, not a current general licensing rule. [[JCHK Sources#V2|Volume 2]], PDF pp. 23–29.

## Returning the network adapter to normal

A laptop used for [[JCHK Switch Initialization]] temporarily uses a manually assigned address in the `169.254.0.0/16` range. After switch configuration, the guide instructs the operator to restore **DHCP**, Dynamic Host Configuration Protocol, so the network can assign the laptop its expected address. Leaving the temporary address in place can make a healthy kit appear unreachable. [[JCHK Sources#V2|Volume 2]], PDF pp. 30–32, 41, 64.

If the USB does not appear or the installer will not start, begin with seating, another USB port, the one-time boot menu, known-good media, and the firmware baseline. Recreating installation media is a separate erasing operation, so verify the USB target again. [[JCHK Sources#V2|Volume 2]], PDF p. 143.

Related: [[JCHK JAKD Deployment]] · [[JCHK Post-Deployment Readiness]] · [[JCHK Troubleshooting and Recovery]]
