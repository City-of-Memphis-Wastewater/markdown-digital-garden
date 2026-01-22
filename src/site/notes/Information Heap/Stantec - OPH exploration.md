---
{"dg-publish":true,"permalink":"/information-heap/stantec-oph-exploration/","noteIcon":"","created":"2026-01-21T16:59:07.636-06:00"}
---

Date: [[-Daily Activity Log-/2026 01-January 21\|2026 01-January 21]]

The Maxson [[DROP160\|DROP160]] K Drive "OPH_RAID" is shown to have last been modified in March of 2025.

So data is coming in to the system, but it has not been being stored.

The Historian Diagnostics window shows that the services are:
- "Ovation Process Historian Report Service": Running
- "Ovation Process Historian":  Not Running

Look at Resource Moniter
- It is expected that writing to the K drive should correlate with network activity.

In Services, this is what it looked like on 21 January 2026:

| Name                                      | Description | Status  | Startup Type | Log On As   |
| ----------------------------------------- | ----------- | ------- | ------------ | ----------- |
| Ovation Process Historian                 |             |         | Manual       | Local sys.. |
| Ovation Process Historian Report Services |             | Running | Manual       | Local sys.. |


# Big Implementation Goal "Change management" 
- We do not have a defined workflow for managing change.
- Enterprise tool with support, redundancy 
	- Atlassian? This is more for software.
	- Hexagon - do they already offer a change management solution?
	- https://www.iobeya.com/use-cases/lean-engineering/?utm_term=continuous%20improvement%20solutions&utm_campaign=SN+-+Software+-+NA&utm_source=bing&utm_medium=ppc&hsa_acc=5947928080&hsa_cam=22867720055&hsa_grp=1225956764445969&hsa_ad=&hsa_src=o&hsa_tgt=kwd-76622738915433:loc-4125&hsa_kw=continuous%20improvement%20solutions&hsa_mt=p&hsa_net=adwords&hsa_ver=3&msclkid=eaf417e042d318f67c6e7e911c8bc660