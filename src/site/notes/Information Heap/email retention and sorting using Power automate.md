---
{"dg-publish":true,"permalink":"/information-heap/email-retention-and-sorting-using-power-automate/","noteIcon":"","created":"2026-03-31T14:34:13.508-05:00"}
---

Date: [[-Daily Activity Log-/2026 03-March 31\|2026 03-March 31]]

```
what about power automate? What can it access in terms of data sources? becuase what i could do is have list of emails, by reference ID, that should be backed up. And when a new item is added to this list, power automatee is triggered to perform the backup and retention

ergo, the machincation doesnt need to be done from python or a local machine, all that needs to be done is the contents of the list is impacted

what are good options for keeping this list? sharepoint, some file in onedrive, even a github repo file

or there could be an email that does the triggereing - with a particular subject, and then an "add to list" command, and the list is maintained in the same place as the email backups and only edit by power automate

```
This is a high-level architectural shift that completely bypasses **ThreatLocker** and **WSL** limitations. By moving the logic to **Power Automate (Microsoft 365)**, you are moving the "machination" to the cloud where the City's retention and sync rules can be manipulated natively.

### Power Automate: Data Source Capabilities

Power Automate is designed to "glue" these exact services together. For your **10-Year Project Archive**, it can access:

- **Outlook 365:** Read emails, move them to folders, and extract attachments.
    
- **SharePoint / Microsoft Lists:** The best "database" for your list of Reference IDs.
    
- **OneDrive for Business:** To save PDFs or `.eml` copies of the emails.
    
- **HTTP Request / GitHub:** It can even push data to a GitHub repo via API if you want your archive in a version-controlled environment.
    

---

### The "Trigger-Based" Archive Architecture

Instead of a local Python script "pulling" data, you create a "Push" system.

#### 1. The "List" (The Brain)

**Option: Microsoft Lists (SharePoint-backed)**

- **Why:** It is human-readable, searchable, and has a native Power Automate trigger ("When a new item is created").
    
- **Columns:** `Reference ID` (e.g., REQ0057391), `Status` (Active/Archived), `Last Updated`.
    

#### 2. The Triggering Mechanism

You can trigger the backup in two ways:

- **The "List Entry" Trigger:** You manually add "REQ0057391" to your Microsoft List. Power Automate then searches your Inbox for that ID and archives everything it finds.
    
- **The "Email Tag" Trigger:** You move an email into a folder called "To Archive." Power Automate sees the new mail, extracts the `Thread-Index`, and adds it to your master list automatically.
    

#### 3. The "Backup" Action

When the flow runs, it performs these steps:

1. **Convert to PDF:** Power Automate has a "Convert File" action (via OneDrive) that can turn an email body into a PDF.
    
2. **Save to Project Folder:** It saves that PDF into `OneDrive/Projects/REQ0057391/`.
    
3. **Apply Retention:** It can move the original email to a "Legal Hold" or "Permanent" folder in Outlook that is exempt from the 6-month deletion.
    

---

### Comparison of List Options

|**Storage Option**|**Best For...**|**Power Automate Ease**|
|---|---|---|
|**Microsoft Lists**|**Project Management.** It's a GUI-friendly database.|**Excellent.** Native triggers and actions.|
|**OneDrive (Excel/CSV)**|**Simple Logging.**|**Good.** Easy to append rows, harder to "query."|
|**GitHub (JSON/MD)**|**Developer Workflow.** Keeps your notes and IDs together.|**Moderate.** Requires "HTTP" action to talk to GitHub API.|

---

### Proposed "Zero-Touch" Workflow

1. **Tag:** You receive an email. You simply assign it a **Category** in Outlook called "Archive-REQ0057391".
    
2. **Flow:** Power Automate triggers on "When an email is Categorized."
    
3. **Process:** * It parses the Category name to get the ID.
    
    - It checks your **Microsoft List** to see if this project exists.
        
    - It saves a PDF of the email to your **OneDrive**.
        
    - It moves the email to your **"Never Delete"** folder.
        
4. **Sync:** Since your OneDrive is synced to your machine, that PDF automatically appears in your **WSL/Ubuntu** environment under `/mnt/c/Users/george.bennett/OneDrive/Archives/`.
    

This solves the **Retention** problem (by moving the mail to a protected folder) and the **Backup** problem (by creating a permanent PDF on the file system) without a single line of local code that ThreatLocker could block.

Which of these "Trigger" methods feels more natural to your daily project management—tagging an email in Outlook, or adding a row to a list?

```
great

  

let's build a power automate blow that copies emails to a backup location in sharepoint, based on mutiple triggers, including an email being sent with a particular subject line, and with expected content fields to determine the email selection and the organizational routing

  

actually, where should backed up email go? maye it is not to sharepoint but just a retention change and an email folder manageed in outlook
```

Moving the logic to Power Automate is a brilliant move—it bypasses **ThreatLocker** entirely because the "work" happens in the Microsoft Cloud, not on your local machine.

### Where should the backup go?

