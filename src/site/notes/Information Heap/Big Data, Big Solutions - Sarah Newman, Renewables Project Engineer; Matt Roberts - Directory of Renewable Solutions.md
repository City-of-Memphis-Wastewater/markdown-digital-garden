---
{"dg-publish":true,"permalink":"/information-heap/big-data-big-solutions-sarah-newman-renewables-project-engineer-matt-roberts-directory-of-renewable-solutions/","noteIcon":"","created":"2025-07-29T12:02:05.678-05:00"}
---

Date: [[-Daily Activity Log-/2025 07-July 29\|2025 07-July 29]]

"Keep it under control"
Batteries! Battery fleets.
# Overview
- More data is required by field devices
- Ovation System Architecture
- Configuration tips and tricks
- The future
# System capacity requirements
- Traditional DCS / SCADA
	- typical point counts up to 200 points per MW
	- Data links used to be thought of as an afterthought, the main system was the hardwired (4-20mA) IO.
- String inverters
- Renewables: 
	- Primarily remote
	- point counts up to 2000 points per MW
	- hard wired IO points is going down
	- Modbus and OPC counts are going up 
- ACID management level

# Scaling Up
- A lot of data is just stored for warranty purposes and analysis, rather than real-time logic and actuation
- Increase value of remote operators
- four elements of scaling
	1. device count (IP addresses)
	2. Scan rate
		- 100 Hz vs 30 seconds
	3. point count
	4. configuration time
		- A minute per point is too slow
		- **Maximize repeatability** throughout the fleet
	
"The sun moves slow."

# Where is the data coming from and going?
- Less than 5% is for control
	- Modes
	- set points
	- resets
- Condition monitoring (35%)
	- temperature
	- power, current, voltage
- Statuses & faults (55%)
	- alarms

# Terminology
- Device: Physical field equipment component
	- inverter
	- battery
	- controller
	- datalogger
- IO point
	- field device with a register to communicate with. Sends a value
- Ovation point
	- a named IO point with a system ID and configuration with Alarms and tracking
- OCB
	- A special type of Ovation point, internally assigned to in Ovation logic
	- Add to controller point count
- Scan rate
	- Frequency at which points are communicated between devices
	- depends on vendor
- DDB
	- Dynamic data blocks
	- medium used by Ovation to broadcast points across the Ovation highway
	- Can hold between 50-70 points
- Ovation highway
	- The devices that are connected into the network and are communicating
- Ovation system
	- A step further than the Ovation highway in that it can be across multiple networks

# Planning your system
- Capacity
	- Determine capacity: consider performance, reliability, and capability.
	- How do you design around capacity?
	- Something that works at a FAT doesn't work well in all situations.
	- Design for performance in the worst case conditions.
	- works =/= works well
- Reliability
- validated architecture
	- stated limits
	- Neither a firm upper limit nor a guaranteed capacity
- system specific design
	- disclaimer
- selection of helpful user guides
	- OVREF100

# From field to the cloud
- Grid
- Enterprise SCADA & APM
- Site SCADA
- Site controls
- Field equipment and devices

"Data that resides at the edge or in a central location"
"Central visibility"
# Elements of an Ovation System
- Database server (750k per seconds)
	- 1 fast points = 10 normal "slow" points 
- Ovation highway
- Ovation controllers (254 drops per highway) (field device and external data link)
- Ovation process historian
- ELC module (field device and external data link)
- Embedded CPS (field device and external data link)
	- Specific to Ovation Green
	- OCC / OCR / SDC
- SCADA server (up to 128k points per device) (field device and external data link)
	- Ability to feed OPH directly, bypassing the Ovation Highway to reduce burden on the highway
	- It is on the same network bypassing the DDB
- Omitted:
	- domain controller
	- engineering workstations
	- network equipment
 Software defined control
  - will be able to replace the SCADA servers

Incorporating datacenters, such that one operator can access 50+ sites at once. 


# An Expanded Ovation System
- Up to 300 networks, 225 million total points.


# Ovation Green SCADA
- Clustered controllers with redundancy
- Green elements are granular and so require a lot of points!
- Control data comes into a controller, which the monitoring data comes into a local data collector node.


# Scan rates & Configuration
- Shortcut: edit as an XML
- Scanblocks
- "RTU" = remote terminal unit
- CPS = communication protocol suite
- Scan type
	- Periodic: Normal scan interview
	- Exception: Used for output scan blocks. Writing commands to a field device. Typically used only when a value of a configured point changes.
	- Trigger: executes on state change for a defined trigger point
	- Mapping ("SCADA 226" command)
- Minimize scanblocks to optimize throughput
	- Modbus read registers can span up to 125 registers
- Packets have a structure
	- The header has the source and destination IP address.
	- The payload
	- Minimize scanblocks to minimize packets
	- OPC requires big tagnames, so is the worst for throughput and maximizing point count
-

# Historian configuration
- Storage types: Main, main extended, archive
- Scan frequency
	- Fast (0.04 to 0.1 sec)
	- Standard (1 to 3600 sec)
- Deadband algorithm: can define how changes in analog points are filtered by the historian. Once the point goes out of the deadband, the value will "historize".
	- STANDARD
	- RATIO
	- LOG
	- POWER
	- FLOW
	- SLOPE
	- RADIATION
	- PCT_RANGE
# Best practices if you are nearing point limits
- packed points - significantly decreases OCB points
- Packed IO - plan how to unpack in the SCADA config (best practice) or unpack in control sheets to allow for flexibility regarding where points originate
- SCADA 226 command - directly set a datalink value without adding OCB points
- VCALC command - complex mathematical operations to avoid OCB points
# Replication
- Control macros, to bundle logic
- Object builder
	- Attributes allow for programmatic configuration to site specific needs

# Data mapping methodology
1. Each device is entered as an asset into a hierarchy
2. Signal map templates are created for each type
3. Normalized data classes are defined to organize data independent from individual assets
4. Data storage organized by asset, by data class, and by time to allow for dynamic retrieval
5. Various interfaces can query data in efficient and flexible ways