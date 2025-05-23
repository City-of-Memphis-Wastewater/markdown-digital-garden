---
{"dg-publish":true,"permalink":"/meetings/hach-wims-review-scope-of-work-2025-05-may-21/","noteIcon":"","created":"2025-05-23T14:53:50.085-05:00"}
---

Date: [[-Daily Activity Log-/2025 05-May 21\|2025 05-May 21]]


Attendees:
- Lori Franklin
- Andre Armstrong
- Henry Nakayama
- Peter Gabor
- John Abrera
- Bruce Lee
- Julie LaVoice
- Jennifer Nimako
- Richard Barlow
- Eric Boyd
- Matthew East
- George Clayton Bennett
- Brittany Jones


Eric Boyd: Purpose of meeting: Discern scope of project
- Purchase two servers for Benjamin Hooks Library


Henry: 
- Henry is the project manager
- At the user level, the project should: ... 
- Stantec has expertise for software and hardware
- We generate a lot of data every day
	- Laboratory (x-lims)
	- Emerson Ovation DCS/SCADA system -> historian is purposefully air gapped
	- Microsoft Access database (WIMS) - really slow, because there is so much data in it.
- With Hach WIMS and (Rio) Aquatic Informatics, we want to
	- produce reports
	- experience a digital twin
	- process modelling
	- queries
	- cloud based system
	- SQL server
- ---> We only need one server for both plants

Eric:
- Does a scope of work document already exist?
Henry:
- Yes, an outline exists - expect it in an email after
Matthew East:
- Will these be physical servers or do we want virtualized servers? Windows servers, much like the EDS servers. 
Andre Armstrong:
- One physical server. Though the vendor says a virtual server may suffice.
Matthew East:
- What licenses are necsssary? One Windows license and one SQL license?
Andre:
- Yes, one Windows license and one SQL license
Bruce:
- Bruce wants to see a direct ask for server IT. He has seen options. Does it meet or match the architecture. 

![Pasted image 20250521155026.png](/img/user/Pasted%20image%2020250521155026.png)

Pete:
- Explains the Hach WIMS architecture.
- The WIMS SQL database and the WIMS apps server can both live on the same machine.

Bruce:
- Specifications for consolidated server: recommended two terabytes - what is the outlook for data consumption?
- Uncontrolled data growth is a concern.

John Abrera:
- There is a factor of safety of three or four for expected data usage, based on vendor specs.

Bruce:
- Bruce advocates for a physical box, so that there is no uncontrolled growth issue in a virtual environment with shared resources.

Eric Boyd:
- Concur. Does the vendor have remote access to manage the storage?
- What is the expected growth.
- Backup schema will be impacted.

Pete:
- The software allows for offsite back up occasionally. However can data effectively be recalled from a backup for delayed analysis?
  
![Pasted image 20250521160249.png](/img/user/Pasted%20image%2020250521160249.png)

Henry:
- Expect upgrades to the plant for the next 10 and 20 years.
- Regulations will increase, ergo work will increase.
- Disinfection is costing $15 million per year. All of this analysis is necessary to streamline the system.

Eric:
- We will have to implement the Hach WIMS software?

Andre:
- Who will be designated as administrator.

Matthew East:
- It's mostly a question of process.
- How often do you need to get in the box? If rarely, it could be handled with tickets.

Andre:
- How will outside vendors be able to provide support and updates?

John Abrera:
- Yes, the vendor will provide updates.

Matthew:
- Will City IT be required to do regular maintenance on your SQL server? Or is it done by the vendor.

John Abrera:
- The City will need to be involved to some degree.

Eric Boyd:
- Is a security assessment necessary?

Richard Barlow:
- Yes there should be a security assessment.

Eric Boyd:
- Good, please provide a contact to the vendor.
- What is the budget?

Henry:
- Around half a million

Andre Armstrong;
- For the physical server, around $3,000.

Eric Boyd:
- Please provide all costs and budget information.
- SOW's.
- One month is a reasonable timeline to sort out details.

Henry:
- Key issue now is hardware purchasing, to avoid delays down the line.

Eric Boyd:
- The IT Finance Manager says that IT won't be able to purchase a server, it'll be up to Public Works to do purchasing. 
- Enough bandwidth from the project team should be available by early June.

Bruce Lee:
- This cannot be done with the IT budget for the time being.

Henry:
- We can purchase this ourselves.

Bruce:
- We do not mind picking out the right device. We just need to fully understand the requirements that dictate which equipment you need. And then once we can get a quote, does it meet the needs of the project. 

Eric:
- Please submit the SOW.
- Eric will reach back out to Henry and Andre.
Henry:
- Stantec should assess the expected data consumption rate.