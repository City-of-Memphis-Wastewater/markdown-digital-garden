---
{"dg-publish":true,"permalink":"/information-heap/vibration-and-temperature-monitoring-for-preventative-maintenance-reaeration-blower-motors/","noteIcon":"","created":"2026-01-02T11:48:35.629-06:00"}
---

Date: [[-Daily Activity Log-/2026 01-January 02\|2026 01-January 02]]

```
temperature and vibration (mm/s) sensor on motor to trigger preventative maintenance - deciding the alarm value critical threshold 

Key questions:
	- What model are these motors?
	- Do we have an O&M manual that dictates the critical thresholds that should trigger a PM?
	- What temp and vibration critical thresholds are normal for motors of this type?
	  
Command:
eds trend YRI1671 YRI1661 YRI1651 VI1672 VI1675 VI1665 VI1662 VI1655 VI1652 --days 300

```
There is a meter section is [[Hexagon EAM\|Hexagon EAM]], for each Asset

I would assume there is a way to connect the temp and vibration meter values from our Ovation DCS via API to feed into Hexagon EAM, such that a PM can be triggered if some threshold value is exceeded. The alterative is for a person to manually notice an Ovation alarm for the exceeded critical threshold, but it would be better to connect directly to Hexagon. 

---

To get data from your **Ovation DCS** into **Hexagon EAM**, you essentially need to build a digital "conveyor belt" that pulls values from your control system and pushes them into Hexagon's database.

Since Ovation and Hexagon don't "speak" to each other out of the box, you use a **Middleware** (a bridge) to handle the translation.

---

### 1. Step-by-Step Data Flow

#### **Step A: Extracting Data from Ovation**

Ovation typically stores its data in a **Process Historian** (SQL-based) or provides it via **Enterprise Data Solutions (EDS)**.

- **The Method:** Your IT/OT team can use a Python script or a tool like Node-RED to query the Ovation EDS API or the SQL database directly.
    
- **The Frequency:** You don't need "real-time" (every second) data for PMs. Pulling a reading once every **15 to 60 minutes** is usually sufficient for vibration and temperature trends.
    

#### **Step B: The "Bridge" (Middleware)**

The bridge is a small piece of code that does three things:

1. **Polls Ovation:** "Give me the current value for tag `VI1672`."
    
2. **Maps the Data:** "Tag `VI1672` belongs to Asset `MOTOR-001` in Hexagon."
    
3. **Formats the Message:** It packages this into a JSON format that Hexagon understands.
    

#### **Step C: Pushing to Hexagon EAM API**

Hexagon EAM has a specific API endpoint for meter readings called **MP7051**. Your bridge will send an `HTTP POST` request to your Hexagon URL.

**What the data looks like when sent to Hexagon:**

JSON

```
{
  "equipment": "MOTOR-001",
  "meter": "VIB-01",
  "readingvalue": "3.8",
  "readingdate": "2025-10-24T10:00:00Z"
}
```

---

### 2. How to "Authorize" the Connection

You cannot just send data to Hexagon; you need a "key" for security.

1. **Go to Hexagon EAM:** Navigate to the **Integration Configuration** screen.
    
2. **Generate API Key:** Create a dedicated "User" for this integration (e.g., `OVATION_USER`) and generate an **API Key**.
    
3. **Set Permissions:** Ensure this user has "Insert" rights for the **Meter Readings** screen.
    

---

### 3. Turning the Data into a Work Order

Once the data is flowing into the **Meter** section of the Asset in Hexagon, you use **Condition Protocols** to automate the PM:

|**Hexagon Component**|**Function**|
|---|---|
|**Meter**|The container holding the $mm/s$ or $^\circ C$ values.|
|**Condition Protocol**|The logic: "If Meter > 4.5, then Action."|
|**Work Order Template**|A pre-defined WO that says "Check Motor Alignment" with the required tools/parts.|

**The result:** The moment Ovation records a vibration spike, a Work Order appears in the Mechanical Lead's inbox in Hexagon—no manual monitoring required.

---

### 4. Implementation Checklist for your IT Team

- [ ] **Identify Tags:** List all Ovation Tags (e.g., `VI1672`) and their corresponding Hexagon Asset IDs.
    
- [ ] **Access API:** Request the **REST API Swagger documentation** from your Hexagon Administrator (usually at `https://[your-url]/web/swagger/index.html`).
    
- [ ] **Service Account:** Create a technical user in Hexagon with an API Key.
    
- [ ] **Test Call:** Use a tool like **Postman** to manually push one reading to Hexagon to confirm the connection works.
    

**Would you like me to provide a sample Python script that demonstrates how to send a single meter reading to the Hexagon API?**

