---
{"dg-publish":true,"permalink":"/information-heap/geojson-to-ovation-graphic-src-transpiler-python-project/","noteIcon":"","created":"2026-04-23T12:41:50.268-05:00"}
---

Date: [[-Daily Activity Log-/2026 04-April 23\|2026 04-April 23]]

Imagine you have two friends who speak completely different languages, and they both want to draw a map of your school’s playground.

One friend, **GeoJSON**, is a world traveler. He uses a GPS and talks in big numbers like latitude and longitude to describe exactly where the swings and slides are on the planet.

The other friend, **Ovation**, is a computer artist who only draws on a special grid that is exactly **16,384 squares wide** and **16,384 squares tall**. He doesn't care where the playground is in the world; he just needs to know which square on his grid to color in.

---

### What is GeoJSON?

Think of **GeoJSON** like a digital "Connect the Dots" list. It’s a text file that says:

- _"Dot 1 is at these Earth coordinates."_ * _"Dot 2 is at these Earth coordinates."_ * _"Connect them to make a blue pipe."_
    

It's great for maps, but the computer running the water plant can't read it.

### What is Ovation SRC?

**Ovation SRC** is the "Instruction Manual" for the computer screens at the water plant. Instead of Earth coordinates, it uses the 16k grid. It says things like:

- _"Draw a RECTANGLE starting at square 5000."_
    
- _"Make it GREEN if the water is moving."_
    
- _"Make it RED if there’s a problem."_
    

### What is the Transpiler?

The **Transpiler** is the "Magic Translator." Since we don't want to spend all day manually drawing 10,000 pipes and guessing which square they go in, we build a robot (the transpiler) to do it for us.

1. It reads the **GeoJSON** "Dots."
    
2. It does some quick math to shrink the Earth coordinates down to fit on the **Ovation** 16k grid.
    
3. It writes the **SRC** "Instruction Manual" automatically.
    

### Why does it matter?

At the T.E. Maxson water plant, it’s really important that the picture on the screen matches the real pipes in the ground.

If we use a transpiler, the map is **perfect**. If a worker needs to fix a sensor, they can look at the screen and know exactly where it is in the real world. It saves time, prevents mistakes, and makes sure the water stays clean for everyone in Memphis!


---

For a college audience, we shift from "robots and crayons" to **Systems Architecture**, **Data Interoperability**, and **Industrial Lifecycle Management**.

---

## The GeoJSON-to-Ovation Pipeline: A Technical Brief

### 1. The Architectural Gap

**GeoJSON** is a standard for encoding geographic data structures using JavaScript Object Notation. It is **coordinate-agnostic** (though WGS84 is the standard) and **stateless**. It describes "What is where" in a global context.

**Ovation SRC** is a **proprietary domain-specific language (DSL)** used to define Human-Machine Interface (HMI) graphics for Emerson’s Ovation Distributed Control System (DCS). It operates on a **normalized 16-bit integer coordinate system** ($0$ to $16384$) designed for deterministic rendering on industrial workstations.

### 2. The Transpilation Process

The "Transpiler" is a software shim that performs **Coordinate Normalization and Protocol Translation**.

- **Spatial Projection:** It maps real-world surveyed coordinates (e.g., Tennessee State Plane) into the local Cartesian grid of the Ovation canvas.
    
- **Semantic Mapping:** It translates GeoJSON "Properties" (metadata) into Ovation "Dynamics." For example, a `status` field in GeoJSON becomes a `COLOR_CHANGE` trigger in the `.src` file, linked to a specific DCS point.
    

### 3. Why it Matters

In critical infrastructure (like the T.E. Maxson Wastewater plant), the **Digital Twin** must be high-fidelity. By automating the conversion from GIS (Geographic Information Systems) to DCS Graphics, we ensure that the operator's view is geographically accurate, reducing human error during field maintenance.

---

## Functional Comparison: Ovation vs. GeoJSON

While GeoJSON is the king of web mapping, it is functionally "blind" to the requirements of real-time industrial control.

|**Feature**|**GeoJSON**|**Emerson Ovation**|
|---|---|---|
|**Data Flow**|Static/Snapshot|Real-time Bi-directional (Multicast)|
|**Logic**|None (Data only)|Conditional Triggers (`IF/THEN`), Alarms, Pokes|
|**Interactivity**|Requires external JS|Native "Control Pokes" to hardware|
|**Deterministic**|No (Browser dependent)|Yes (Hard-coded refresh rates)|

**What Ovation can do that GeoJSON can't:** Ovation provides the **Binding** between a visual pixel and a physical PLC (Programmable Logic Controller) register. It handles the high-speed telemetry (100ms updates), alarm priority handling, and security auditing required to keep a city's infrastructure from failing—things a static JSON file is never designed to do.

---

## Is Ovation Outdated?

The answer is a nuanced **"No, but it is Legacy-Integrated."**

In the world of "Big Iron" industrial automation, "modern" looks different. While the `.src` format feels like something from the 1990s (and it is), the underlying system is built for **99.999% uptime**.

- **The UI/UX:** Compared to React or Three.js, it looks "outdated."
    
- **The Reliability:** It is state-of-the-art in terms of safety-critical stability.
    
    Most modern DCS systems (including Ovation 3.8+) are moving toward **HTML5/Web-based graphics**, but the core logic still relies on these stable, legacy text-based "source" files because they are easy to audit and version control.
    

---

## Ethics in Industrial Software

As an engineer, you don't just build tools; you manage the power those tools exert over users and municipalities.

### Discussion Questions

1. **The Lock-in Dilemma:** Analyze the Emerson Ovation ecosystem. By utilizing proprietary formats like `.src`, `.diag`, and `.exp`, does Emerson prioritize system integrity, or is this a calculated "User Lock-in" strategy designed to prevent competition from open-source alternatives? Is it ethical to force a taxpayer-funded utility into a 20-year proprietary contract?
    
2. **Safety vs. Transparency:** If an industrial software is "Open Source," it is easier to fix, but also easier for bad actors to find vulnerabilities. In critical infrastructure (wastewater, power, nuclear), does the "Security through Obscurity" provided by proprietary formats offer a valid ethical defense?
    
3. **Data Sovereignty:** Who owns the geographic data of a city's sewers? If the data is trapped in a format only one company’s software can read, has the city lost sovereignty over its own physical infrastructure?
    

**Given the complexity of the Maxson plant, do you think the efficiency gained by the transpiler outweighs the ethical concerns of deepening the tie to a proprietary vendor?**