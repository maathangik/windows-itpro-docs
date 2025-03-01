---
title: Update Compliance Configuration Script
ms.reviewer: 
manager: dougeby
description: Downloading and using the Update Compliance Configuration Script
keywords: update compliance, oms, operations management suite, prerequisites, requirements, updates, upgrades, antivirus, antimalware, signature, log analytics, wdav
ms.prod: w10
ms.mktglfcycl: deploy
ms.pagetype: deploy
audience: itpro
author: aczechowski
ms.author: aaroncz
ms.localizationpriority: medium
ms.collection: M365-analytics
ms.topic: article
---

# Configuring devices through the Update Compliance Configuration Script

**Applies to**

- Windows 10
- Windows 11

> [!NOTE]
> A new policy is required to use Update Compliance: "AllowUpdateComplianceProcessing." If you're already using Update Compliance and have configured your devices prior to May 10, 2021, you must rerun the script so the new policy can be configured.

The Update Compliance Configuration Script is the recommended method of configuring devices to send data to Microsoft for use with Update Compliance. The script configures the registry keys backing policies, ensures required services are running, and more. This script is a recommended complement to configuring the required policies documented in [Manually configured devices for Update Compliance](update-compliance-configuration-manual.md), as it can provide feedback on whether there are any configuration issues outside of policies being configured. 

> [!NOTE]
> The configuration script configures registry keys directly. Registry keys can potentially be overwritten by policy settings like Group Policy or MDM. *Reconfiguring devices with the script does not reconfigure previously set policies, both in the case of Group Policy and MDM*. If there are conflicts between your Group Policy or MDM configurations and the required configurations listed in [Manually configuring devices for Update Compliance](update-compliance-configuration-manual.md), device data might not appear in Update Compliance correctly. 

You can download the script from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=101086). Keep reading to learn how to configure the script and interpret error codes that are output in logs for troubleshooting.

## How this script is organized

This script's two primary files are `ConfigScript.ps1` and `RunConfig.bat`. You configure `RunConfig.bat` according to the directions in the `.bat` itself, which will then run `ConfigScript.ps1` with the parameters entered to `RunConfig.bat`. There are two ways of using the script: in **Pilot** mode or **Deployment** mode. 

- In **Pilot** mode (`runMode=Pilot`), the script will enter a verbose mode with enhanced diagnostics, and save the results in the path defined with `logpath` in `RunConfig.bat`. Pilot mode is best for a pilot run of the script or for troubleshooting configuration.
- In **Deployment** mode (`runMode=Deployment`), the script will run quietly. 


## How to use this script

Open `RunConfig.bat` and configure the following (assuming a first-run, with `runMode=Pilot`):

1. Define `logPath` to where you want the logs to be saved. Ensure that `runMode=Pilot`.
2. Set `commercialIDValue` to your Commercial ID.
3. Run the script.
4. Examine the logs for any issues. If there are no issues, then all devices with a similar configuration and network profile are ready for the script to be deployed with `runMode=Deployment`.
5. If there are issues, gather the logs and provide them to Support.


## Script errors

|Error  |Description  |
|---------|---------|
| 1    | General unexpected error| 
| 6    | Invalid CommercialID| 
| 8    | Couldn't create registry key path to setup CommercialID| 
| 9    | Couldn't write CommercialID at registry key path| 
| 11    | Unexpected result when setting up CommercialID.| 
| 12    | CheckVortexConnectivity failed, check Log output for more information.| 
| 12    | Unexpected failure when running CheckVortexConnectivity.| 
| 16    | Reboot is pending on device, restart device and restart script.| 
| 17    | Unexpected exception in CheckRebootRequired.| 
| 27    | Not system account. |
| 30    | Unable to disable Enterprise Auth Proxy. This registry value must be 0 for UTC to operate in an authenticated proxy environment.| 
| 34    | Unexpected exception when attempting to check  Proxy settings.| 
| 35    | Unexpected exception when checking User Proxy.| 
| 37    | Unexpected exception when collecting logs| 
| 40    | Unexpected exception when checking and setting telemetry.| 
| 41    | Unable to impersonate logged-on user.| 
| 42    | Unexpected exception when attempting to impersonate logged-on user.| 
| 43    | Unexpected exception when attempting to impersonate logged-on user.| 
| 44    | Error when running CheckDiagTrack service.| 
| 45    | DiagTrack.dll not found.| 
| 48    | CommercialID is not a GUID| 
| 50    | DiagTrack service not running.| 
| 51    | Unexpected exception when attempting to run Census.exe| 
| 52    | Could not find Census.exe| 
| 53    | There are conflicting CommercialID values.| 
| 54    | Microsoft Account Sign In Assistant (MSA) Service disabled.| 
| 55    | Failed to create new registry path for SetDeviceNameOptIn| 
| 56    | Failed to create property for SetDeviceNameOptIn at registry path| 
| 57    | Failed to update value for SetDeviceNameOptIn| 
| 58    | Unexpected exception in SetrDeviceNameOptIn| 
| 59    | Failed to delete LastPersistedEventTimeOrFirstBoot property at registry path when attempting to clean up OneSettings.| 
| 60    | Failed to delete registry key when attempting to clean up OneSettings.| 
| 61    | Unexpected exception when attempting to clean up OneSettings.| 
| 62    | AllowTelemetry registry key is not of the correct type REG_DWORD| 
| 63    | AllowTelemetry is not set to the appropriate value and it could not be set by the script.| 
| 64    | AllowTelemetry is not of the correct type REG_DWORD.| 
| 66    | Failed to verify UTC connectivity and recent uploads.|  
| 67    | Unexpected failure when verifying UTC CSP.| 
| 91    | Failed to create new registry path for EnableAllowUCProcessing| 
| 92    | Failed to create property for EnableAllowUCProcessing at registry path| 
| 93    | Failed to update value for EnableAllowUCProcessing| 
| 94    | Unexpected exception in EnableAllowUCProcessing| 
| 99    | Device is not Windows 10.| 
