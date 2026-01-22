---
{"dg-publish":true,"permalink":"/information-heap/m365-cli-setup-for-term-store-management/","noteIcon":"","created":"2026-01-20T13:36:49.343-06:00"}
---

Date: [[-Daily Activity Log-/2026 01-January 20\|2026 01-January 20]]

```
# This is a standard number to use if you do not have a specific ap ID from Entra

m365 cli config set --key clientId --value 1950a06c-22ad-4e31-a9cf-35634496f2af


# tenant id: 41647561-6537-4423-96a9-859e89f8919f
```

```
m365 setup

✔ autoOpenLinksInBrowser: true
✔ copyDeviceCodeToClipboard: true
✔ output: text
✔ printErrorsAsPlainText: true
✔ prompt: true
✔ showHelpOnFailure: true
✔ helpMode: full
✔ authType: browser
✔ clientId: 0000000c-0000-0000-c000-000000000000
✔ tenantId: 41647561-6537-4423-96a9-859e89f8919f
✔ clientSecret:
✔ clientCertificateFile:
✔ clientCertificateBase64Encoded:
✔ clientCertificatePassword: undefined
```


Sorry, but we’re having trouble signing you in.

AADSTS700016: Application with identifier '1950a06c-22ad-4e31-a9cf-35634496f2af' was not found in the directory 'City of Memphis'. This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant. You may have sent your authentication request to the wrong tenant.

Sorry, but we’re having trouble signing you in.

AADSTS50011: The redirect URI 'http://localhost:41097' specified in the request does not match the redirect URIs configured for the application '0000000c-0000-0000-c000-000000000000'. Make sure the redirect URI sent in the request matches one added to your application in the Azure portal. Navigate to https://aka.ms/redirectUriMismatchError to learn more about how to fix this.

Request Id: 1af1a022-7076-4d1b-80e0-34a7ab12a700
Correlation Id: 1144f429-250b-4462-bbf0-bb356c2fd2c0
Timestamp: 2026-01-20T21:31:48Z
Message: AADSTS50011: The redirect URI 'http://localhost:41097' specified in the request does not match the redirect URIs configured for the application '0000000c-0000-0000-c000-000000000000'. Make sure the redirect URI sent in the request matches one added to your application in the Azure portal. Navigate to https://aka.ms/redirectUriMismatchError to learn more about how to fix this.

# With code

```
 m365 cli config set --key authType --value deviceCode
```

You don't have access to this

Your sign-in was successful but does not meet the criteria to access this resource. For example, you might be signing in from a browser, app, location, or an authentication flow that is restricted by your admin.

