---
{"dg-publish":true,"permalink":"/meetings/gresham-smith-site-visit-september-4th-2025-for-reliable-blower-starting/","noteIcon":"","created":"2025-09-04T07:36:24.710-05:00"}
---

Date: [[-Daily Activity Log-/2025 09-September 04\|2025 09-September 04]]

[[Contractors/Gresham Smith\|Gresham Smith]]
[[Contractors/Malasari Engineering\|Malasari Engineering]]

---
# Topic
- Blowers

---
# Purpose
- Reliable starting and control for blowers
---

# Meeting Summary

**Date/Time:** Thu, Sep 4, 2025 · 7:30am – 11am (CT)  
**Topic:** Blowers  
**Participants:** City of Memphis (Gary Garrison, Mike Brower, Chris Luss, ~~Donald Hudgins~~, George Bennett), Gresham Smith (Jason Carroll, Justin Avent, ~~Steve Walker~~, ~~Craig Parker~~), Malasari Engineering (Odell Johnson – organizer)

### Electrical & Power Issues (Gary Garrison)

- **Transformers & Feed**
    
    - Dual 23kV feeds from MLGW: one shared, one dedicated; normally run with ties open.
        
    - Transformers updated ~10+ years ago (1 MW each).
        
    - Voltage swings require “changing taps” depending on neighbors/season.
        
    - VFDs alarm (not trip) if voltage is too high.
        
    - History of unusual outages (e.g., goose strike on overhead lines).
        
- **Switchgear & Relays**
    
    - MCC installed in 1990, protection relays only monitor current.
        
    - Auto-start transformers previously failed (burned coils) → bypassed to across-the-line start.
        
    - No phase monitoring; inrush (1200A/15 sec) overwhelms 400A fuses.
        
    - Attempting to retrofit better relays (Eaton three-pole vs current two-pole).
        

---

### Blowers & Process

- **Equipment**
    
    - Maxson: six 1500 HP blowers.
        
    - Stiles: five 5000 HP blowers (operate four). Fewer issues due to full switchyard, automated switching, redundancy.
        
- **Problems at Maxson**
    
    - Reliability issues since ~2010 (after delayed upgrades).
        
    - Original blowers were water-cooled (TWAC); converted to air-cooled → overheating/fire risk.
        
    - Surge/backpressure issues when bringing on multiple blowers; bypass buttons for startup were removed in panel upgrades.
        
    - Guide vane control removed; manual operation only.
        
    - Lonestar blowers start at higher vane % to push past check valves; trying to apply this to aeration blowers.
        
    - Surge risk higher with fine bubble diffusers (7–8 psi system pressure).
        
- **Operations**
    
    - Likely won’t need all six blowers due to fine bubble efficiency.
        
    - Higher demand in winter vs summer.
        
    - Sometimes supplementing with peroxide to reduce blower load.
        

---

### Controls & Monitoring

- **Past Issues**
    
    - Control panels replaced but bypass omitted.
        
    - SCADA/automation plan abandoned after problems with Emerson project.
        
    - Bentley-Nevada health system removed; replaced by CSI/Ovation, but incomplete integration.
        
- **Current**
    
    - Limited monitoring of DO, guide vanes, or machine health.
        
    - Air filter maintenance gaps on Lonestar blowers.
        
    - Oil leaks, high vibration, lubrication, high oil temp issues.
        
    - Medium-voltage fuses cost ~$5,000 each.
        

---

### Key Perspectives

- **Jason Carroll (Gresham Smith):**
    
    - Suggest soft starts with breakers instead of fuses.
        
    - Utilities often overload B-phase; on-load tap changers may help.
        
    - Wants plan-view drawings; focus on starting reliability and process control.
        
- **Mike Brower (City):**
    
    - Main decision: fuses vs soft start/breakers.
        
    - Blowers were stable until ~2010; upgrades missed.
        
    - Notes Maxson has harsher H₂S environment.
        