Since you have a **10-year retention** requirement, you have two distinct paths. Here is the trade-off:

- **Option A: The Outlook "Vault" (Retention Change)**
    
    - **How:** The flow moves the email to a specific folder (e.g., `_Archive_10Year`) and applies a **Retention Tag**.
        
    - **Pro:** It stays in your Outlook search. It’s easy to reply to.
        
    - **Con:** You are still dependent on the "live" mailbox. If the account is ever de-provisioned or hit by a mailbox size limit, the data is at risk.
        
- **Option B: The SharePoint / OneDrive "Library" (True Backup)**
    
    - **How:** The flow converts the email to a **PDF** or **.eml** file and saves it to a Project Folder.
        
    - **Pro:** This is a "hard" backup. It’s a file on a disk. It survives even if the email is deleted from the server.
        
    - **Con:** You can't "reply" to a PDF.
        

**My recommendation:** Use **both**. Have the flow move the email to a protected folder _and_ save a copy as a PDF in SharePoint for your Obsidian notes to reference.

---

### The Power Automate Blueprint

We can build a "Router" flow that triggers based on your "Command" email.

#### 1. The Trigger: "When a new email arrives"

Set the folder to **Inbox**. Under **Advanced Options**, you can filter by:

- **Subject Filter:** `ARCHIVE:`
    
- **From:** Your own email address (so only you can trigger it).
    

#### 2. The "Parser" (Finding the Fields)

Use the **"Html to text"** action to clean up the email body. Then, use **Expressions** to find your fields.

- **Target Project:** `substring(body, indexOf(body, 'Project:'), ...)`
    
- **Reference ID:** `REQ0057391`
    

#### 3. The Logic (The Routing)

Use a **Switch** or **Condition** control:

- **If Project == "BioRem":** Route to the BioRem SharePoint site.
    
- **If Project == "General":** Route to your personal OneDrive Archive.
    

#### 4. The Actions (The "Backup")

1. **Export Email:** Use the **"Export Email (V2)"** action. This gets the raw file data.
    
2. **Create File:** Save that data to **SharePoint** or **OneDrive** with a name like `[RefID]_[Subject].eml`.
    
3. **Move Email:** Use the **"Move Email (V2)"** action to move the original message to your **"Never Delete"** folder in Outlook.
    

---

### How to Trigger it from Outlook

Since you wanted a specific subject line, you could just forward an email to yourself:

- **To:** Yourself
    
- **Subject:** `ARCHIVE: REQ0057391`
    
- **Body:** `Project: BioRem`
    

Power Automate will see this, look at the _attachments_ (the original email you forwarded), and process the whole thread into your 10-year vault.

### Your Next Step

To build this, do you have access to **"Power Automate for Office 365"** in your City of Memphis portal? You can check by going to `make.powerautomate.com`.

If you can log in there, I can give you the exact **Expressions** to extract that `Reference ID` from your email body. Should we start by defining the exact "Command" format you want to use in your emails?

```
My recommendation: Use both. Have the flow move the email to a protected folder and save a copy as a PDF in SharePoint for your Obsidian notes to reference.

  

I agree with this

  

discuss security risks

  

if any one knows that my triggers involve the contents of an email or subject line, they can manupulate this, like for a ddos attack
```

You’ve hit on a critical security point. When you build an automation that triggers based on external inputs (like incoming emails), you are essentially creating an **unauthenticated API endpoint**. If a malicious actor (or even a spam bot) discovers your trigger logic, they can exploit it.

Since you are working within the City of Memphis infrastructure, protecting your Power Automate quota and your SharePoint storage is paramount.

---

## 1. The "Spoofing" and "Injection" Risks

If your trigger is simply `Subject contains "ARCHIVE"`, anyone who sends you an email with that subject could:

- **Storage Exhaustion:** Send thousands of high-attachment emails to fill up your SharePoint quota (a "Storage DDoS").
    
- **Unauthorized Archiving:** Force your flow to move legitimate, sensitive emails out of your Inbox into a hidden "Never Delete" folder before you even see them.
    
- **Metadata Corruption:** If your flow parses the body for a "Project ID," an attacker could inject malicious strings or long text to break your Obsidian indexing or SharePoint naming conventions.
    

---

## 2. Security "Hardening" Strategies

To prevent these attacks, you should implement **Layered Validation** within the flow logic.

### A. The "Sender Allow-List" (The Firewall)

The very first action in your Power Automate flow after the trigger should be a **Condition** check.

- **Logic:** `If From is equal to [Your Professional Email]`
    
- **Action if False:** Terminate the flow immediately.
    
- **Why:** This ensures that even if someone spoofs the subject line, the flow only processes mail originating from _your_ authenticated account.
    

### B. The "Secret Token" (Two-Factor Logic)

Don't just rely on the subject line. Add a "Secret Key" to the body of your archive command that the flow looks for.

- **Example Body:** `REF: REQ0057391 | KEY: BlueGriffin99`
    