[Sign out and sign in with a different account](https://login.microsoftonline.com/41647561-6537-4423-96a9-859e89f8919f/reprocess?prompt=select_account&sosid=&ctx=rQQIARAAfY89aBNhGMcvTVtL0Zp2EEGQDAVBe8kl75u7e4MV3stHkzYxuXyZy3LcXd77qEnuzF2uvexKl4p0cHNxqTiJi-AgQicLYgcnJ0fpYhehIIippas_Hv78lofn-d8OJ2NMepk5R6PP8jy0C7tguDQfORjduvnh9CC3-_Ru79M-_-QoFPoRCn2bilTwyDOTWeJbGjnT1-Ebpuc5bjoeN4aKY8b6lja0XVv3YprdfxtehgkWcik2QbMpwNEQJgGNWAXRfAoRHuk8SiD9fXjymIIgAxBNWKDQGoSI5tgEQ6u6ymos32Ug1A7Dwn9uxWNdoiujnhe1HTKwulFnaOtWj0RtXe9ZAyIrmkZc92h66nj6MhNOz83NR6jrVJR6OTMp_OXzb0CFVkp71_R7r_68oA5n4pVWKRUPkIWrj7htppofuw9Vv50C9p3soFzmhoLhFO-7dnGdKa9y6cSz2ezJ7NTOJerjFWpv4TiERSwUxRzGOOMP3MJGzvdrogqEMRA1bmug1NodvwlqY7G9zigPOqY08cnOP8SuZq23JUcWykqfbRY33Q0RZqtI9TKgxAZYkqudfDNQxIbc9OvsFlojlVZv1K03FKNVKRW0McHE6CTHaKgyEu40iMfXOCODcRbCXFFtBpu-2pY3U0kr2B5Bc03pGzkB5piyalkbHQERvZSXZLNGmy6oYbrblKRqvt_ISn1PcB1JzgHX8ce2bhQNjE8XHv_a__pu9_nPwvHVlcJAiNsWZzbYwAVci9R026izSsZP5iFo8mpQ56somyephr36JkJ9n8widbpI7SxRfwE1)

[More details](https://login.microsoftonline.com/41647561-6537-4423-96a9-859e89f8919f/reprocess?ctx=rQQIARAAfY89aBNhGMfvmraWojXtIIIgGQqCbZJL7isXrPBe79JekzO5NKm5LMfd5b2PmNxd76tNdqVLRTq4ubhUnMTFTYROFsQOTk6OkkUXISCIqaWrPx7-_Jbn4fnfTeQzWHEZu0BPn-dF6Jd2ib80nzyJ7tx-Pz7hD5_e6308Ljw5Q9HvKPp1KlkFUWjlORjbOjzX0dRCAHtQDxVV193ICV8nbllh6AXFbNb0Vc_K9G3ddwPXCDO623-bWCZyFEGTVC5NkTidJog8nmYolUkXSAYWGKPA5BjjNMH-50g204GGGvXClOtBx-6kPN817B5MuYbRsx14_goMgrPpqdH0VSxRnJubTyI3kRTycmZS7fOn3ziCrlaObhj3X_15gZzOZKs7FTI7YGxQ26X3sVppGDzS4haJuyucI4q0z5qe8CBwhS1MXKOLuWez3M9Z9OAK8uEacrQwQoEEWEHiAQDrsRNslvk4rksazg5xSaf3HLXeasdNvD6UWluY-rBtyROf7PxD6uj2Vkv2FFZU-1RT6AZlieBqjBau4xVqAGSl1i41B6rUUJrxNrXHbMDqTi_qbDdUc6da2dSHEECznR8yvobJoN2AYaFOm-sAcATBC1pz0I21ltIl8_ZgPyKsDbVv8izBY6Jm2-U2y0CjUpIVq562ArwO0p2mLNdK_QYn90M28GSFxwMvHrqGKZgAjBce_zr-8u7w-Y_N0fVVoed1Gz5WXilEjg-oXckkVrTQI_M-tSdawS7PERIr-Tm-JK69SSLfJrOIjBeRgyXkLw2&sessionid=00a94039-e63a-c449-7610-bfb6c68d044c#)

Need help! Contact City Of Memphis Servicedesk at 901-636-6100

Troubleshooting details

If you contact your administrator, send this info to them.

[Copy info to clipboard](https://login.microsoftonline.com/41647561-6537-4423-96a9-859e89f8919f/reprocess?ctx=rQQIARAAfY89aBNhGMfvmraWojXtIIIgGQqCbZJL7isXrPBe79JekzO5NKm5LMfd5b2PmNxd76tNdqVLRTq4ubhUnMTFTYROFsQOTk6OkkUXISCIqaWrPx7-_Jbn4fnfTeQzWHEZu0BPn-dF6Jd2ib80nzyJ7tx-Pz7hD5_e6308Ljw5Q9HvKPp1KlkFUWjlORjbOjzX0dRCAHtQDxVV193ICV8nbllh6AXFbNb0Vc_K9G3ddwPXCDO623-bWCZyFEGTVC5NkTidJog8nmYolUkXSAYWGKPA5BjjNMH-50g204GGGvXClOtBx-6kPN817B5MuYbRsx14_goMgrPpqdH0VSxRnJubTyI3kRTycmZS7fOn3ziCrlaObhj3X_15gZzOZKs7FTI7YGxQ26X3sVppGDzS4haJuyucI4q0z5qe8CBwhS1MXKOLuWez3M9Z9OAK8uEacrQwQoEEWEHiAQDrsRNslvk4rksazg5xSaf3HLXeasdNvD6UWluY-rBtyROf7PxD6uj2Vkv2FFZU-1RT6AZlieBqjBau4xVqAGSl1i41B6rUUJrxNrXHbMDqTi_qbDdUc6da2dSHEECznR8yvobJoN2AYaFOm-sAcATBC1pz0I21ltIl8_ZgPyKsDbVv8izBY6Jm2-U2y0CjUpIVq562ArwO0p2mLNdK_QYn90M28GSFxwMvHrqGKZgAjBce_zr-8u7w-Y_N0fVVoed1Gz5WXilEjg-oXckkVrTQI_M-tSdawS7PERIr-Tm-JK69SSLfJrOIjBeRgyXkLw2&sessionid=00a94039-e63a-c449-7610-bfb6c68d044c#)

Error Code: 53003

Request Id: e289bbe9-276a-4154-ba5e-08a32afa0200

Correlation Id: 1e2775c3-f8be-45c3-8d8e-3c6cc9a73886

Timestamp: 2026-01-20T21:33:18.342Z

App name: Microsoft App Access Panel

App id: 0000000c-0000-0000-c000-000000000000

IP address: 167.29.0.4

Device identifier: 0b5649b2-796b-4c6b-b8dc-9a3ae2677ab7

Device platform: Windows 10

Device state: Registered

Flag sign-in errors for review: [Enable flagging](https://login.microsoftonline.com/common/debugmode)

If you plan on getting help for this problem, enable flagging and try to reproduce the error within 20 minutes. Flagged events make diagnostics available and are raised to admin attention.

---

### You don't have access

https://portal.azure.com/#view/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/~/Overview

Copy the error details and send them to your administrator(s) to get access to this page.
{"sessionId":"929fc098eb0145c5977a5db9bddc0057","subscriptionId":"","resourceGroup":"","errorCode":"401","resourceName":"","details":"Error loading your content"}


---


# Progress, next steps
```
Import-Module "C:\Users\george.bennett\Documents\WindowsPowerShell\Modules\PnP.PowerShell\3.1.0\PnP.PowerShell.psd1"
# waiting for ThreatLocker approval
```

https://www.powershellgallery.com/packages/PnP.PowerShell/3.1.0

```
# approved!

PS C:\Users\george.bennett> Connect-PnPOnline -Url "https://memphistn.sharepoint.com" -Interactive -ClientId 31359c7f-bd7e-475c-86db-fdb8c937548e
```


```

Thanks for the ThreatLocker approval for PnP PowerShell. To complete the connection, I need a "Single-Tenant App Registration" created in our Entra ID (Azure) tenant, as the default PnP multi-tenant IDs are now retired by Microsoft for security.

Please create a new App Registration for my use with the following specs:

-  Name:  `PnP-SharePoint-Automation-Bennett`
    
-  Supported Account Types:  Single Tenant (Accounts in this directory only)
    
-  Redirect URI (Public Client/Desktop):  `http://localhost` (the standard loopback for modern command-line tools)
    
-  Required API Permissions (Delegated): 
    
    - `TermStore.ReadWrite.All` (Required for Taxonomy/Term Store management)
        
    - `AllSites.Read` (Required for site connection)  
        
	- (optionals) `AppCatalog.ReadWrite.All` (For future site script/SPFx deployments)
	  
Once created, I just need the **Application (Client) ID** to point my PnP module to our internal registration. This is the most secure way to run these scripts as it keeps the 'doorway' limited only to our environment, giving IT full visibility and audit control. This is essential for my upcoming bulk taxonomy updates and site configuration tasks, which are too complex to manage via the web UI."

This is for the SharePoint API, not for MicrosoftGraph, but we can talk about that as well.

```

```
### What to do the second you get that Client ID

Once IT sends you that ID, don't just run a connection; run a "Checkup" to make sure they gave you the right permissions.

**Run this test script:**
$clientID = "PASTE-THE-ID-HERE"
$siteUrl = "https://memphistn.sharepoint.com"

# 1. Connect
Connect-PnPOnline -Url $siteUrl -Interactive -ClientId $clientID

# 2. Test Site Access
Write-Host "Checking Site Access..." -ForegroundColor Cyan
Get-PnPSite | Select-Object Url

# 3. Test Term Store Access (The big one!)
Write-Host "Checking Term Store Access..." -ForegroundColor Cyan
Get-PnPTermStore
```


```
# Store the ID from IT, after Entra registration
$myClientId = "PASTE-THE-ID-HERE"

# Connect to gateway
Connect-PnPOnline -Url "https://memphistn.sharepoint.com" -Interactive -ClientId $myClientId

# Pull the Term Store name
Get-PnPTermStore | Select-Object Name
```