- **Gary Garrison (City):**
    
    - Emphasized need for bypass/control flexibility.
        
    - Critical to integrate machine health and SCADA.
        
    - Concerned about surge and vane operation.
        

---

### Upgrades Already Done

- Automatic bar screens.
    
- Fine bubble diffusers (more efficient, lower blower demand).
    
- Clarification expansion.
    
- Bio-towers upgraded (wood → plastic media).
    
- Re-Aeration Basin with dedicated blowers.
    
- Electrical feed alterations.
    

---

### Core Issues to Address

1. **Reliable blower starting** (soft start vs fuses, bypass logic).
    
2. **Voltage management** (tap changes, feed reliability).
    
3. **Controls & automation** (guide vane control, DO-based modulation).
    
4. **Machine health monitoring** (phase, vibration, oil/lube, air filters).
    
5. **SCADA/communications** restoration for integration.
    

---


---
# Intro

[[People/Gary Garrison\|Gary Garrison]]'s rundown
- Transformers
	- YY vs Y
	- Pad switches used to be used
	- 23kv MCC
	- main-tie-main
	- two circuits coming in from the utility ([[Organizations/MLGW\|MLGW]])
		- one is shared with everyone on the road
		- one is dedicated for just us
	- the tie is bypassed; tie closed manual transition
- VFD's don't like to be over a certain voltage
	- They don't trip out - they throw an orange alarm, "check your lines"
- "Changing the taps" based on our neighbors usage, and throughout the year, particularly in the summer months
- "I don't open mains unless all the load is off. Typically we don't have an issue tripping loads."
- There was once a goose that flew into the primary lines overhead and caused browning of the substation and caused us to lose our blowers
- Typically we run our plant wit both circuits and the ties open
- 

[[Jason Carroll\|Jason Carroll]]
- Utilities often put things on B phase
- On-load might be useful to manage frequent changes
- Lots of problems with blowers and fuses
	- Do you want to go with soft start and breakers, instead of fuses.

---

# Loose Transcription
Jason:
- Lots of problems with blowers and fuses
	- Do you want to go with soft start and breakers, instead of fuses.
Mike:
- That's the decision to make now
Gary:
- We updated the transformers over 10 years ago. They are 1 MW.
Justin: That's not very big.

Odell: Is Stiles having the same issues with their blowers?
Mike: Nope.
Odell: If Stiles is not having the same issues, why?
Gary: We have six 1500 hp blowers. They use four out of five 5000 hp blowers.
Mike: The hydrogen sulfide problem is worse at Maxson compared to Stiles.
Mike: They have a north and south (A and B) side for their blowers. They have a full switch yard.
Gary: MLGW has automated switching at Stiles, which has caused trouble.
Odell: The generators are running blowers 3 and 4 at Stiles.
Gary: Our motor control center was put in 1990 when I was a 4th year apprentice. The auto-start transformer would burn up if someone hits start and the fuse is blown. This blew a coil on the two-coil transformer. For safety reasons, these have been bypassed to go across the line. We are not monitoring the phases. The old protection relays only monitor current. It will see an imbalance, but to overcome inrush (~1200 amp in-rush for 15 seconds on 400 amp fuses).
Gary: We are trying to make a new relay start on the auto transformer which is two pole. Eaton sells a three pole. The older protection relays would bypass the protection for some percentage and slowly starts dropping.

Mike: When the plant came online it has 5 and then the 6th was added almost immediately. It was designed for 6. The blowers have been fine until 2010. Things were not upgraded in a timely manner. There were two major electrical upgrade packages before 2017, which changes the way the feed comes in all together.

