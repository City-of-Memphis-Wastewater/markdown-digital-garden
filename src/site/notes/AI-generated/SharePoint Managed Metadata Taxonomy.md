---
{"dg-publish":true,"permalink":"/ai-generated/share-point-managed-metadata-taxonomy/","noteIcon":"","created":"2025-08-28T07:51:17.355-05:00"}
---

Date: [[-Daily Activity Log-/2025 08-August 28\|2025 08-August 28]]

# CG 0
Perfect 👍 — thanks for pasting the whole tree. This is actually a really strong starting point for designing a **SharePoint Term Store / Managed Metadata schema**.

Your directory tree already suggests _taxonomy groups_ you’d want to replicate in the Term Store so that documents, images, emails, and notes across different locations/projects can be tagged consistently, instead of being trapped in folders.

---

## Step 1 — Identify Top-Level **Groups**

In SharePoint’s **Term Store**, the highest level is a **Group** (usually tied to business units or security boundaries).  
For you, since this is a single plant’s workspace, we can probably define one big group:

- **T.E. Maxson Treatment Plant Metadata** (Group)
    

Later, if you federate with other plants (Maynard C. Stiles, M.C. Stiles, etc.), you’d create one group per plant.

---

## Step 2 — Define **Term Sets**

Each Term Set represents a reusable category you’ll tag documents with. Based on your folders, the following make sense:

1. **Document Type**
    
    - Analysis
        
    - Compliance Report
        
    - Encumbrance Record
        
    - O&M Manual
        
    - SOP
        
    - Equipment Drawing
        
    - Meeting Notes
        
    - Training Material
        
    - Photos
        
    - Emails
        
    - Schematics
        
    - Spreadsheets  
        _(etc. – this normalizes “notes”, “docs”, “pdf”, “exports”)_
        
2. **Knowledge Base / Heap / Projects / Records / Colloquial**
    
    - These are “content purpose buckets.”
        
    - You might not need them as folders anymore, but instead as **Metadata Term Set: Content Category**:
        
        - Knowledge Base
            
        - Records
            
        - Projects
            
        - Heap (general)
            
        - Colloquial
            
3. **Equipment & Systems**
    
    - Raw Sewage Pumps
        
    - Dewatering
        
    - Disinfection
        
    - Effluent Pumps
        
    - Flares
        
    - Lagoons
        
    - Switchgear
        
    - Clarifiers
        
    - Probes
        
    - Trickling Filters
        
    - Ovation / EDS / SCADA
        
    - _(essentially flatten out the Equipment and O&M Manual folders into a term hierarchy)_
        
4. **Vendors / Contractors (Encumbrance – B&C list)**
    
    - Active Bolt & Screw
        
    - AT&T
        
    - Grainger
        
    - Graybar Electric
        
    - Xylem  
        _(This is a big controlled vocabulary — super powerful for search, compliance, and PO tie-ins. This should live as its own Term Set.)_
        
5. **Projects**
    
    - RJN Clarity Project
        
    - COD and TOC Daily
        
    - Plant Animation
        
    - AECOM Raw Sewage Pump Project
        
    - CMAR Process Upgrade GMP4 / GMP5
        
    - _(etc. from Projects, Shareables, Modeling, Lab, etc.)_
        
6. **Compliance Areas**
    
    - NPDES Permit
        
    - TVA Biogas Report
        
    - Maxson Permit 2011
        
    - Maxson Permit 2018
        
    - _(sub-terms for appeals, comments, etc.)_
        
7. **Dates (optional, or use existing SharePoint columns)**
    
    - Year
        
    - Month  
        _(you could auto-tag from file metadata, but sometimes it’s useful to have controlled date buckets)_
        

---

## Step 3 — Example Schema Layout

Here’s what this looks like in SharePoint Term Store structure:

**Group: T.E. Maxson Treatment Plant Metadata**

- **Term Set: Document Type**
    
    - Analysis
        
    - Compliance Report
        
    - Manual
        
    - Drawing
        
    - Photo
        
    - Email
        
    - Spreadsheet
        