- **Why:** Even if an attacker knows you use "ARCHIVE" in the subject, they won't know your specific string. If the key is missing or wrong, the flow does nothing.
    

### C. Rate Limiting (Flood Protection)

Power Automate has built-in **Concurrency Control**, but you can add your own "Circuit Breaker."

- **Logic:** Use a "List Rows" action on a small SharePoint log to see how many archives have been processed in the last 10 minutes.
    
- **Action:** If the count is > 50, send you a warning email and stop. This prevents a loop or a mass-mailing attack from spiraling out of control.
    

---

## 3. Data Integrity and Privacy

- **Credential Exposure:** Ensure your "Client Secrets" or "Connection References" in Power Automate are not shared. Since this is an internal City flow, other admins might be able to see the flow logic—keep your "Secret Token" in a **Secure Environment Variable** if possible.
    
- **Over-Privilege:** When connecting to SharePoint, ensure the "Connection" only has access to the _specific_ Archive folder, not your entire Departmental site. This follows the **Principle of Least Privilege**.
    

---

## 4. The "Internal" Threat

Since you are using this for **10-year retention**, consider the "Legal Hold" aspect. If an email is moved to a "Never Delete" folder and converted to a PDF, you now have two copies of a record.

- **Risk:** If the City undergoes a "Discovery" process for a legal matter, you are responsible for both copies. Ensure your backup location in SharePoint is compliant with the City's IT security policies regarding "Shadow IT" or unofficial record keeping.
    

---

### Revised Flow Architecture

|**Step**|**Security Layer**|**Purpose**|
|---|---|---|
|**Trigger**|Subject Filter|Initial Filter (Low Security)|
|**Step 1**|Condition: `From == Me`|Identity Verification|
|**Step 2**|Condition: `Body contains SecretKey`|Content Authentication|
|**Step 3**|Data Sanitization|Prevents "Project Name" injection from breaking file paths|
|**Step 4**|Execution|Move and Copy|

**Should we start by setting up the "Secret Token" and "Sender Check" logic in the Power Automate designer?**

```
Us JSON or some format that can be pasted into powerautomate's AI chat input box to help generate a first pass of this architecture, from the triggering based on subject "ARCHIVE EMAIL", the sender check (george.bennett@memphistn.gov), and the command to make PDF and eml copies to a sharepoint location and also to move the emails to a certain folder with elevated retatnion, based on metadata field within the mail identifying the proper folder.
```

Using the "Copilot" or AI chat input in Power Automate is a great way to skip the manual "drag-and-drop" phase. The AI is trained to interpret structured natural language better than raw JSON, so I have formatted this as a **Prompt Specification**.

You can paste the block below directly into the Power Automate "Describe it to design it" or Copilot chat box.

---

### Power Automate Copilot Prompt

> **Build a flow with the following architecture:**
> 
> **1. Trigger:** When a new email arrives in the Inbox where the subject contains "ARCHIVE EMAIL".
> 
> **2. Security Filter:** Add a Condition to check if the From address is exactly "george.bennett@memphistn.gov". If no, terminate the flow.
> 
> **3. Data Parsing:** Use 'Html to text' on the email body. Use expressions to extract a metadata field labeled "Folder:" and "ProjectID:".
> 
> **4. Retention & Move:** Move the original email to an Outlook folder named "Permanent Archive" (this folder has a 10-year retention policy).
> 
> **5. EML Backup:** Use the 'Export Email (V2)' action to get the message content. Create a file in SharePoint at the site "Project Archives" using the 'Create File' action. Name the file using the ProjectID and Message-ID with a .eml extension.
> 
> **6. PDF Conversion:** Use the 'Convert File' action via OneDrive for Business to convert the email body to a PDF. Save this PDF to the same SharePoint location identified by the "Folder" metadata field.

---

### Implementation Details (For your manual adjustment)

Once the AI generates the skeleton, you will need to tweak a few specific "mechanics" that the AI often oversimplifies:

#### The "Metadata" Extraction Expression

To pull your folder name from the body (e.g., if your email says `Folder: BioRem`), you'll likely need to use this expression in a **Compose** action:

`split(first(split(outputs('Html_to_text')?['body'], 'Folder:')), '\n')[1]`

_(This looks for the word "Folder:", takes the text after it, and stops at the next line.)_

#### The "10-Year" Folder Logic

Since Power Automate cannot _create_ a retention policy on the fly (it can only move mail), ensure you have already created the folder in Outlook and applied the **"Never Delete"** tag manually as we discussed. The flow simply acts as the "porter" moving the mail into that protected space.

### Security Warning: The "Secret Token"

Before you hit "Save," I strongly recommend adding one more **Condition** after the Sender Check:

- **Check:** Does the email body contain a specific "Secret Key" (e.g., `AuthCode_99`)?
    
- **Why:** Even if someone inside the `memphistn.gov` domain mimics your email, they won't have your passkey, preventing accidental or malicious triggering by colleagues.
    

**Would you like me to write the specific Power Automate Expressions for the ProjectID and Folder extraction so you can paste them into the 'Compose' actions?**