Gary: Originally these were TWAC water cooled motors, which used effluent water. The radiators were removed, and now it is run like a fan forced air cooled motor. The protection value was dropped down to protect it from getting over heated. Operations was told that motors are run basically unloaded until the guide vanes are adjusted by percentage, to blow more air. At one point, there was a fire, because it ran too hot. 
Gary: One issue is that we cannot monitor and guide vanes can be adjusted if we need air or not. If you're running all the air at peak for piping in the air header, pressure can build up to cause a surge and bounce back, against the check valve and bouncing against the vanes. The controls that we did tool our a vital part of starting those. When there are a couple of blowers running, there is pressure in the system - to bring up a third or fourth blowers, you have to overcome the check valve, and it will instead cause surging. There was a bypass button to allow the blower to get up to speed.  Our process has been to turn everything down to 10% and then to bring another one on. 
Gary: Our lonestare blowers start at a higher guide vane percentage (~65%) to bring the pressure up so it has a change to push the check valve open. We have started trying to use that strategy with our aeration blowers.
Mike: Piping reduces at it goes further out. In the past few years, we changes from course bubble diffusers to fine bubble diffusers. Now we run between 7 and 8 psi. 8.1 psi is supposed to be maximum.
Mike: We probably will never have to run all six at once ever again, because the fine bubble diffusers are much more efficient than the course bubble diffusers. So now we don't need as much air coming in, particularly in the summer time. Loading on the basins will be higher in the winter, so we will need more air.
Chris Luss: We have been using one blower plus peroxide 
Mike:
- Upgrades
	- Course bar screens changed from manual to automatic
	- The plant stopped chlorinating in 1980, because the chloramines were killing fish
	- We don't have a permit that says we have to disinfect, but we are doing it here because we are doing it at Stiles
	- Incoming electrical was altered
	- Addition of fine bubble diffuers
	- expansion of clarification system
	- Added bio-towers ([[Equipment/trickling filter towers\|trickling filter towers]];) wooden media changed to plastic.
	- [[Equipment/Re-Aeration Basin\|Re-Aeration Basin]] with dedicated blowers.

Jason:
- Scope
	- Focus is on improving starting and reliability of the six blowers
	- Do you also want control and process of the blowers? (Gary: Yes)

Gary:
- All the control panels were taken out and new ones were put in without the bypass
- There was a plan to communicate with SCADA and more automation
- At the same time, raw sewage pumps were upgraded to medium voltage frequency drives - problems with this and the Emerson project caused the whole SCADA system to die, so everything fell by the wayside, when we ran our of money.
- The original electrical engineer that designed that said we don't need a bypass button for the starting, with fully automated touch screen - we never got that far.
- The Bentley-Nevada system machine health was removed and  a new one was added - CSI / Ovation - Emerson said they would not implement the control until all six are done, and there was no way to run the stuff in manual. [[Information Heap/CSI machine health system\|CSI machine health system]]
- Air filter issue on reaeration blowers. There was not a plan to change the filters. 45,000 cubic foot blowers (lonestar). Vibration problems. Loose wires were found in one of the soft stars, but this has been corrected. High oil temp, lubrication. Air filters. But we can monitor it, and we're getting the kinks worked out.
- Guide vanes were going to be controlled by DO readings, to determine more air or less air - there is potential with the three new Lonestar blowers to be able to do this, with the headers to open up based on a set point. Right now there is no control, everything is manual. 
- The 3S blower spit oil all over the floor, and the mechanical pump pumped all the oil out.  There is an electrical pump that runs when the blower has been turned off to help cool it down, for fifteen minutes. The babbitt bearing on the gear box may have been damaged - there has been high current trying to start this thing.
- A medium voltage 400 amp fuse costs $5k

Jason:
- We want soft start with some sort of control.

Gary:
- Time interval for reduced voltage start was way too long
- We have been suggested a software, like a WEG. Magnetic contactors, fans, all the power is run overhead in a 4160 high hat. 1500 HP, 4160.
- Westinghouse was installed, before then all the switch gear was outdoors. Then the screw pump control was put into a building to protect it. 

Jason: Request for plan view drawings.
Gary: We have to do something with control and machine health.

---