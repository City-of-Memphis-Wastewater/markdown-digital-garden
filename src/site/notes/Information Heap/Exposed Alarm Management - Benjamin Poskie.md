---
{"dg-publish":true,"permalink":"/information-heap/exposed-alarm-management-benjamin-poskie/","noteIcon":"","created":"2025-07-29T13:17:00.825-05:00"}
---

Date: [[-Daily Activity Log-/2025 07-July 29\|2025 07-July 29]]

#ovation 
key words and phrases
- alarm rationalization
# Alarm Metrics
- We need a way to measure ourselves.
- Compare your performance over time,
- Metrics: Alarms per hour, priority distribution ratio, and more.

# Philosophical standardization - database
- Alarm conventions
- Rationalization and justification
- How to expedite alarm rationalization?
- Choose priority levels.
	- Bulk priorities and bulk parameters, and then come back and do case by case one by one.
	- Do not do one by one first - it'll kill your soul.
	- And then, "Does it match the default I picked, or do i need an exception?"
## How to bulk edit?
- Digital
	- save as text file and transfer to corporate laptop to import into spreadsheet for further processing
- Analog
- other
	- Set default for non-alarms
	- set defaults for hardware: drops, node, modules, pack 16, ethernet
	- establish means to filter alarm screen for priority 1,2,3
- In developer studio, filter
	- every point that ends in "fail to start", "fts", "fail to close", "ftc", etc
- Standards - mention these in the contract, using parlance like "follow the guidelines". Don't use parlance like "comply"
	- ISA18.2
	- IEC 62682

# Standardized control logic
- Consistent point names
- Consistent descriptions
- Without these, it'll be a real tough job to bulk edit, because your text search filtering will be no bueno
- "First out algorithm"
- "Give the automatic function a change to work. If it doesn't work, tell the operator."


"lower inhibit"
"raise inhibit"


# Alarm statistics and quantitative analysis
- Find most frequent ("chattering") alarm 
- Come to historical review - how often did it alarm for the past month
- For most tanks, they are big enough that a 10 second alarm delay does not matter.
	- You can choose to delay the alarm
	- This introduces risk
- On the return side
	- You can choose to hold the alarm for extra time
	- Holding allows you to combine chattering alarms.
- Process value

# Exceptions to the standardization
- being right from an engineering perspective versus being right from an operator perspective.

# Suppression Logic
- It is easy to say when to activate the suppression
- It is hard to say when to release the suppression
- "Ask the opposite question"
	- Assume it is always disabled. When should i enable it?
- Mainly low is considered. High alarms usually aren't suppressed, from a safety perspective.
- Bit patterns are used to look at high 1, high 2, high 3, high 4, high user, low 1, low 2, low 3, low 4, low user. Keyword: masking. Bit 15 is special.
- "Sliding pressure mode"
- "Cutout off delay" - prolongs the suppression for when it is active.

Benjamin.Poskie@Emerson.com
ANSI/ISA 18.2, EEMUA 191, IEC 62682
Nimmo, I. "Rescue Your Plant from Alarm Overload"
Nimmo, I. "Abnormal Situational Awareness - The Need for Good Situational Awareness" 


Agile Ops

Do alarms first, before upgrading alarms.

Control action, and then operator (alarm) action.

Ben can see the idea of external configuration from a central database, but the API is over his head.