- **Term Set: Content Category**
    
    - Knowledge Base
        
    - Records
        
    - Projects
        
    - Colloquial
        
    - Heap
        
- **Term Set: Equipment & Systems**
    
    - Pumps
        
        - Raw Sewage
            
        - Scum
            
        - Effluent
            
    - Dewatering
        
        - Press 6
            
        - Press 7
            
    - Clarifiers
        
    - Lagoons
        
- **Term Set: Vendors**
    
    - Grainger
        
    - Xylem
        
    - Graybar
        
    - [etc., full list]
        
- **Term Set: Projects**
    
    - RJN Clarity
        
    - COD/TOC Daily
        
    - Plant Animation
        
    - AECOM Raw Sewage Pump Project
        
- **Term Set: Compliance**
    
    - NPDES
        
    - TVA Biogas
        
    - Maxson Permit 2011
        
    - Maxson Permit 2018
        

---

## Step 4 — How to Use

- Instead of dropping a document into `Compliance - Records\Monthly TEM TVA Biogas Report\August TVA Report\emails`,  
    → user drops the file into the **library** and tags it with:
    
    - Document Type = Email
        
    - Compliance = TVA Biogas Report
        
    - Date = August 2025
        
- Instead of `Dewatering Manuals from Chris-Hill\01700-002\Press 6\PLC`,  
    → file gets tagged with:
    
    - Document Type = Manual
        
    - Equipment = Dewatering > Press 6 > PLC
        
    - Vendor = Chris Hill
        

This flattens the folder nightmare into a metadata-first model.

---

👉 Question for you: do you want me to draft this schema in a format you can **import directly into SharePoint Term Store** (CSV or XML for Managed Metadata Service)? That way you could seed your term sets instead of building them manually.

---

# CB 1
```
Great, how do I generate this tag taxonomy?

Be very clear abut the steps in SharePoint
```

# CG 1


Here’s what your situation means in SharePoint Online terms:

- **“View-only access to the term store properties”** → you can’t change tenant-wide settings for the Managed Metadata Service (only a SharePoint **Admin** can).
    
- **“Add term group”** being available → you _do_ have delegated permissions to create/manage term groups within the Term Store. That means you can create your own taxonomy for your site collections or projects, even without full admin rights.
    

---

### ✅ Steps to Build Your Own Tag Taxonomy in the Term Store

1. **Go to the Term Store**
    
    - From your SharePoint site, click the **Settings gear (⚙️)** → **Site settings** (or in modern sites, use **Site contents → Site settings**).
        
    - Under **Site Administration**, look for **Term store management**.
        
    - Alternatively, from Microsoft 365 app launcher → **Admin → SharePoint → Term Store** (if you can access it directly).
        
2. **Create a Term Group**
    
    - In the left-hand navigation of the Term Store, right-click (or use the menu) → **Add term group**.
        
    - Give it a clear name (e.g., `Project Tags`, `Department Taxonomy`, `Document Categories`).
        
    - Assign yourself as a **Group Manager** and/or **Contributor** so you can edit.
        
3. **Create a Term Set**
    
    - Inside your new group, click **Add term set**.
        
    - Example name: `Document Types`, `Business Functions`, `Projects`.
        
    - Set options like whether tagging is open (any user can add terms) or closed (only you define terms).
        
4. **Add Terms (the actual tags)**
    
    - Right-click your term set → **Create Term**.
        
    - Enter the label (e.g., `Finance`, `HR`, `Legal`, `Engineering`).
        
    - You can nest terms for hierarchy (e.g., `Finance → Accounts Payable → Invoices`).
        
5. **Configure Term Properties (optional)**
    
    - Each term can have:
        
        - **Synonyms** (so “HR” and “Human Resources” mean the same).
            
        - **Translations** (if you’re multilingual).
            
        - **Custom properties** (like IDs or external references).
            
