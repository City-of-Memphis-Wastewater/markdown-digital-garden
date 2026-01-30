---
{"dg-publish":true,"permalink":"/information-heap/emerson-sure-service-call-for-drop-160-being-down-january-30-2026/","noteIcon":"","created":"2026-01-30T12:12:19.129-06:00"}
---

Date: [[-Daily Activity Log-/2026 01-January 30\|2026 01-January 30]]

We want [[Information Heap/Garth Egbert\|Garth Egbert]] involved on the call.

SureService Contract Number MC 626

[[Information Heap/Drop 160\|Drop 160]] at [[Information Heap/Maxson\|Maxson]] has not been storing data since March 2025. [[Information Heap/Drop 161\|Drop 161]], the backup OPH 

Ovation Version 3.8

Each of the OPH drops have an OPH_STORAGE drive and an OPH_RAID5 drive.

Which files to look for for fact finding:
- OPH configuration
- Logs
Emerson will analyze these files.

This is a good reason to expediate the [[Information Heap/Airwall 110\|Airwall 110]]. Right now we can only do a teams.

Emerson support engineer on the call: Sebastian.

I should expect an email from Emerson with further instruction


----


> Hello George,
> 
> Thank you for contacting Ovation Product Support. A recap for your records is provided below. Kindly reference the Call Record Number in your subject line when requesting or providing an update.
> 
> |   |   |
> |---|---|
> |**_Record number:_**|**_NC-2600-4250_**|
> |**_Customer Name:_**|_George Bennett_|
> |**_Call Subject:_**|**_OPH Drop160 not storing information_**|
> |**_Software Version_**|_3.8_|
> |**_DCS Support Specialist:_**|**_Sebastian Gomez_**|
> 
> **Problem Details**
> 
> You stated Drop 160 is not storing data since March 2025. The backup Drop 161 has been gathering the information. You`d like to understand why Drop 160 has stopped and what steps can be executed to correct this.
> 
>  **Instructions**
> 
> George, as we discussed during our phone call, please provide the following information and files for us to review:
> 
> **OPH config file @ DBS:**
> 
> 1.  From Developer Studio open the Historian Configuration tool (at System level / Configuration / Historians / Ovation Process Historian Servers / right click on the OPH server/ Engineer)
> 2.  In the Historian Configuration tool go to the Home tab and select the Export option.
> 3.  Save the document as an .xml file. 
> 
> **OPH gather**:
> 
> 4. Go to the OPH server 
> 5. Navigate to D:\OvHist\Tools\gather\ and double click on OVH_Gather.bat file.
> 6. It will compile some files and then will copy them into the WORK folder.
> 7. Zip and send the following folder: D:\OvHist\Tools\gather\Work
> 
> Open Historian Diagnostics. Send pictures of the status on the different tabs.
> 
> We at Ovation Product Support will be pleased to assist you. Please do not hesitate to contact us with any further concerns or comments.
> 
> Best regards,
> 
> **Sebastian Gomez** | DCS Support Engineer | Ovation Product Support |  
> 
> **Emerson**  |Power and Water Solutions