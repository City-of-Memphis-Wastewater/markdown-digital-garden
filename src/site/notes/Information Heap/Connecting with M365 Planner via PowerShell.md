---
{"dg-publish":true,"permalink":"/information-heap/connecting-with-m365-planner-via-power-shell/","noteIcon":"","created":"2026-03-02T16:19:18.779-06:00"}
---

Date: [[-Daily Activity Log-/2026 03-March 02\|2026 03-March 02]]

```
Connect-MgGraph 
Get-MgUserPlannerPlan -UserId "george.bennett@memphistn.gov" | Select-Object Id, Title, ContainerId
```

```
PS C:\Users\george.bennett\dev> Get-MgPlannerPlanTask -PlannerPlanId "vez7tvdWkEyI6NbxMh63AIIAED2_" | Select-Object Id, Title, BucketId

Id                           Title                             BucketId
--                           -----                             --------
h-QzVSIVWEGiohEXGcXFVoIAAY7C Clayton is BioRem Project Manager bRHbaA0-WUWTelEpto95K4IACWxQ
hX7CHtnxLEasD_UoJTcUSIIAC3oL BioRem Refurbishment              Sb1U9qCXs0COKS1LYT8E74IAEb9n
ekDC6qHAnk6Q7oLsPXN4_oIACKv3 Weekly Friday Meeting             yrYTnd7iL0mAJu0iTsorZIIAH2pQ


PS C:\Users\george.bennett\dev> Get-MgUserPlannerPlan -UserId "george.bennett@memphistn.gov" | Select-Object Id, Title, ContainerId

Id                           Title                                           ContainerId
--                           -----                                           -----------
c9Fs1Nu7RUO8gTG_EHoBDoIAA-Jk Project Management - Template February 26, 2026
eKpDY8wNk0i5mvSv2Fy4V4IAERXQ Maxson Monthly Staff meeting 2025-01-07
vez7tvdWkEyI6NbxMh63AIIAED2_ Shared with Henry
w8n8EDiA1U2VXxcC1DH1i4IAHMJz Emerson SureService Tickets


PS C:\Users\george.bennett\dev> Get-MgPlannerPlanBucket -PlannerPlanId "vez7tvdWkEyI6NbxMh63AIIAED2_"

Id                           Name               OrderHint             PlanId
--                           ----               ---------             ------
bRHbaA0-WUWTelEpto95K4IACWxQ Agreements         8584293781K           vez7tvdWkEyI6NbxMh63AIIAED2_
AxRTfaD3mUKu9kTUNwpq34IAE-oj Completed Projects 8584293783FB          vez7tvdWkEyI6NbxMh63AIIAED2_
Sb1U9qCXs0COKS1LYT8E74IAEb9n Ongoing Projects   8584293783Y'          vez7tvdWkEyI6NbxMh63AIIAED2_
yrYTnd7iL0mAJu0iTsorZIIAH2pQ To do              8584293784987078532Pt vez7tvdWkEyI6NbxMh63AIIAED2_


PS C:\Users\george.bennett\dev> Get-MgPlannerTaskDetail -PlannerTaskId "h-QzVSIVWEGiohEXGcXFVoIAAY7C"

Id                           Description PreviewType
--                           ----------- -----------
h-QzVSIVWEGiohEXGcXFVoIAAY7C             automatic


PS C:\Users\george.bennett\dev> $bucketId = "bRHbaA0-WUWTelEpto95K4IACWxQ"
PS C:\Users\george.bennett\dev> $planId = "vez7tvdWkEyI6NbxMh63AIIAED2_"
PS C:\Users\george.bennett\dev>
PS C:\Users\george.bennett\dev> Get-MgPlannerPlanTask -PlannerPlanId $planId | Where-Object { $_.BucketId -eq $bucketId } | ForEach-Object {
>>     Write-Host "--- Details for: $($_.Title) ---" -ForegroundColor Cyan
>>     Get-MgPlannerTaskDetail -PlannerTaskId $_.Id | Select-Object Description, Checklist, References
>> }
--- Details for: Clayton is BioRem Project Manager ---

Description Checklist                                                             References
----------- ---------                                                             ----------
            Microsoft.Graph.PowerShell.Models.MicrosoftGraphPlannerChecklistItems Microsoft.Graph.PowerShell.Models....


PS C:\Users\george.bennett\dev>
```

```
PS C:\Users\george.bennett\dev> Invoke-MgGraphRequest -Method GET -Uri "https://graph.microsoft.com/v1.0/planner/tasks/h-QzVSIVWEGiohEXGcXFVoIAAY7C"

Name                           Value
----                           -----
dueDateTime
completedDateTime
title                          Clayton is BioRem Project Manager
assignments                    {}
referenceCount                 0
completedBy
percentComplete                0
hasDescription                 False
planId                         vez7tvdWkEyI6NbxMh63AIIAED2_
bucketId                       bRHbaA0-WUWTelEpto95K4IACWxQ
assigneePriority
previewType                    automatic
startDateTime
orderHint                      8584293781514465830Pc
activeChecklistItemCount       0
id                             h-QzVSIVWEGiohEXGcXFVoIAAY7C
priority                       5
@odata.etag                    W/"JzEtVGFzayAgQEBAQEBAQEBAQEBAQEBASCc="
conversationThreadId
createdBy                      {user, application}
createdDateTime                2/27/2026 9:46:34 PM
@odata.context                 https://graph.microsoft.com/v1.0/$metadata#planner/tasks/$entity
checklistItemCount             0
appliedCategories              {}


PS C:\Users\george.bennett\dev>
```