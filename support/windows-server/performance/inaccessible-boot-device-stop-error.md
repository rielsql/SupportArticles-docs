---
title: Error 0x7B after you reconfigure hardware devices
description: Discusses that you receive a Stop error (INACCESSIBLE_BOOT_DEVICE) after you reconfigure critical hardware devices.
ms.date: 09/08/2020
author: delhan
ms.author: Delead-Han
manager: dscontentpm
audience: itpro
ms.topic: troubleshooting
ms.prod: windows-server
localization_priority: medium
ms.reviewer: kaushika
ms.prod-support-area-path: Blue Screen/Bugcheck
ms.technology: Performance
---
# "INACCESSIBLE_BOOT_DEVICE" Stop error code after you reconfigure critical hardware devices

This article helps fix the Stop error code 0x00000007B that occurs after you reconfigure critical hardware devices.

_Original product version:_ &nbsp; Windows Server 2012 R2, Windows 10 - all editions  
_Original KB number:_ &nbsp; 3024175

## Symptoms

Consider the following scenario:

- You are using a computer that is running Windows Server 2012 R2, Windows 8.1, Windows Server 2012, or Windows 8.
- The operating system includes two drivers that are configured to load at startup. For example, the system includes driverA.sys and driverB.sys, and you expect both to load at startup.
- When the computer starts, driverA.sys is used in the boot-up process but driverB.sys is not. Therefore, the system reconfigures the Start Type setting for driverB.sys from Boot Start to Demand Start.
- You change the computer hardware configuration. For example, you replace the PCI slot on an SAS controller to which the boot-up hard disk drive is attached. After you make this change, a device that is required to start the machine may require driverB.sys, which is now set to Demand Start instead of Boot Start.

In this scenario, the system does not load driverB.sys, and STOP Error code 0x00000007B is generated. When this occurs, you receive the following error message:
INACCESSIBLE_BOOT_DEVICE (Stop 0x7B)

## Cause

This problem occurs because the system does not support a change in the hardware configuration of devices that are boot critical.

The system has a built-in feature to reduce resource consumption by not loading unnecessary drivers. This feature determines whether a specific driver is set to load automatically at system startup but is not required during startup. If the driver is not required at startup, the system may change the driver to a start type of Demand Start.

Because of this feature, drivers that are necessary to start the system under alternative hardware configurations may be set to Demand Start. If the hardware configuration of the system changes, the system may not find the volume that is necessary to start the system. If this occurs, the Stop error that is mentioned in the "Symptoms" section is generated.

## Resolution

To resolve this issue, restart the system in safe mode. In safe mode, any change to a driver's Start Type setting is reversed, and the values are re-created.

For more information about how to start in safe mode, see the following Microsoft Knowledge Base article:

[2809468](https://support.microsoft.com/help/2809468) How to easily enter safe mode in windows 8? (One-click series)

## More information

For more information about how to troubleshoot Stop error 0x7B, go to the following Server and Tools Team blog article:

[Troubleshooting a Stop 0x7B in Windows](https://blogs.technet.com/b/askcore/archive/2013/08/05/troubleshooting-a-stop-0x7b-in-windows.aspx) 

## Status

This behavior is by design.