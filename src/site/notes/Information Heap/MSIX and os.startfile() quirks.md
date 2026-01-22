---
{"dg-publish":true,"permalink":"/information-heap/msix-and-os-startfile-quirks/","noteIcon":"","created":"2026-01-16T16:12:12.503-06:00"}
---

Date: [[2026 01-January 16\|2026 01-January 16]]

# Directory paths are treated differently than file paths!


```

os.startfile(abs_path)

This is known to fail within msix from the store for launching a txt file

But it is known to succeeed within the same msix for launching the system file explorer to a path.
```

You are absolutely right to point out that conflict, and it highlights a very specific quirk of the Windows Shell.

To clarify: **`os.startfile` does not treat all "paths" equally.**

### Why the inconsistency exists

The Windows Shell (`ShellExecute`) differentiates between a **Namespace Object** (like a folder) and a **File Object** (like a `.txt` file).

- **The Folder Case (Success):** When you pass a directory path to `os.startfile`, you are asking the Shell to open a window to a location. Since `explorer.exe` is the owner of the desktop shell itself, the MSIX container allows the "request to open a location" to pass through to the host. It's viewed as a navigation request, not a process-launch request.
    
- **The Text File Case (Failure):** When you pass `report.txt`, the Shell has to:
    
    1. Look up the registry association for `.txt`.
        
    2. Find the executable (e.g., Notepad).
        
    3. **Launch that executable.**
        

This is where the MSIX sandbox puts its foot down. It blocks the **launch** of a second process (`notepad.exe`) via the Shell to prevent "shatter attacks" or unauthorized code execution outside the container.

### The "App Identity" Problem

Another reason `os.startfile(folder)` succeeds is that the folder doesn't belong to the app—it’s a system location. But the `.txt` file is often seen as "app data." If the MSIX doesn't have the `com.microsoft.windows.protocol` or specific "file type associations" declared in its **AppXManifest.xml**, the OS assumes the app isn't allowed to hand that file off to a third-party program.

### Summary of the Behavior

|**Target**|**Method**|**MSIX Result**|**Why?**|
|---|---|---|---|
|**Folder**|`os.startfile()`|**Success**|Interpreted as Shell navigation (Host handles it).|
|**.txt File**|`os.startfile()`|**Failure**|Interpreted as a Process Launch (Sandbox blocks it).|
|**.txt File**|`Popen(system_notepad)`|**Success**|Bypasses the Shell lookup by calling the binary directly.|

