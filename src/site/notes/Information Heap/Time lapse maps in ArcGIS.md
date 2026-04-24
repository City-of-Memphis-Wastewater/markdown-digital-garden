---
{"dg-publish":true,"permalink":"/information-heap/time-lapse-maps-in-arc-gis/","noteIcon":"","created":"2026-04-21T16:09:04.436-05:00"}
---

Date: [[-Daily Activity Log-/2026 04-April 21\|2026 04-April 21]]

```
links
Web editor / "app": https://memegis.maps.arcgis.com/apps/instant/webeditor/index.html?appid=8e4b7061c98e4ea39b43ef6edf44e9ad

Map viewer: https://memegis.maps.arcgis.com/apps/mapviewer/index.html?webmap=fdfdcb841bbf43aeab1a362072d533ae 
```

That is a fascinating way to approach GIS—shifting it from a static "snapshot" of the plant into a dynamic **4D Historical Record**.

To answer your question: **Yes, a workflow for this is standard in high-end asset management**, often referred to as **Temporal GIS** or **Time-Enabled Data**.

---

### Q&A: Temporal Mapping and Lifecycle Tracking

**The Question:**

> "If we add metadata to each piece of equipment to track its creation (install) and removal (decommissioning) dates, can we configure a map viewer to show only 'present-day' assets while retaining historical data for time-lapse analysis? Does a specific workflow exist to manage this without deleting features?"

**The Answer:**

#### 1. How do you track the "Time" metadata?

You add two specific Date fields to your attribute table: `Install_Date` and `Retire_Date` (or `Decommission_Date`).

- **Active Equipment:** Has an `Install_Date` but the `Retire_Date` remains **Null**.
    
- **Removed Equipment:** Both fields are populated.
    
- **Future Equipment:** Has a future `Install_Date`.
    

#### 2. How do you filter the "Present Day" view?

Instead of deleting a feature when it’s removed from the plant, you simply update its `Retire_Date`. In your Web Map, you apply a **Definition Query** (filter):

- `Install_Date <= CURRENT_TIMESTAMP AND (Retire_Date IS NULL OR Retire_Date > CURRENT_TIMESTAMP)`
    
    This ensures the viewer only sees what is currently on the floor, effectively "hiding" the ghosts of old equipment and the plans for new ones.
    

#### 3. How do you perform the "Time-Lapse" analysis?

ArcGIS has a built-in **Time Slider** widget. Once you enable "Time" on the layer properties and point it to your date fields, a slider appears at the bottom of the map. As you slide it from 1990 to 2026, you will see equipment appear as it’s installed and disappear as it’s retired.

#### 4. Does a formal workflow exist for this?

Yes. In professional GIS circles, this is known as **State-based Archiving** or **Lifecycle Management**.

**The standard Order of Operations is:**

1. **Schema Design:** Add `Status` (Active/Retired/Proposed), `Install_Date`, and `Retire_Date` to your feature classes.
    
2. **Editor Tracking:** Enable this so you know _who_ recorded the removal.
    
3. **Web Map Filtering:** Create multiple "Views" of the same data—one for "Current Operations" (filtered) and one for "Historical Analysis" (unfiltered with Time Slider enabled).
    
4. **The "Sync" Workflow:** When you identify equipment to be removed, you edit it in the Web Editor, change the status to "Retired," and input the date. It immediately vanishes from the "Current" map but remains in the database for your next meeting or report.
    

---

### The Benefit for Plant Engineering

This is incredibly useful for **Digital Twins**. Instead of having five different CAD files for "As-Built 2010," "As-Built 2018," and "Current," you have **one** GIS layer that contains the entire history of the facility. You can literally "rewind" the map to see which pump was in a specific station during a 2022 failure event.

**Would you like help writing the specific SQL filter (Definition Query) for your current web map to set up this "Present Day" view?**