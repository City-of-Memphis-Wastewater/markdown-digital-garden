---
{"dg-publish":true,"permalink":"/training/share-point-documents-local-synchronization/","noteIcon":"","created":"2025-07-07T14:23:47.926-05:00"}
---

Date: [[-Daily Activity Log-/2025 07-July 03\|2025 07-July 03]]

# Query 0:
What is the difference between the "Sync" button and the "Add shortcut to OneDrive" button in SharePoint Documents?

# ChatGPT 0:
Great question — the **"Sync"** and **"Add shortcut to OneDrive"** buttons in **SharePoint Documents** offer different ways of accessing shared files from your computer. Here's a breakdown:

---

### ✅ **"Sync" Button**

**Function:**  
Creates a **local copy** of the SharePoint document library on your computer using **OneDrive sync client**.

**Details:**

- Downloads the full folder (or selected files) to your local machine.
    
- Files are available in **File Explorer** under the organization’s name.
    
- Changes you make sync **automatically** both ways (cloud ↔️ local).
    
- Requires **OneDrive sync app** to be installed and signed in.
    
- Good for **offline access**, large libraries, and frequent collaboration.
    

**Typical Path:**

```
C:\Users\<YourName>\YourOrgName\...
```

**Use when:**

- You want **offline access** to files.
    
- You plan to **edit and manage files often** from File Explorer.
    
- You want **two-way sync** between SharePoint and your computer.
    

---

### 📎 **"Add shortcut to OneDrive" Button**

**Function:**  
Adds a **shortcut link** to the SharePoint folder inside your **OneDrive web interface** and **OneDrive folder** on your PC.

**Details:**

- Doesn’t download files immediately.
    
- Shortcut appears in your **OneDrive** under “My files.”
    
- It behaves like a **symbolic link** to the SharePoint folder.
    
- Changes reflect in the source, but the link is not a full sync.
    
- Can cause issues if syncing across multiple accounts or libraries.
    

**Typical Path (in OneDrive):**

```
C:\Users\<YourName>\OneDrive - <YourOrgName>\<ShortcutName>
```

**Use when:**

- You want **quick access** to a few SharePoint folders.
    
- You **don’t need offline access**.
    
- You want a **lighter-weight alternative** to full sync.
    

---

### 🧠 In Summary:

|Feature|Sync|Add Shortcut to OneDrive|
|---|---|---|
|Downloads files|✅ Yes|❌ No (just a link)|
|Offline access|✅ Yes|❌ No (unless manually synced)|
|File Explorer access|✅ Yes|✅ Yes (as shortcut)|
|Two-way sync|✅ Yes|⚠️ Limited (depends on usage)|
|Good for large libraries|✅ Yes|❌ No|
|Clutter potential|⚠️ May add a lot of files|✅ Minimal|
|Stability (as of 2025)|✅ Mature|⚠️ Occasionally buggy|

---

Let me know your exact use case (e.g. how often you access it, if you need offline access), and I can recommend which is better for you.