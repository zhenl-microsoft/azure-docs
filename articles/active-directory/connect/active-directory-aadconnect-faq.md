---
title: 'Azure Active Directory Connect: FAQ - | Microsoft Docs'
description: This page has frequently asked questions about Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: billmath

---
# Frequently asked questions for Azure Active Directory Connect

## General installation
**Q: Will installation work if the Azure AD Global Admin has 2FA enabled?**  
With the builds from February 2016, this is supported.

**Q: Is there a way to install Azure AD Connect unattended?**  
It is only supported to install Azure AD Connect using the installation wizard. An unattended and silent installation is not supported.

**Q: I have a forest where one domain cannot be contacted. How do I install Azure AD Connect?**  
With the builds from February 2016, this is supported.

**Q: Does the AD DS health agent work on server core?**  
Yes. After installing the agent, you can complete the registration process using the following PowerShell cmdlet: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**Q: Does AADConnect support syncing from two domains to on Azure AD?**</br>
Yes, this is supported. Refer to [Multiple Domains](active-directory-aadconnect-multiple-domains.md)
 
**Q: Do we support having multiple connectors for same Active Directory domain in Azure AD connect?**</br> 
No, this is not supported 

## Network
**Q: I have a firewall, network device, or something else that limits the maximum time connections can stay open on my network. How long should my client side timeout threshold be when using Azure AD Connect?**  
All networking software, physical devices, or anything else that limits the maximum time connections can remain open should use a threshold of at least 5 minutes (300 seconds) for connectivity between the server where the Azure AD Connect client is installed and Azure Active Directory. This also applies to all previously released Microsoft Identity synchronization tools.

**Q: Are SLDs (Single Label Domains) supported?**  
No, Azure AD Connect does not support on-premises forests/domains using SLDs.

**Q: Are Forests with disjoint AD domains supported?**  
No, Azure AD Connect does not support on-premises forests containing disjoint namespaces.

**Q: Are "dotted" NetBios named supported?**  
No, Azure AD Connect does not support on-premises forests/domains where the NetBios name contains a period "." in the name.

**Q: Is pure IPv6 environment supported?**  
No, Azure AD Connect does not support pure IPv6 environment.

## Federation
**Q: What do I do if I receive an email that asking me to renew my Office 365 certificate**  
Use the guidance that is outlined in the [renew certificates](active-directory-aadconnect-o365-certs.md) topic on how to renew the certificate.

**Q: I have "Automatically update relying party" set for O365 relying party. Do I have to take any action when my token signing certificate automatically rolls over?**  
Use the guidance that is outlined in the article [renew certificates](active-directory-aadconnect-o365-certs.md).

## Environment
**Q: Is it supported to rename the server after Azure AD Connect has been installed?**  
No. Changing the server name will cause the sync engine to not be able to connect to the SQL database and the service will not be able to start.

## Identity data
**Q: The UPN (userPrincipalName) attribute in Azure AD does not match the on-prem UPN - why?**  
See these articles:

