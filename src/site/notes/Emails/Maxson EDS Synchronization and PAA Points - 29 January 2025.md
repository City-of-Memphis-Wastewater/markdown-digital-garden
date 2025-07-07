---
{"dg-publish":true,"permalink":"/emails/maxson-eds-synchronization-and-paa-points-29-january-2025/","noteIcon":"","created":"2025-07-07T14:23:44.345-05:00"}
---

Content:

Clayton,

Please let me know when it’ll be a good time next week to get this PAA system updated in the Ovation system and to the EDS.

Below are 2 items we will need to accomplish to get all PAA data being sent from the Evonik PAA PLC to the Emerson Ovation and EDS.

1. Assigning addressing to PAA points. Last time when I was on-site we identified a number of data points that should have been in Ovation, but were not added. Based on review of the PAA P&ID’s we created the missing points, but did not have the addressing information coming from the Evonik PAA PLC.

We have since received that information from CDM. I’ve reviewed the list of data and addressing that the Evonik PLC is set up to send to the Ovation system. Below are

I can walk you through how to do this over the phone or can show you in person next week. But to get the values from the Evonik PAA PLC into Ovation we need to do the following:

1. This table contains Ovation points we built together and added to the graphic, but did not assign the appropriate Ovation device and Modbus register the to Ovation point. We will need to open each point in the Ovation Developer studio and in the hardware tab, assign the correct device and register.

|   |   |   |   |   |
|---|---|---|---|---|
|**Ovation Tag Name**|**DESCRIPTION**|**OVATION DEVICE**|**SERVICE  <br>PORT**|**MODBUS  <br>REGISTER**|
|FAH-431|CITY WIER FLOW HIGH ALARM|SCP-430|502|819|
|FAL-431|CITY WIER FLOW LO ALARM|SCP-430|502|820|
|FAL-331A|FCV-331A CONTROL VALVE LOW FLOW ALARM|SCP-430|502|816|
|FCV-331A|FCV-331A VALVE POSITION FEEDBACK|SCP-430|502|40067|
|FAL-331B|FCV-331B CONTROL VALV LOW FLOW ALARM|SCP-430|502|818|
|FCV-331B|FCV-331B VALVE POSITION FEEDBACK|SCP-430|502|40069|
|ZIC-432|INJECTION POINT A VALVE CLOSED|SCP-430|502|833|
|AAH-431A|INJECTION POINT B PAA HI ALARM|SCP-430|502|811|
|AAL-431A|INJECTION POINT B PAA LO ALARM|SCP-430|502|812|
|ZIC-433|INJECTION POINT B VALVE CLOSED|SCP-430|502|834|
|ZIC-434|INJECTION POINT B VALVE CLOSED|SCP-430|502|835|
|AAH-431B|INJECTION POINT C PAA HI ALARM|SCP-430|502|813|
|AAL-431B|INJECTION POINT C PAA LO ALARM|SCP-430|502|814|
|ZIC-435|INJECTION POINT C VALVE CLOSED|SCP-430|502|836|
|FI-331A|PAA FLOW TO WEST INJECTIONS POINTS A/B/C|SCP-430|502|40063|
|FI-331B|PAA FLOW TO WEST INJECTIONS POINTS A/B/C|SCP-430|502|40065|
|FI-431|WEST INJECTION POINT FLOW|SCP-430|502|40073|

2. I found a couple additional points in the CDM supplied list that I think are worth adding into the Ovation system. These points are not in the Ovation system now. We will need to add the points, assign the Modbus addresses like above, and add to a graphic.

Again, I’m happy to do this on-site next week or if you feel that these points are not needed we can omit.

|   |   |   |   |   |
|---|---|---|---|---|
|**DESCRIPTION**|**OVATION DEVICE**|**SERVICE  <br>PORT**|**MODBUS  <br>REGISTER**|**Ovation Tag Name**|
|PUMP P-121A RUN COMMAND|SCP-420|502|49|P-121A|
|PUMP P-121B RUN COMMAND|SCP-430|502|849|P-121B|
|SCP4 24VDC POWER SUPPLY FAILURE|SCP-430|502|841|XS-435|
|SCP4 PHASE/VOLTAGE ALARM|SCP-430|502|840|XS-434|
|SCP4 UPS IN BATTERY MODE|SCP-430|502|839|XS-433|

2. This morning we found that the graphics in the EDS are not the most recent version and newly added graphics are not being displayed. I can correct next week when on-site, but for informational purposes I can explain how the graphics are sent to the Emerson EDS.

- On the Ovation Database server (Drop 200) there is a process that is supposed to run nightly that collects all of the graphics, zips them, and sends them to the EDS server.
- There is a batch file on the Ovation database server (Drop 200) called zip_files.bat. This batch file calls a couple other batch files to export the graphics and send via SFTP to the EDS server. You can search for zip_files.bat on the Ovation database server and open in a text editor if interested, but it should just work.

There are a couple issues that could be preventing this from happening:

1. The script is looking for a mapped network drive L:\. If for some reason on the database server the mapped drive no longer exists, this script won’t work.

1. We will need to open the zip_files.bat to check if it says “SET ONLINE_DIR=L:\\OVATION\on-line”.
2. We will need to make sure that when opening windows explorer we see a mapped drive “L” (or whatever drive letter is specified above)

3. This could be a port being blocked on the network. Since the point database file is being sent to the EDS server nightly with no issue I don’t think that this is the case.

There should be an icon in the upper left of the Database Server that says something about a script to send graphics to the EDS.

I will check more and with Emerson more to see if there could be any issue. If you get a chance and could take a picture of the desktop on the database server and a picture of windows explorer it would help to troubleshoot. If you don’t have the bandwidth or are uncomfortable with the Ovation system just yet, I can walk through all of this next week.

Thanks,

Pete

