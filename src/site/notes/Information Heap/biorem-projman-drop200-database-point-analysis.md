---
{"dg-publish":true,"permalink":"/information-heap/biorem-projman-drop200-database-point-analysis/","noteIcon":"","created":"2026-03-10T14:45:56.664-05:00"}
---

Date: [[-Daily Activity Log-/2026 03-March 10\|2026 03-March 10]]

## Gemini said

Based on the SQL*Plus output you provided, here is the full breakdown of the tables available in the database. Since you are specifically looking for points related to the **BioRem refurbishment** (points starting with `AIR4100` or `BR_`), I have highlighted the most relevant schemas first.

+1

### **1. Critical PTADMIN Tables (Point Administration)**

These are the tables where your point metadata is most likely stored. Your previous query failed on `PTADMIN.POINTS`, but these tables exist and likely contain the data you need:

- **`PTADMIN.OBJECT_DYNAMIC`**: Often stores the real-time status and values for DCS points.
    
- **`PTADMIN.OBJECT_STATUS`**: Contains the current state of objects/points in the system.
    
- **`PTADMIN.NETWORK_DYNAMIC`**: Relevant if you are looking for point data as it traverses the DCS network.
    
- **`PTADMIN.EXTPROC_POINTLIST`**: This was one of the few tables that appeared when you filtered for "%POINT%".
    
    

### **2. Other Relevant Plant Schemas**

- **SCADA Schema**:
    
    - `SCADA.SCADA_VALUE_MASTER`: Likely stores the engineering values and scales for SCADA-connected points.
        
        
    - `SCADA.SCADA_OBJECT_DYNAMIC`: Real-time data for SCADA objects.
        
        
- **PTAUDIT Schema**:
    
    - `PTAUDIT.AUDIT_LOG`: Useful if you need to see who changed a point description or alarm limit.

- **PTXML Schema**:
    
    - `PTXML.FF_DEVICE_REF`: Important if the BioRem system uses Foundation Fieldbus devices.

### **3. Why "PTADMIN.POINTS" Failed**

The table `PTADMIN.POINTS` is not in the `all_tables` list. In many Ovation versions, "POINTS" is actually a **View** (a virtual table combined from others).

**To find the real point list, try running this command now:**

SQL

```
SELECT owner, view_name 
FROM all_views 
WHERE owner = 'PTADMIN'
ORDER BY view_name;
DESC PTADMIN.OBJECT_ATTRIBUTE_VIEW;
```

### **4. System Summary (For Reference)**

The database contains thousands of tables, mostly for internal Oracle and Ovation operations:

- **SYS**: Core Oracle system tables (e.g., `ACCESS$`, `AUD$`, `COL$`).
    
- **GSMADMIN_INTERNAL**: Global Service Manager tables for database routing.
    
- **DBSNMP**: Oracle Management Agent tables for performance monitoring.


---

## PTADMIN views
- DDBVIEW
- DROP_TYPE_VIEW
- FULL_VALUE_VIEW
- NVLXLAT_SEARCH_LOOKUP
- OBJECT_ATTRIBUTE_VIEW
- OBJECT_HIERARCHY_EXPORT_VIEW
- OBJECT_HIERARCHY_VIEW
- OBJECT_TYPE_RECORD_VIEW
- SYS_DISTDB_ATTRIBUTE_VIEW
- SYS_DISTDB_HIERARCHY_VIEW
- SYS_DISTDB_VALUE_VIEW
- SYS_LOAD_VIEW
- SYS_OBJECT_ATTRIBUTE_VIEW
- SYS_OBJECT_VALUE_VIEW
- SYS_OBJECT_XFORM_VIEW
- SYS_OBJECT_XFORM_VIEW_ALL
- SYS_SID_VIEW
- SYS_TYPES_ATTRIBUTE_FULL_VIEW
- SYS_TYPES_ATTRIBUTE_VIEW
- TYPES_ATTRIBUTE_FULL_VIEW
- TYPES_ATTRIBUTE_VIEW
- VALUE_SEARCH_ATTR
- VALUE_SEARCH_LOOKUP
- VALUE_VIEW


---

Comparing the `OvPtExport.exp` file data with the list you just provided, we can clearly see the architecture for **4001.diag** (likely the Biogas/BioRem Diagnostic Graphic).

This isn't just a list of points; it's a map of a **sequenced sampling system**. Here is what the data reveals about how the plant is actually monitoring these points:

### 1. The Multi-Tank Sampling Logic

The **AIR4100** series is a "Common Header" analyzer system. Instead of having 5 expensive H2S/Methane analyzers, the plant has one analyzer that switches between different sample lines using a manifold.

- **Common Points (Sample Inlets):** `AIR4100-11` through `AIR4100-16` are your baseline "Inlet" readings.
    
- **The Individual Tanks (1-4):**
    
    - **Tank 1:** `AIR4100-21` to `26`
        
    - **Tank 2:** `AIR4100-31` to `36`
        
    - **Tank 3:** `AIR4100-41` to `46`
        
    - **Tank 4:** `AIR4100-51` to `56`
        
- **The Common Outlet:** `AIR4100-61` to `66` (This is the final treated gas leaving the facility).
    

### 2. The "Active" Sample Indicator

The points `AIR4100-10`, `-10-B`, `-10-C`, and `-10-D` (Label: `CUR SMPLE PT`) are the status flags. On your **4001.diag** graphic, these points likely drive a highlight or an arrow showing which tank is currently being sucked into the analyzer.

### 3. Critical Alarm Health (The AAH Series)

The **AAH4100** series (A through J) are the internal diagnostic bits for the analyzer itself. For your audit, pay close attention to:

- **AAH4100-E (SMPLE FLO LO):** If this is active, none of your BR_ or AIR4100 readings can be trusted because the pump isn't pulling gas.
    
- **AAH4100-D (COND PRESENT):** This indicates moisture in the sample lines—a common killer of H2S sensors in wastewater plants.
    

### 4. Direct BioRem Instrumentation (BR_ Series)

The `BR_` points in your list provide the physical process conditions for each of the four beds. They follow a clear numbering pattern:

- **Bed 1:** `_820_1`, `_890_1`, `_830_1`, `_350_1`, `_181_1`, `_180_1`
    
- **Bed 2:** `_820_2`, `_890_2`... (and so on through Bed 4)
    

---

### Audit Conflict Found

There is a naming discrepancy between your `OvPtExport.exp` and this IDCS list that you should verify:

- In the **Export**, `BR_AIT_350_1` was labeled "Bed 1 Outlet H2S".
    
- In this **IDCS list**, `BR_AIT_350_1` is labeled "**RECIRC WATER PH**".
    

This suggests that the **AIR4100** series handles the **Gas** analysis, while the **BR_** series handles the **Water/Physical** parameters (pH, Temp, Flow). You’ll want to confirm if `BR_AIT_350_1` is actually a pH probe in the recirculation line rather than an H2S sensor.

Would you like me to cross-reference the `IO_LOCATION` for these **AIR4100** points in the export to see if they are on the same Modbus link as the BioRem PLC?