6. **Use Your Terms in a Site**
    
    - Go to a **library or list** → Settings → **Add column** → **Managed Metadata**.
        
    - Point that column to your term set.
        
    - Now, users will be able to select terms from your taxonomy when tagging content.
        

---

👉 The key thing: since you can add a **term group**, you can create and own your taxonomy. You just can’t see or edit the global/tenant-wide ones unless IT grants you **Term Store Admin** role.

---

Do you want me to outline a **minimal setup path** (fastest way to make a working tagging system for your site), or a **structured enterprise path** (best practice with governance, permissions, hierarchies)?

---

# CB 2
```
Recommend term sets and terms

```

# CG 2
```
Here’s a **SharePoint Term Store schema** that maps directly to the folders you shared (names and nesting reflect your tree). Use this to create **one Term Group** and the following **Term Sets** with terms.

---

# Term Group

**Maxson Plant – Managed Metadata**

---

## Term Set: Content Purpose (mirrors your “- Heap / - Records / - Projects / - Knowledge Base / - Colloquial / Shareables”)

- Analysis (Heap)
    
- Compliance (Records)
    
- Encumbrance (Records)
    
- Equipment (Heap)
    
- Electrical Maintenance (Knowledge Base)
    
- Mechanical Maintenance (Knowledge Base)
    
- Operations (Knowledge Base)
    
- Lab (Projects)
    
- Modeling & Animation (Projects)
    
- Ovation, EDS, & Data Monitoring (Projects)
    
- Maps, Plans, & Drawings (Records)
    
- SOPs (Colloquial)
    
- Meeting Notes (Heap)
    
- Pictures (Heap)
    
- Plant History, Past and Present (Heap)
    
- Software (Knowledge Base)
    
- Training (Heap)
    
- Shareables
    
- Goal Setting (Heap)
    
- Textbooks & Reading Material (Heap)
    
- TEM New Administration Building
    
- zEverything Else (Heap)
    

> Use this as a quick “what bucket does this belong to?” tag.

---

## Term Set: Business Area / Library (top-level folders normalized)

- Analysis
    
- Compliance
    
- Encumbrance
    
- Equipment
    
- Electrical Maintenance
    
- Mechanical Maintenance
    
- Operations
    
- Lab
    
- Maps, Plans, & Drawings
    
- Meeting Notes
    
- Modeling & Animation
    
- Ovation / EDS / Data Monitoring
    
- SOPs
    
- Pictures
    
- Plant History
    
- Software
    
- Training
    
- Shareables
    
- Goal Setting
    
- Textbooks & Reading Material
    
- TEM New Administration Building
    
- Everything Else
    

> This helps when the same file type exists in multiple places.

---

## Term Set: Document Type (from your subfolders: emails/notes/spreadsheets/etc.)

- Email
    
- Note
    
- Spreadsheet
    
- Drawing
    
- Schematic (PDF)
    
- Picture / Photo
    
- Video
    
- HMI Screen
    
- PLC Program
    
- Document (General) / Docs
    
- Literature
    
- Procedure
    
- Configuration / Config
    
- Export (Aggregate)
    
- Import
    
- Query
    
- Script
    
- Secret (Credentials)
    
- Template
    
- Link (URL)
    
- Report
    
- Addendum / Addenda
    
- Training Material
    
- Purchase Order / Template
    
- Meeting Minutes
    

---

## Term Set: Systems & Assets (from “O&M-Manuals-Maxson – Records”, Equipment, KBs)

- Raw Sewage Pumps
    
    - 1,200 HP RAW SEWAGE PUMP #SW02033
        
    - Motor Health
        
- Dewatering
    
    - Presses 1–5
        
    - Press 6
        
    - Press 7
        
    - Press 8
        
    - Mixer Panel
        
        - Drawings
            
        - HMI
            
        - PLC
            
- Coarse Bar Screens
    
- Effluent Pump
    
- Disinfection
    
- Reaeration
    
    - Reaeration Basins
        
- Scum Pumps
    
- Secondary Clarifiers
    
    - 5N
        
    - 5S
        
- Lagoons
    
    - Lagoon 2A
        
        - Pumps
            
        - Cover
            
    - Lagoon 5
        
- Flares
    
    - Varec (CDM)
        
- Medium-Voltage ATS
    
- 23kV Switchgear (Building A)
    
- BioAir
    
- BioRem
    
- Probes
    
    - MRU Optimax
        
    - MRU SWG100
        
- Ovation / EDS / SCADA
    
    - Emerson SureService
        
    - FreeWave Gateway & Endpoint
        
- CONTRACT #1 – T.E. MAXSON 42" FM (SW061)
    

---

## Term Set: Projects & Programs (from Lab, Modeling, Maps, Shareables, etc.)

- RJN Clarity Project (Analysis → Flow)
    
- COD and TOC Daily – Clayton 2025 (Lab)
    
- TSS (Lab)
    
- PlantAnimation (Modeling & Animation)
    
    - ABF Recycle Flow and Bypass Gate – Project
        
    - Aeration Basin Routing – Project
        
    - Grit Chamber – Project
        
    - Primary Clarifiers – Project
        
    - Screw Pumps – Project
        
    - zGeneral Plant Images and Blueprints
        
- AECOM Raw Sewage Pump Project (Shareables)
    
    - Drawings and Plans
        
        - Addendum 1
            
        - Addendum 2
            
        - Addendum 3
            
        - Addendum 4
            
        - Addendum 5
            
    - Photos
        
- CMAR Process Upgrade (Maps, Plans, & Drawings)
    
    - GMP4
        
        - Addenda
            
    - GMP5
        
        - Addenda
            
- Belt Filter Press Replacement (Maps, Plans, & Drawings)
    
    - Addenda
        
- Lagoon 2A (Maps, Plans, & Drawings)
    
    - Package 2
        
        - Addenda
            
- Lagoon 5 (Maps, Plans, & Drawings)
    
- Overview (Maps, Plans, & Drawings)
    
- Primary Clarifier PS (Maps, Plans, & Drawings)
    
    - Addenda
        
- Solar (Maps, Plans, & Drawings)
    
- Admin Building (Maps, Plans, & Drawings)
    
    - Addenda
        
- Raw Sewage Pumps (Maps, Plans, & Drawings)
    
    - Addendum 1
        
    - Addendum 2
        
    - Addendum 3
        
    - Addendum 4
        
    - Addendum 5
        

---

## Term Set: Compliance Programs & Artifacts (from “Compliance – Records”)

- Monthly TEM TVA Biogas Report
    
    - August TVA Report
        
        - Emails
            
        - Notes
            
        - Spreadsheets
            
    - September TVA Report
        
        - Emails
            
        - Notes
            
        - Spreadsheets
            
    - December TVA Report
        
        - Emails
            
        - Notes
            
        - Spreadsheets
            
- NPDES Compliance
    
    - 2011 Draft Maxson Permit
        
        - Appeal
            
    - 2018 Draft Maxson Permit
        
        - City’s Comments
            
        - Gary Cohen’s Draft Comments
            

---

## Term Set: Encumbrance – Document Types & References (from “Encumbrance – Records”)

- Appropriation Statement
    
- B&C
    
    - Blank B&C
        
- Encumbrance Status Report
    
- Open PO Encumbrance Report (Lines)
    
- Encumbrance Review
    
    - 052204
        
    - 052316
        
    - 052341
        
    - 052342
        
    - 052408
        
    - 052410
        
    - 052518
        
    - 052520
        
    - 052528
        
    - 053106
        
    - Approved by Don
        

---

## Term Set: Vendors (Encumbrance – B&C → B&C)

Active Bolt & Screw; Acuff Enterprises; Agilix Solution – IAC Supply; Amazing Lighting; American Electric Motors; AT&T; Badger Daylighting; Bar; BDP; Best Wade; Blue Cardinal Chem; Brighter days; Centro; Certified Laboratories; Chris Hill; Columbia Chrysler Dodge; Comserv Services; Consolidated Electric; Constructaid; Continental Research Corp; Contractor Supply; CS3; Delta Materials; Diamond Scientific; DJ Shubeck; Dobbs; Dyna Brite Lighting; EDE Engineering; Electronic Responsible Recyclers; Ellicott Dredge Tech; EMCD – Electrical & Mechanical Components; Emerson; EMICC; Enecon TN; Enfinity; ETEC for EDI Purchases; Evonik; Fastenal Company; Floied Fire Extinguish; Fullen Dock; G&W Diesel; GMW; Good Fruit Electric; Gossett Motor Cars; Grainger; Graybar Electric; GSM; Gulf States Engineering; Hach; Hexagon; Hi-speed; Hill Manufacturing; Hotsy Cleaning Equip; Industrial Crane; Industrial Sales; Insight; Inter-con Security; JR Stewart; Ken’s Racing; Kimball Midwest; Ladd; LandMark Const; Lawson Products; Lone Star; Luttrell Belting; Maint Supply & Fasteners; Malasri; MAM Machine and Manufacturing; MCC; McCrory Electric; Memphis Bearing & Supply; Memphis Chain & Cable; Memphis Ice Machine; Memphis Industrial Safety; Memphis Winwater; Metal Supply; MIC Sales; Mid-south Trucking; Midsouth Pipe and Valve; Motion Industries; Napa Auto Parts; Nexair; Overhead Door; Ozark Fluid; Polydyne; Power Tools Inc; ProChem; PSI; Quality Safe & Lock; Rebid; Rent to Kil; Republic Services; River City Soap Aleva; S&S Firestone; SCA; Scruggs; Share Corportion; Shelby Electric; Sid Boedeker Safety Shoe; Southern Steel; State Systems; Steepleton Tire Company; Steve’s Tire & Auto; Stratum; Stribling Equipment; Taborco; Tag Truck Center; Tenn-Plast Inc; Tennessee Tractor; Thomas Consultants; Thompson Machinery; Tri-State; TriNova; Unifirst; Union Auto Parts; Upchurch; USP; Velociti Services; Velodyne; Vulcan; Whitt Deming; Williams Equipment; X-cel; Xylem.

> If you prefer, keep this **closed** (only you add new vendors) to maintain spelling consistency.

---

## Term Set: Facility / Area (physical locations implied in tree)

- Admin Building
    
- 23kV Switchgear – Building A
    
- Lagoon 2A
    
- Lagoon 5
    
- Reaeration Basins
    
- Primary Clarifiers
    
- Grit Chamber
    
- Screw Pump Station
    
- Disinfection
    
- Coarse Bar Screens
    
- Secondary Clarifiers (5N, 5S)
    
- Dewatering Building
    
- Effluent Pump Station
    

---

## Term Set: Operational Timeframe (from “CY 2025 Data”, dated KBs like `2025_07July_10`)

- Calendar Year
    
    - 2011
        
    - 2018
        
    - 2025
        
- Month
    
    - January … December
        
- Day (optional)
    
    - 01–31
        

> You can also capture dates with normal Date columns; include this only if you want **faceted browsing** by Year/Month.

---

# How to apply this

- Create a **Managed Metadata column** for each Term Set you want to expose in a library (e.g., _Content Purpose_, _Business Area_, _Document Type_, _Systems & Assets_, _Projects & Programs_, _Compliance Programs_, _Vendors_, _Facility / Area_, _Operational Timeframe_).
    
- Keep _Document Type_, _Systems & Assets_, and _Projects & Programs_ required in libraries where those tags are critical; make Vendor optional except in Encumbrance libraries.
    

If you want, I can hand you **ready-to-import CSVs** for (a) Vendors, (b) Systems & Assets, and (c) Projects & Programs exactly as above so you can use **Term Store → Import terms** and avoid manual entry.
```