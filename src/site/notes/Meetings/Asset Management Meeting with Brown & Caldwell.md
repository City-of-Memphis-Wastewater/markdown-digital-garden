---
{"dg-publish":true,"permalink":"/meetings/asset-management-meeting-with-brown-and-caldwell/","noteIcon":"","created":"2025-08-08T12:59:16.521-05:00"}
---

Date: [[-Daily Activity Log-/2025 08-August 08\|2025 08-August 08]]

Maintenance strategy
Asset management
Vendor: [[Brown and Caldwell\|Brown and Caldwell]]

People
- Anne Kennedy (Brown and Caldwell)
- [[Rich McGillis\|Rich McGillis]] (Brown and Caldwell)
- Joshua Balentine (Brown and Caldwell)
- Henry Nakayama
- Don Hudgins
- Clayton Bennett
- Chris Luss
- Derek McElroy
- Robert Dodson

Don et al saw [[Brown and Caldwell\|Brown and Caldwell]]'s presentation at the 2025 [[Water Professionals Conference\|Water Professionals Conference]] in Knoxville last week.

---

# Intro
- Key interest: Condition and consequence to inform an R&R plan

[[Rich McGillis\|Rich McGillis]] specializes in water and sewer. The regional manager for "utility performance".


# Louisville
Louisville, KY had a consent decree to negotiate asset management into their amended CD.
Quality quality and pump stations needs to improve.
Define:
- How to assess their assets
Taking inventory:
- Looked at every vertical asset
- five regional plants
- two larger plants
- 290 pump stations
Key questions:
- What are the most critical assets?
- What should you spend on in the future?
- What is the process of capturing data to enable you to make better decisions?


There are some legal aspects here. Jargon:
 - "CIP"
 - "CD"
 - "CMMS"

Louisville's big problem
- Labor limited. 
- Were not optimizing the visualization of the data


Henry: how did you determine the critical assets?
Anne: consequences, likelihood of failure.

They sure do like Power BI.

"rehab and replacement"

11 months, 50k assets.

---

# Meat and Potatoes

Don: Impressed with wastewater specific approach, rather than just sewers and manholes.
Chris: Yay, less work.
Don: We were impressed with the 20 year spending plan.
	- Long lead times have been problematic for us.
	- The city should have planning in the CIP area
	- We need outside help t put together bid packages
	- Inventory all of the assets - does it make sense to coordinate bids
	- Enable our own 20 year forward-look
	- Use CIP budget rather than plant operating budget

Anne:
- Louisville has to spend a certain amount every year due to their CD
- Operations and engineering shared ideas.
- "Six codes of happiness"
- Each asset has a "happiness code"

CMMS software: IPS : [Enterprise Asset Management - IPS ENERGY](https://ips-energy.com/solutions/enterprise-asset-management-eam/)
Software: Articulate 

Our CMMS: [[Software/Hexagon\|Hexagon]]

Six part naming conventions

Don: 
- Primarily we want to be able to write specifications to write bids for things?
- Rather than getting a vendor to give us the specs
- We don't have the personnel to bid properly
- Chris Luss needs help to buy stuff.
- We already have a contract with Stratum
- Inventory of all assets to enable bid packages
- Go through all of our files, help us upload specs to all of our equipment.

- [ ] Stratum guy will be here next week

Anne:
- Helped to inventory the documents
- Pushed for document scanning to get it into the DMS, and then attached to asset records
- Make it easier to find submittal docs, manuals, etc

Don:
- We are not under a consent decree, so we have more flexibility
Anne:
- [[Contractors/Black & Veatch\|Black & Veatch]] is a great company
- We can piece together the best elements to accomplish the goal.
- Condition assessment, engineering study

Don: 
- [[Contractors/Black & Veatch\|Black & Veatch]] has focused on the sewer system, not a wholistic review of the treatment plants.
Anne:
- Did you like the way they did it?

Joshua: 
- Do you need the scope of the fee?
Don:
- It needs to be phased
- Robert Dodson needs to decide if he wants to be involved
- Stiles should be first, because Maxson is still under CMAR 
Chris:
- Vote: Maxson first, hard ball first
Don:
- First phase, review current CMMS's
- Is Robert Dodson keeping up with Hexagon?
Robert:
- Nope. Lack of personnel to load assets.
- [[Contractors/Black & Veatch\|Black & Veatch]] is still assessing lift stations, replacements etc.
- What are you doing with the plants?
- Currently engaged with [[Contractors/Black & Veatch\|Black & Veatch]] for maintenance and tracking of lift stations.
- Would like to see a 1 to 10 year plan.
Don: 
- Is [[Contractors/Black & Veatch\|Black & Veatch]] doing this out of the [[Information Heap/SARP10\|SARP10]]?
Robert:
- Yes
- Calcium aluminate coating in the wet well
- minor replacement as necessary
- Conditions have changed significantly since the project was bid
- In some cases, entire lift stations have had to be rebuild even after they were refurbished
Anne: 
- Yep, just sitting in a problem. Louisville experienced that.
- Aging data is a problem.
- Data-informed decision making in near-real time (within 6 months)
- Morris Foreman was where the started - it was the worst: [Morris Forman Water Quality Treatment Center | MSD](https://louisvillemsd.org/programs/pretreatment-and-hazmat-program/morris-forman-water-quality-treatment-center)

Don:
- We are most interested in electrical, mechanical, instrumentation
- We have an Emerson system DCS, with a SureService plan
- Do you consider pipelines to be an asset?
- Pipe galleries differ from long piping
- An asset is what a maintenance person would touch, rather than inventory

Anne:
- What is an asset? How do you define it?
- Louisville defines it based on a dollar amount, $10k
	- ![Pasted image 20250808134428.png](/img/user/Pasted%20image%2020250808134428.png)

Don:
- We have a problem with value change, given capitalization and depreciation
- We want to synchronize our list with the City's list
Anne
- It's all about an existing agreement for what can be bought without intense review, it if it already in the plan.
- You guys don't have a definition of an asset.
- Components versus parts versus assets versus systems.
- Gatsby 34 definition, $2,500 - this might be too low, because you 
- Asset definitions from AWWA, WEF, and others
- What is the hook? 
	- Maintenance
	- investment
	- data collection
	- decision making
	- evolve over time
	- start somewhere
	- people do adjust their definitions of assets when they recognize something is non-deal
	- Over granular or not granular enough

Don:
- CDM did the CMAR upgrade - ASTM formatting for asset management - 
- ISO 55001 [ISO 55001:2014 - Asset management — Management systems — Requirements](https://www.iso.org/standard/55089.html) - rigid - is it useful for municipalities?
- Excel spreadsheet of nameplate information handed over by [[Contractors/CDM Smith\|CDM Smith]]

---
# Questions

Clayton:
- How can we leverage an opportunity like an asset review to improve our integrated databasing for our various systems? CAD, SCADA/DCS, GIS, digital twin, lab testing, EAM
- Answer: What functional requirements do you want?
- Follow-up: Design must be API, modularity
- Don: This is not the primary purpose of this contract