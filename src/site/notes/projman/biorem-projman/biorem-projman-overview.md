---
{"dg-publish":true,"permalink":"/projman/biorem-projman/biorem-projman-overview/","noteIcon":"","created":"2026-03-09T14:08:34.813-05:00"}
---

Date: [[-Daily Activity Log-/2026 03-March 09\|2026 03-March 09]]

# Key Stakeholders
- [[Contractors/Bar Environmental\|Bar Environmental]]
	- [[People/Jeremy Rivere\|Jeremy Rivere]] - Long term contracted operator of the [[Systems/BioRem\|BioRem]] system
	- [[People/Andy LeJeune\|Andy LeJeune]]
	- [[People/Blake Heilman\|Blake Heilman]]
- [[Vocabulary/Maxson\|Maxson]]
	- [[People/Clayton Bennett\|Clayton Bennett]] - Current project manager
	- [[People/Chris Luss\|Chris Luss]] - Current plant manager, long term project manager
	- [[People/Carl Stevenson\|Carl Stevenson]] - Long term project manager, relations with [[Contractors/Bar Environmental\|Bar Environmental]]
	- [[People/Carlton Mull\|Carlton Mull]] - Electrical Foreman
	- [[People/Harvey Woodall\|Harvey Woodall]] - Current maintenance manager
	- [[People/Gary Young\|Gary Young]] - Current maintenance manager
	- [[People/Don Hudgins\|Don Hudgins]] - The Whip
	- [[People/Krishna Bejjanki\|Krishna Bejjanki]] - Hexagon analyst
	- [[People/Henry Nakayama\|Henry Nakayama]] - Former project manager
	- [[People/Stacy Bullard\|Stacy Bullard]] - Long term operator of the [[Systems/BioRem\|BioRem]] system
- [[Contractors/Tioga Environmental\|Tioga Environmental]]
	- [[People/Maggie Strom\|Maggie Strom]]
- [[Government/Shelby County Health Department\|Shelby County Health Department]]
- [[Government/TVA\|TVA]]

# Milestones
- The BioRem must meet air permit compliance by Monday August 24, 2026, as per [[Vocabulary/SCHD\|SCHD]] mandate
	- Continuous data collection with the must [[Equipment/SWG100 biogas analyzer\|SWG100 biogas analyzer]] be achieved.
	- Sulfur removal via system operation
- Continuous monthly reporting of gas volumes and quality to [[People/Tasha King-Davis\|Tasha King-Davis]] using the [[Information Heap/TEM BioRem Data-Calcs v2\|TEM BioRem Data-Calcs v2]].xlsx spreadsheet (or ultimately the [[dev/biogas-tracker/gastrack\|gastrack]] package).
- Contract a storage tank for wasted process water, rather than returning it to the front of the Maxson plant.

# Risks
- Maintenance
	- Sulfur buildup on media
	- Sulfur buildup in the process water lines
	- Flame arrester melting
	- Corrosion of controls components
	- Loss of experience and know-how
	- Limited documentation of protocols and procedures
	- Heat tracer failure
- Operation
	- Not achieving or maintaining stability of the biological system
	- Loss of experience and know-how
	- Restart from zero after a cleaning process to remove elemental sulfur scaling on media.
	- Low quality gas production that is too wet to burn
	- Low quality gas production that is undesirable to users such as TVA
	- Limited documentation of protocols and procedures
- Process design
	- Removed sulfur is simply routed back to the plant rather than being removed from the process.

# Documentation
- The O&M manual (provide link)
# Models and Representations
- A YML based model of the system, which can represent:
	- Mass
		- Water
		- Gas
		- Methane
		- Elemental Sulfur
		- Hydrogen sulfide gas
		- Bacteria
		- Particulate
	- Energy
		- Sensors
		- Power
		- Organic energy
		- Heat
- CAD
- GIS
- Photography
- Videography
- Google Earth
- [[projman/biorem-projman/biorem-projman-overview\|biorem-projman-overview]]
- Gant chart
- Dedicated calendar
- [[dev/biogas-tracker/gastrack\|gastrack]]
- [[Software/Emerson Ovation\|Emerson Ovation]] graphics, points, and schema
- [[Information Heap/Hexagon EAM\|Hexagon EAM]] asset tracking and preventative maintenance plans

# Operational Theory

[[projman/biorem-projman/biorem-operational-theory\|biorem-operational-theory]]

# Important Documents

- SCHD Air Permit
- SCHD Letter of Displeasure and Milestone Setting

