---
{"dg-publish":true,"permalink":"/meetings/meeting-bio-rem-logs-past-and-future/","noteIcon":"","created":"2025-05-20T09:18:16.564-05:00"}
---

[[People/Don Hudgins\|Don Hudgins]]: 
Set up the BioRem in the Hexagon system
In an appropriate way
Need a location code to assign to it [[people/Mike\|people/Mike]]

Each part of the plant has a location code

Please share in Hexagon a list of codes, [[People/Krish\|Krish]]
Identify which codes cover the BioRem system
Tag sensors and probes (expect more than 8 in the BioRem, there are thousands in the plant)
A minimum of 34 DO probes
Maintenance can log activity with appropriate

[[People/Krish\|Krish]]:
Show screenshare of Hexagon
Discussion with [[People/Rodney Williams\|people/Rodney Williams]]
Identify communication failure modes #failuremode , when information is not passed into the system.
Manual notebook (will use in future) needs to be data-entry-ed to Hexagon.

"Gate"
Show listing of equipment: General classification and subclassification.
Asset code example: "1001ATS"
https://us1.eam.hxgnsmartcloud.com/wed/base/COMMON
Department: 1000M
Show full department list:
This shows disinfection "department" (1001):

Work on taking existing data and making it user friendly
![Pasted image 20250123122930.png](/img/user/Pasted%20image%2020250123122930.png)


![Pasted image 20250123122542.png](/img/user/Pasted%20image%2020250123122542.png)
limited scope: ![Pasted image 20250123122605.png](/img/user/Pasted%20image%2020250123122605.png)
![Pasted image 20250123122623.png](/img/user/Pasted%20image%2020250123122623.png)
"EWP"

When you buy a piece of equipment, we need to update the asset list, for a consistent way of referring to something - consistent ID for consumables.

NP2 was ported over to hexagon #p2 - Mike
![Pasted image 20250123123134.png](/img/user/Pasted%20image%2020250123123134.png)

Do it right with BioRem for how we do Hexagon

Foremen will generate a work order for  some PM task
Department number is the root number of an asset
![Pasted image 20250123123454.png](/img/user/Pasted%20image%2020250123123454.png)
200's are primary clarifiers

Rolling stock:
![Pasted image 20250123123535.png](/img/user/Pasted%20image%2020250123123535.png)
![Pasted image 20250123123656.png](/img/user/Pasted%20image%2020250123123656.png)

Departments = systems? in Hexagon parlance
![Pasted image 20250123123900.png](/img/user/Pasted%20image%2020250123123900.png)

"DO probes are the most important probes are the plant"

1. What did we buy?
2. What has become broken?
3. What will happen when we try to turn the system on?
4. What are the key improvement opportunities? - low hanging fruit

Krish will add a department in Hexagon, to add BioRem. ANd then add all BioRem components are unique assets

Every unit (ex. tank 1) it reads out to one PLC
The PLC should be connnected to the Ovation system, once we get the fiberline fixed - consider Ovation tags and the unique asset ID's

Look at equipment ID's in Ovation (points) for BioRem equipment

RFID tagging / QR code tagging for spares, consumables, etc
![Pasted image 20250123124915.png](/img/user/Pasted%20image%2020250123124915.png)
Strictly maintenance, operators will not benefit directly from the Hexagon system - but they can benefit from the [[Contractors/Hach\|Hach]] [[Assignments/Hach WIMS Improvements\|Hach WIMS Improvements]] interface (which will talk to Hexagon)
Operators will be able to enter work request, but will live mostly/entirely on paper:

Goal for Hexagon:
Query equipment records by run hours, temperature, vibration (high-need likelihood cases)


![Pasted image 20250123125858.png](/img/user/Pasted%20image%2020250123125858.png)

Excel EDS, Don:
![Pasted image 20250123130313.png](/img/user/Pasted%20image%2020250123130313.png)
![Pasted image 20250123130333.png](/img/user/Pasted%20image%2020250123130333.png)

1. Connect to server (EDS tab)
2. Tabular Trend window
3. Add cells, hit "okay"
4. Go to raw data tab, enjoy
Example: Cellulose activity, in order to assess influent contributions from various industries as they come online
-->Automate! #automate
Daily, show averages over time, etc
![Pasted image 20250123130919.png](/img/user/Pasted%20image%2020250123130919.png)
![Pasted image 20250123130933.png](/img/user/Pasted%20image%2020250123130933.png)

[[strategy/PAA dosing\|strategy/PAA dosing]]
Pros and cons to EDS, might be better to access historian, but, use it!
EDS allows for outside access.
Historian has more to access in terms of access.

Key points to analyze and track:
- [[strategy/PAA dosing\|strategy/PAA dosing]] from [[Industry/Evonik\|Evonik]] 
- Hargrove sent us a point list for the PAA system
![[HAR-LST01378-EN-CTL-002 Modbus List Rev 1.pdf]]

File: 2024 Annual maintenance accomplishment report.xlsx - get copy from [[people/Mike\|people/Mike]] on a server. This is an example of the format Don would like to see for the [[Projects/Biogas Flare Reports\|Biogas Flare Reports]]: equipment history 2023-2024.
Expect to find 10 items or so. 


Don's annual reports are on the common drive  - the big long 60 page docs
--
[[People/Krish\|Krish]] called directly:
- [ ] Request list for all BioRem equipment - look in O&M - given by contractor once they built 
- [ ] Logbook: Rodney: get it! Ask Mike to help me find Rodney