* [User names in Office 365, Azure, or Intune don't match the on-premises UPN or alternate login ID](https://support.microsoft.com/en-us/kb/2523192)
* [Changes aren't synced by the Azure Active Directory Sync tool after you change the UPN of a user account to use a different federated domain](https://support.microsoft.com/en-us/kb/2669550)

You can also configure Azure AD to allow the sync engine to update the userPrincipalName as described in [Azure AD Connect sync service features](active-directory-aadconnectsyncservice-features.md).

**Q: Is it supported to soft match on-premises AD Group/Contact objects with existing Azure AD Group/Contact objects?**  
Yes, this will be based on the proxyAddress.  Soft matching is not supported for groups that are not mail-enabled.

**Q: Is it supported to manually set ImmutableId attribute on existing Azure AD Group/Contact objects to hard match it to on-premises AD Group/Contact objects?**  
No, this is currently not supported.

## Custom configuration
**Q: Where are the PowerShell cmdlets for Azure AD Connect documented?**  
With the exception of the cmdlets documented on this site, other PowerShell cmdlets found in Azure AD Connect are not supported for customer use.

**Q: Can I use "Server export/server import" found in *Synchronization Service Manager* to move configuration between servers?**  
No. This option will not retrieve all configuration settings and should not be used. You should instead use the wizard to create the base configuration on the second server and use the sync rule editor to generate PowerShell scripts to move any custom rule between servers. See [Swing migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**Q: Can passwords be cached for the Azure sign-in page and can this be prevented since it contains a password input element with the autocomplete = "false" attribute?**</br>
We currently do not support modifying the HTML attributes of the Password input field, including the autocomplete tag. We are currently working on a feature that will allow for custom Javascript which will allow you to add any attribute to the password field. This should be available later part of 2017.

**Q: On the Azure sign-in page, usernames for users who have previously signed in successfully are shown.  Can this behavior be turned off?**</br>
We currently do not support modifying the HTML attributes of the sign-in page. We are currently working on a feature that will allow for custom Javascript which will allow you to add any attribute to the password field. This should be available later part of 2017.

**Q: Is there a way to prevent concurrent sessions?**</br>
No.

## Auto upgrade

**Q: What are the advantages and consequences of using auto upgrade?**</br>
We advise all customers to enable auto upgrade for their Azure AD Connect installation. The benefits are that they will always receive the latest patches, including security updates for vulnerabilities that we found in Azure AD Connect. The upgrade process is painless and will happen automatically as soon as a new version is available. We serve many thousands of Azure AD Connect customers through auto upgrade with every new release.

The auto upgrade process will always first establish whether an installation is eligible for auto upgrade (this includes looking for custom changes to rules, specific environmental factors etc.) and if so, the upgrade is performed and tested. If the tests show that an upgrade was not successful, the previous version will automatically get restored.

Depending on the size of the environment, the process can take a couple of hours, and while the upgrade happens, no sync between Windows Server AD and Azure AD will happen.

**Q: I received an email telling me that my auto upgrade no longer works and I need to install new version. Why do I need to do this?**</br>
Last year we released a version of Azure AD Connect that, under certain circumstances, might have disabled the auto upgrade feature on your server. We fixed this issue in Azure AD Connect version 1.1.750.0, which was released at the end of last month. We need customers who may have been affected by this issue to manually upgrade to the latest version of Azure AD Connect to mitigate the problem. To manually upgrade, you must download and run the latest version of the AADConnect.msi file.
 
-  If your current version is older than 1.1.750.0, you must upgrade to the latest version, [which can be downloaded here](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
- If your Azure AD Connect version is 1.1.750.0 or newer, you do not have to take any action to mitigate the auto upgrade issue, as you’re already on the version that has a fix for this. 

**Q: I received an email telling me to upgrade to the latest version to re-enable auto upgrade. I am on 1.1.654.0, do I need to upgrade?** </br>	
Yes, you need to upgrade to 1.1.750 or newer to re-enable auto upgrade. Here is the link that explains how to upgrade to a newer version

**Q: I received an email telling me to upgrade to the latest version to re-enable auto upgrade. I have used PowerShell to enable auto-upgrade, do I still need to install the latest version?**</br>	
Yes, you still need to upgrade to version 1.1.750.0 or newer. Enabling the auto upgrade service with PowerShell does not mitigate the auto upgrade issue found in versions before 1.1.750

**Q: I want to upgrade to a newer version but I’m not sure who installed Azure AD Connect and we do not have the username and password.  Do we need this?**</br>
You don’t need to know the username and password that was initially used to upgrade Azure AD Connect – any Azure AD account that has the Global Administrator role can be used.

**Q: How can I find which version of Azure AD Connect I am on?**</br>	
To verify which version of Azure AD Connect is installed on your server, please go to the Control Panel and look up the installed version of Microsoft Azure AD Connect in “Programs > Programs and Features”:

![version](media/active-directory-aadconnect-faq/faq1.png)

**Q: How do I perform an upgrade to the latest version of AADConnect?**</br>	
This [article](active-directory-aadconnect-upgrade-previous-version.md) explains how to upgrade to a newer version. 

**Q: We already upgraded to the latest version of AADConnect last year, do we need to upgrade again?**</br>	
The Azure AD Connect teams makes frequent updates to the service, and it is important that your server is up to date with the latest version to benefit from bug fixes and security updates as well as new features. If you enable auto upgrade your software version will be updated automatically. To find the version release history of Azure AD Connect, please follow this [link](active-directory-aadconnect-version-history.md).

**Q: How long will it take to perform the upgrade and what is the impact on my users?**</br>	
The time needed to upgrade depends on your tenant size and for larger organizations it may be best to do this in the evening or weekend. Note that during the upgrade no synchronization activity takes place.

**Q: I believe I upgraded to AADConnect but in the Office portal it still mentions DirSync.  Wny is this?**</br>	
The Office team is working to get the Office portal updates to reflect the current product name – it does not reflect which sync tool you are using.

**Q: I checked my Auto-Upgrade status and it says “suspended ". Why is it suspended? Should I enable it?**</br> 	
A bug was introduced in a previous version that, under certain circumstances, would leave the auto upgrade status set to “suspended”. Manually enabling it is technically possible but would require several complex steps, so the best thing you can do is install the latest version of Azure AD Connect

**Q:  My company has strict change management requirements and I want to control when it’s pushed out. Can I control when Auto-Upgrade is launched?**</br> 
No, there is no such feature today, this is something we’re evaluating for a future release.

**Q: Will I get an email if the auto-upgrade failed? How will I know that it was successful?**</br>  	
You will not be notified as to the result of the upgrade, this is something we’re evaluating for a future release.

**Q:Do you publish a time-line as to when you plan to push out auto-upgrades?**</br>  	
Auto-upgrade is the first step in our release process of a newer version, so whenever there is a new release we will push auto-upgrades. Newer versions of Azure AD Connect are pre-announced in the [Azure AD Roadmap](../../active-directory/whats-new.md).

**Q: Does auto-upgrade upgrade AAD Connect Health?**</br>  	Yes, auto upgrade also upgrades AAD Connect Health

**Q: Do you also auto-upgrade AAD Connect servers in Staging Mode?**</br> 	
No, you can not auto-upgrade an Azure AD Connect server that is in staging mode.

**Q: If Auto-Upgrade fails and my AAD Connect server does not start, what should I do?**</br> 	
In rare cases, the Azure AD Connect service does not start after performing the upgrade. In those case, please reboot the server, which usually fixes the issue. If the Azure AD Connect service still does not start,  please open a support ticket. Here is a [link](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) that explains how to do that. 

**Q: I’m not sure what the risks are when upgrading to a newer version of Azure AD Connect. Can you call me to help me with the upgrade?**</br>
If you need help upgrading to a newer version of Azure AD Connect, please open a support ticket, here is a [link](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) that shows how to do that.

## Troubleshooting
**Q: How can I get help with Azure AD Connect?**

[Search the Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Search the Microsoft Knowledge Base (KB) for technical solutions to common break-fix issues about Support for Azure AD Connect.

[Microsoft Azure Active Directory Forums](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* You can search and browse for technical questions and answers from the community or ask your own question by clicking [here](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[How to get support for Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)

* Use this link to get support through the Azure portal.

