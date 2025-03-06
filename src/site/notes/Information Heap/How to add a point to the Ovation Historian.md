---
{"dg-publish":true,"permalink":"/information-heap/how-to-add-a-point-to-the-ovation-historian/","noteIcon":"","created":"2025-03-06T15:02:42.520-06:00"}
---

Date: [[-Daily Activity Log-/2025 03-March 06\|2025 03-March 06]]

Based on this document and what you know about Emerson Ovation Historian, how do you enter a point into the historian, such that it tracks the data?

###### ChatGPT said:

To enter a point into the Emerson Ovation Historian so that it tracks data, follow these steps:

1. **Access the Historian Configuration Tool**:
    
    - Open the **Historian Configuration Tool window**.
2. **Navigate to the Historian Details Tab**:
    
    - In the **left pane**, locate and open the **Historian Details** tab.
3. **Select a Scan Group**:
    
    - In the **Scanner [Name]** group, choose a scan group.
    - Click the **Point List** tab in the **right pane**.
4. **Add a New Point**:
    
    - Click **Add** in the **Edit ribbon bar**.
    - This opens the **Add Points to Scan Group** window.
5. **Select Points to Add**:
    
    - Choose the points you want to track.
    - Use `Shift` or `Ctrl` to select multiple points.
    - Click **Add** to insert them into the scan group.
6. **Modify Point Settings (if needed)**:
    
    - After adding points, you can modify properties such as:
        - **Deadband algorithm** (e.g., `STANDARD`, `PCT_RANGE`, `LOG`).
        - **Scan frequency** (to determine how often data is collected).
        - **Processing type** (to define aggregation or raw data collection).
    - Click **Modify** in the **Edit ribbon bar** to adjust these settings.
7. **Validate and Apply Changes**:
    
    - Click **Validate** in the **Home ribbon bar** to ensure consistency.
    - Click **OK** to confirm validation.
    - Click **Apply** to save changes.
8. **Load Configuration to Historian**:
    
    - In **Ovation Developer Studio**, right-click the workstation.
    - Select **Load** to apply the new configuration to the historian.

After following these steps, the point should be actively tracked by the historian​.

Sources

4o

##### You said:

How do i access the historian config tool? In developer studio? Is there alternative access?

###### ChatGPT said:

You can access the **Historian Configuration Tool** in **Ovation Developer Studio** using the following steps:

1. **Open Ovation Developer Studio**:
    
    - Refer to the **Ovation Developer Studio User Guide** if needed.
2. **Navigate to the Historian Servers Folder**:
    
    - Expand the system tree and go to:  
        `Systems -> Configuration -> Historians -> Ovation Process Historian Servers -> Ovation Process Historian Servers`
3. **Right-click on the Historian Workstation**:
    
    - Select **"Engineer"** from the menu.
4. **Launch the Historian Configuration Tool**:
    
    - This will open the **Historian Configuration Tool window**, where you can configure historians, scan groups, points, and storage settings​.

### Alternative Access:

There is no mention of an alternative standalone way to access the **Historian Configuration Tool** in the document. However, if your setup allows, you might be able to launch it separately if it is installed on your system outside of **Ovation Developer Studio**. If you need specific access outside of Developer Studio, check with your **system administrator** or **Emerson documentation** for additional options.