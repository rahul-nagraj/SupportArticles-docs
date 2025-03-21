---
title: Copy-and-paste operations don't work when you connect to a Hyper-V virtual machine by using VMConnect Enhanced Session Mode
description: Describes an issue in which clipboard file copy redirection may not work as expected when you connect to a Hyper-V virtual machine by using VMConnect Enhanced Session Mode. Provides a resolution.
ms.date: 01/15/2025
manager: dcscontentpm
audience: itpro
ms.topic: troubleshooting
ms.reviewer: kaushika, stevepar, eltons, raviks
ms.custom:
- sap:virtualization and hyper-v\configuration of virtual machine settings
- pcy:WinComm Storage High Avail
---
# Copy-and-paste operations don't work when you connect to a Hyper-V virtual machine by using VMConnect Enhanced Session Mode

This article provides a solution to an issue where copy-and-paste operations may not work as expected when you connect to a Hyper-V virtual machine by using VMConnect Enhanced Session Mode.

_Original KB number:_ &nbsp; 4090037

## Symptoms

You can't copy and paste files between a system that is running [Virtual Machine Connection (VMConnect.exe)](/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect) and a Hyper-V guest virtual machine when you use Enhanced Session Mode through Remote Desktop Protocol (RDP).

## Cause

Enhanced Session Mode enables the transfer of files to and from virtual machines through the Clipboard copy-and-paste operations. However, copying and pasting files through the Clipboard is disabled if the **Do not allow drive redirection policy** option is enabled.

## Resolution

To successfully copy files to and from Hyper-V virtual machines when you use VMConnect through Enhanced Session Mode, make sure that you don't enable the following policy:

- Policy path: *Computer Configuration\\Policies\\Administrative Templates\\Windows Components\\Remote Desktop Services\\Remote Desktop Session Host\\Device and Resource Redirection*

    > [!NOTE]
    > If you use the Local Group Policy Editor, the *Policies* folder isn't part of the node path.

- Policy setting: **Do not allow Clipboard redirection**

    Set this policy to **Disabled** or **Not configured** to allow file copy redirection in Remote Desktop Services and Hyper-V Enhanced Session Mode sessions.

## References

- [Use local resources on Hyper-V virtual machine with VMConnect](/windows-server/virtualization/hyper-v/learn-more/use-local-resources-on-hyper-v-virtual-machine-with-vmconnect)
- [Policy CSP - RemoteDesktopServices](/windows/client-management/mdm/policy-csp-remotedesktopservices#remotedesktopservices-donotallowdriveredirection)
