---
title: Configure Windows Defender ATP endpoints using Mobile Device Management tools
description: Use Mobile Device Management tools to deploy the configuration package on endpoints so that they are onboarded to the service.
keywords: configure endpoints using mdm, endpoint management, configure Windows ATP endpoints, configure Windows Defender Advanced Threat Protection endpoints, mdm
search.product: eADQiWindows 10XVcnh
ms.prod: w10
ms.mktglfcycl: deploy
ms.sitesec: library
ms.pagetype: security
author: mjcaparas
localizationpriority: high
---

# Configure endpoints using Mobile Device Management tools

**Applies to:**

- Windows 10 Enterprise
- Windows 10 Education
- Windows 10 Pro
- Windows 10 Pro Education
- Windows Defender Advanced Threat Protection (Windows Defender ATP)

You can use mobile device management (MDM) solutions to configure endpoints. Windows Defender ATP supports MDMs by providing OMA-URIs to create policies to manage endpoints.

For more information on using Windows Defender ATP CSP see, [WindowsAdvancedThreatProtection CSP](https://msdn.microsoft.com/en-us/library/windows/hardware/mt723296(v=vs.85).aspx) and [WindowsAdvancedThreatProtection DDF file](https://msdn.microsoft.com/en-us/library/windows/hardware/mt723297(v=vs.85).aspx).

## Configure endpoints using Microsoft Intune

For more information on using Windows Defender ATP CSP see, [WindowsAdvancedThreatProtection CSP](https://msdn.microsoft.com/en-us/library/windows/hardware/mt723296(v=vs.85).aspx) and [WindowsAdvancedThreatProtection DDF file](https://msdn.microsoft.com/en-us/library/windows/hardware/mt723297(v=vs.85).aspx).

### Onboard and monitor endpoints

1. Open the Microsoft Intune configuration package .zip file (*WindowsDefenderATPOnboardingPackage.zip*) that you downloaded from the service onboarding wizard. You can also get the package from the [Windows Defender ATP portal](https://securitycenter.windows.com/):

    a.  Click **Endpoint Management** on the **Navigation pane**.

    b.  Select **Mobile Device Management/Microsoft Intune**, click **Download package** and save the .zip file.

2. Extract the contents of the .zip file to a shared, read-only location that can be accessed by the network administrators who will deploy the package. You should have a file named  *WindowsDefenderATP.onboarding*.

3. Use the Microsoft Intune custom configuration policy to deploy the following supported OMA-URI settings. For more information on Microsoft Intune policy settings see, [Windows 10 policy settings in Microsoft Intune](https://docs.microsoft.com/en-us/intune/deploy-use/windows-10-policy-settings-in-microsoft-intune).

Onboarding - Use the onboarding policies to deploy configuration settings on endpoints. These policies can be sub-categorized to:
- Onboarding
- Health Status for onboarded machines
- Configuration for onboarded machines

Policy | OMA-URI | Type | Value | Description
:---|:---|:---|:---|:---
Onboarding | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/Onboarding | String | Copy content from onboarding MDM file |  Onboarding
Health Status for onboarded machines | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/HealthState/SenseIsRunning | Boolean | TRUE |  Windows Defender ATP service is running
 | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/HealthState/OnBoardingState | Integer | 1 | Onboarded to Windows Defender ATP
  | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/HealthState/OrgId | String | Use OrgID from onboarding file | Onboarded to Organization ID
 Configuration for onboarded machines  | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/Configuration/SampleSharing | Integer | 0 or 1 <br> Default value: 1 | Windows Defender ATP Sample sharing is enabled


> [!NOTE]
> The **Health Status for onboarded machines** policy uses read-only properties and can't be remediated.

### Offboard and monitor endpoints

For security reasons, the package used to offboard endpoints will expire 30 days after the date it was downloaded. Expired offboarding packages sent to an endpoint will be rejected. When downloading an offboarding package you will be notified of the packages expiry date and it will also be included in the package name.

> [!NOTE]
> Onboarding and offboarding policies must not be deployed on the same endpoint at the same time, otherwise this will cause unpredictable collisions.

1.	Get the offboarding package from the [Windows Defender ATP portal](https://securitycenter.windows.com/):

    a. Click **Endpoint Management** on the **Navigation pane**.

    b. Under **Endpoint offboarding** section, select **Mobile Device Management /Microsoft Intune**, click **Download package** and save the .zip file.

2.	Extract the contents of the .zip file to a shared, read-only location that can be accessed by the network administrators who will deploy the package. You should have a file named *WindowsDefenderATP_valid_until_YYYY-MM-DD.offboarding*.

3. Use the Microsoft Intune custom configuration policy to deploy the following supported OMA-URI settings. For more information on Microsoft Intune policy settings see, [Windows 10 policy settings in Microsoft Intune](https://docs.microsoft.com/en-us/intune/deploy-use/windows-10-policy-settings-in-microsoft-intune).

Offboarding - Use the offboarding policies to remove configuration settings on endpoints. These policies can be sub-categorized to:
- Offboarding
- Health Status for offboarded machines
- Configuration for offboarded machines

Policy | OMA-URI | Type | Value | Description
:---|:---|:---|:---|:---
Offboarding | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/Offboarding | String | Copy content from offboarding MDM file | Offboarding
 Health Status for offboarded machines | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/HealthState/SenseIsRunning | Boolean | FALSE |Windows Defender ATP service is not running
  | ./Device/Vendor/MSFT/WindowsAdvancedThreatProtection/HealthState/OnBoardingState | Integer | 0 | Offboarded from Windows Defender ATP

> [!NOTE]
> The **Health Status for offboarded machines** policy uses read-only properties and can't be remediated.


## Related topics
- [Configure endpoints using Group Policy](configure-endpoints-gp-windows-defender-advanced-threat-protection.md)
- [Configure endpoints using System Center Configuration Manager](configure-endpoints-sccm-windows-defender-advanced-threat-protection.md)
- [Configure endpoints using a local script](configure-endpoints-script-windows-defender-advanced-threat-protection.md)
- [Troubleshoot Windows Defender Advanced Threat Protection onboarding issues](troubleshoot-onboarding-windows-defender-advanced-threat-protection.md)
