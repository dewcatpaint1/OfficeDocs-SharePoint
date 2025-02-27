---
title: "Manage Loop components in SharePoint"
ms.reviewer: trhoan
ms.author: mikeplum
author: MikePlumleyMSFT
manager: serdars
recommendations: true
audience: Admin
f1.keywords:
- NOCSH
ms.service: sharepoint-online
ms.localizationpriority: medium
ms.topic: article
ms.collection:  
- Strat_SP_admin
- Microsoft 365-collaboration
search.appverid:
- SPO160
- MET150
description: "Learn how to manage Loop components by using PowerShell."
---

# Manage Loop components in SharePoint

Loop experiences on Microsoft 365 OneDrive or SharePoint are backed by .fluid files and powered by Microsoft Fluid Framework. Administrators need to manage access to Loop experiences from SharePoint and not from the Microsoft Teams admin center.

## Loop service requirements

Loop's near real-time communications are enabled by the core services that run a WebSocket server. Coauthors in the same session need to establish secured WebSocket connections to this service to send and receive collaborative data such as changes made by others, live cursors, presence, etc. These experiences are crucial to Loop, and all the scenarios powered by Fluid framework. So at the minimum, WebSocket will need to be unblocked from the user's endpoint.

Just like other Microsoft 365 experiences, Loop also leverages core services across SharePoint and Microsoft 365. To effectively enable Loop experiences or OneDrive and SharePoint files-backed experiences powered by Fluid Framework, follow the instructions in [Office 365 URLs and IP address ranges](/microsoft-365/enterprise/urls-and-ip-address-ranges) to ensure connections to Loop services.

## Settings management

You'll need the latest version of SharePoint PowerShell module to enable or disable all Fluid Experiences across your Microsoft 365 organization. Microsoft Fluid Framework defaults to ON for all targeted release organizations. Because Loop components are designed for collaboration, the components are always shared as editable by others, even if your organization is set to default to view-only for other file types. See the Learn more link next to the setting for more details.

|Experience|SharePoint organization properties|Notes|
|:---------|:---------------------------------|:----|
|**All Microsoft 365 experiences** powered by Fluid Framework|`IsFluidEnabled` (boolean)|This core property controls all other experiences powered by Fluid Framework. Setting it to `False` will effectively disable all experiences (everything in this table) in the organization powered by Fluid Framework.|
|Loop components in Teams|n/a|there is no setting for disabling only Loop components in Teams at this time, you must use the core property above.|
|Microsoft Whiteboard on OneDrive|`IsWBFluidEnabled` (boolean) |only applies when `IsFluidEnabled` is `True`|
|Microsoft OneNote collaborative Meeting notes|`IsCollabMeetingNotesEnabled` (boolean)|only applies when `IsFluidEnabled` is `True`|

To check your tenant's default file permissions
1.	Go to the [Microsoft 365 admin center](https://admin.microsoft.com).
2.	Under Admin centers, select **SharePoint**.
3.	Select **Policies** > **Sharing**, and under **File and folder links**, view your organization's default file permissions.

To check if the Fluid Framework is enabled, run `Get-SPOTenant` without any arguments. Verify the value of IsFluidEnabled is true.

To enable the Fluid Framework, run `Set-SPOTenant -IsFluidEnabled $true`. The change will take a short time to apply across your organization. 

The feature will be available on Teams Windows Desktop, Mac, iOS, Android. When enabled, users will see a new option for inserting Loop components in the message compose experience for these clients.

To disable Fluid Framework, run `Set-SPOTenant cmdlet Set-SPOTenant -IsFluidEnabled $false`. The change will take a short time to apply across your organization. 

## Related topics

[Overview of Loop components in Teams](/microsoftteams/live-components-in-teams)
