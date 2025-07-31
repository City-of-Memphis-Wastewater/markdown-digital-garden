---
{"dg-publish":true,"permalink":"/information-heap/ovation-data-hub-talk-scott-szymanski/","noteIcon":"","created":"2025-07-29T14:30:12.872-05:00"}
---

Date: [[-Daily Activity Log-/2025 07-July 29\|2025 07-July 29]]

Speaker: Scott Szymanski, Lead Developer of Data Hub Project
Event: Annual [[Software/Emerson Ovation\|Emerson Ovation]] User's Conference

# Root
- Data Hub 2.0 release
- REST APIs
- Web apps
- Ovation Green SCADA integration
- Demo

# History
- The point: Make data accessible outside of the control room
	- HTTP/S protocols
	- OPH data
	- REST APIs
		- Not ideal for streaming high volume Ovation point data to other systems - OPC is better for this
		- Automating routine tasks for reporting
	- Web applications
		- Web HMI
		- Signal diagrams
		- Historical reviews will come, etc

# Security
- Authentication
- User roles and point security groups
- HTTPS encryption in transit
- JWT expiring tokens
- Read only mode disables all write API's
	- Write to only over ride historic data
- Audit logging of all requests
- Operator events sent to OPH

# Deployments
- Standard deployment versus DMZ deployment
- Standard deployment is often not allowed by IT groups 
- The Purdue model of network segregation * look this junt up*
- Future: Data diode

# Data Hub 2.0.0
- First available in June 2025
- Concerned with supporting Ovation 4.0 release.
- Upgrading from Data Hub 1.1.0 does not require new licenses; lower does require a new license

# REST APIs
- Agnostic
	- Node Red
	- Python
	- JavaScript
	- C#
- Responses in JSON format
- Write back functionality
	- Acknowledge and reset alarms
	- Tuning changes, set point changes
	- Talk to stations for auto versus manual control
	- Tuning: Keyboard (?)
- Where Used functionality
- Point ancillary fields
- Point I/O fields


# Use Cases
- see photograph of slide
# Web Apps
- Ovation Data Hub Applications
- Advantages over RDP
	- More secure
	- More scalable
	- Less data shared
	- Reduces Windows licensing costs
- Available
	- Developer tools for REST APIs - Swagger
	- Ovation System Health Monitor
	- Intelligence Framework Advisory Dashboard
	- Operator applications are being developed
- Not Electron - WebView2 - just "install website" via Edge.

# Ovation Green SCADA (OGS)
- Purchased from a third party to support renewables

---

"The EDS will never go away - We will always have an EDS product."

Datalink mode versus Ovation Highway mode.  

Remote work versus security. --> More for remote operation centers, which will still be behind a firewall.