- N Drive
	- N:\Environmental\Treatment Plants\Maxson Plant\Emissions Inventory
	- N:\Environmental\Treatment Plants\Maxson Plant\Maxson TVA Biogas Contract
	- N:\Environmental\Treatment Plants\Maxson Plant\Maxson Biogas Drawings
	- N:\Environmental\Treatment Plants\Maxson Plant\TEM Biogas
	- N:\Environmental\Treatment Plants\Maxson Plant\TEM Air Permit Docs 2022-
	- N:\Environmental\Treatment Plants\Maxson Plant\TEM Air Permit Fees
	- N:\Environmental\Treatment Plants\Maxson Plant\TEM Air Permit Info
	- N:\Environmental\Treatment Plants\Maxson Plant\TVA Biogas Analysis 
	- N:\Environmental\Treatment Plants\Maxson Plant\TEM SCADA Emerson 38882
	- N:\Environmental\Treatment Plants\Maxson Plant Projects\Odor Control
	- N:\Environmental\Treatment Plants\Maxson Plant Projects\TVA Gas Turbine Plant

- External Maintenance SharePoint
	- [TEM SCHD Permit Compliance](https://memphistngov.sharepoint.com/sites/PWe/EnvironmentalMaintenancePlant?e=1%3aab8d0a864d7442f39aaf6086e0f852f1&xsdata=MDV8MDJ8R0VPUkdFLkJFTk5FVFRAbWVtcGhpc3RuLmdvdnw4YTE0OTMyYzMzNTY0YWU3ZTgwNDA4ZGQyMDQ2Mjg3Ynw0MTY0NzU2MTY1Mzc0NDIzOTZhOTg1OWU4OWY4OTE5ZnwwfDB8NjM4NzAyMjA3ODkxMTMzNjYzfFVua25vd258VFdGcGJHWnNiM2Q4ZXlKRmJYQjBlVTFoY0draU9uUnlkV1VzSWxZaU9pSXdMakF1TURBd01DSXNJbEFpT2lKWGFXNHpNaUlzSWtGT0lqb2lUV0ZwYkNJc0lsZFVJam95ZlE9PXwwfHx8&sdata=NDlyQW5NWXZtNUFySDhGdHlBWjB4NmRUTDlVejl2RlZSaW9sTnZ2cHdJTT0%3d&CT=1734635035516&OR=OWA-NT-Mail&CID=4f3520b2-1c91-06bb-2d69-e7d9387f84aa&clickParams=eyJYLUFwcE5hbWUiOiJNaWNyb3NvZnQgT3V0bG9vayBXZWIgQXBwIiwiWC1BcHBWZXJzaW9uIjoiMjAyNDEyMDYwMDcuMTIiLCJPUyI6IldpbmRvd3MgMTAifQ%3d%3d&SafelinksUrl=https%3a%2f%2fmemphistngov.sharepoint.com%2fsites%2fPWe%2fEnvironmentalMaintenancePlant)
	- [TEM Weekly Status Forms](https://memphistngov.sharepoint.com/:f:/r/sites/PWe/EnvironmentalMaintenancePlant/Shared%20Documents/TEM%20Weekly%20Status%20Forms?csf=1&web=1&e=levNCh)
	- [Title-V Air Emission Permits](https://memphistngov.sharepoint.com/:f:/r/sites/PWe/EnvironmentalMaintenancePlant/Shared%20Documents/Title-V%20Air%20Emission%20Permits?csf=1&web=1&e=DPJE06)
	- [TEM Title V Reports](https://memphistngov.sharepoint.com/:f:/r/sites/PWe/EnvironmentalMaintenancePlant/Shared%20Documents/TEM%20Title%20V%20Reports?csf=1&web=1&e=onawDM)
	- [TEM TVA Reports](https://memphistngov.sharepoint.com/:f:/r/sites/PWe/EnvironmentalMaintenancePlant/Shared%20Documents/TEM%20TVA%20Reports?csf=1&web=1&e=NwowvN)

- Ovation files - See [[projman/biorem-projman/biorem-projman-ovationdcs\|biorem-projman-ovationdcs]] 
	- Graphics
		- 2000ab.diag
		- 2002.diag
		- 4001.diag
		- PLG6000.diag
		- PLG6001.diag
		- PLG6002.diag
		- PLG6003.diag
	- Control sheets
		- Please add, seem to be missing


# History, Status

[[Information Heap/biorem-projman-history\|biorem-projman-history]]