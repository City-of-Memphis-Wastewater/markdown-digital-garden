---
{"dg-publish":true,"permalink":"/projman/projman-protocols/projman-cli-protocol/","noteIcon":"","created":"2026-03-09T13:11:01.835-05:00"}
---

Date: [[-Daily Activity Log-/2026 03-March 09\|2026 03-March 09]]

# Key Aspects
## How Can CLIs be Leveraged in Project Management?

Information propagation, export, backups, synchronization, reporting.

This is related to [[projman/projman-protocols/projman-api-protocol\|projman-api-protocol]].
## Ranking System and Decision Matrix
 How can we assess use cases of CLIs based on:
 - Timely execution
 - Fit
 - Leveragability
 - Stability
 - Good vibes

The inference here is that some ways to leverage an CLI might be like pulling teeth, take a long time to implement, be hard to maintain, and be a waste of time.

## Which CLIs are Available to Leverage for Project Management?

- MicrosoftGraph
	- Commands (Powershell):
		- Connect-MgGraph 
		- Invoke-MgGraphRequest
		- Get-MgPlannerTaskDetail
		-  Get-MgUserPlannerPlan
	- Purpose:
		- Access Microsoft Planner, SharePoint, Power Automate, and the wider M365 platofmr
- PnP
	- Examples (PowerShell):
		- Import-Module "C:\Users\george.bennett\Documents\WindowsPowerShell\Modules\PnP.PowerShell\2.12.0\PnP.PowerShell.psd1"
		- Connect-PnPOnline -Url "https://memphistn.sharepoint.com" -Interactive -ClientId 9bc3ab49-b65d-410a-8536-c7d0d8b7e23d
		- Connect-PnPOnline -Url "https://memphistn.sharepoint.com" -DeviceLogin
		- Install-Module PnP.PowerShell -Scope CurrentUser -AllowClobber -Force  
		- Set-PnPTermGroup -Identity "Maxson Term Taxonomy" -AddContributor "george.bennett@memphistn.gov"
		- 
- AzureCLI
	- Examples:
		- curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
		- az --help
		- winget install -e --id Microsoft.AzureCLI 
		- $ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi
- obsidian
	- Commands
		- login
		- tasks
	- Purpose:
		- Access local obsidian vault to for tasks, reading, and writing of files.
		- Could be used to automate documentation export using custom software, rather than relying on something like Claude Cowork for modification to the knowledge base.
- m365
	- Commands:
		- login
	- Examples:
		- m365 sharepoint term group set --siteUrl https://memphistngov.sharepoint.com/sites/... --name "Maxson Term Taxonomy" --contributors "user@memphistn.gov"
	- Status:
		- Promising, but not yet approved by [[Information Heap/CoM IT\|CoM IT]]
		- Error Code: 53003
		- Need to resolve Entra user permission

---