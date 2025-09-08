---
{"dg-publish":true,"permalink":"/information-heap/this/","noteIcon":"","created":"2025-09-07T09:22:22.313-05:00"}
---

Date: [[2025 09-September 07\|2025 09-September 07]]

 Pipeline-friendly, Python-first approach that’s pipx-installable, but structured so COM integration can be added later if needed.

---

## **1️⃣ High-Level Concept**

**Goal:** Highlight text anywhere → hotkey → send text to your Python CLI (`pipeline`) → optional output (log, notification, Excel, file, etc.).

**Components:**

|Component|Role|
|---|---|
|**Clipboard watcher**|Python process that monitors the clipboard for new text.|
|**Hotkey trigger**|Global hotkey (PowerToys Run, or Python via `keyboard` library) triggers the action.|
|**CLI integration**|Sends the text to your pipx-installed CLI tool (`pipeline`) with appropriate arguments.|
|**Notification/log**|Optional: system tray notification, log file, or output to a target (Excel, file, terminal).|
|**Optional COM layer**|Future extension to integrate into apps like Word, Excel, or Explorer context menus.|

---

## **2️⃣ Architecture Sketch**

```
[User Highlights Text] 
        ↓
     [Copy] (Ctrl+C)
        ↓
[Python Clipboard Watcher]  <-- runs as background process
        ↓  (Hotkey Trigger: Ctrl+Shift+P)
[Pipeline CLI Call]  <-- runs: "pipeline query <text>"
        ↓
[Output Handling]
   ├─ Terminal log
   ├─ File (CSV, JSON, etc.)
   └─ Excel sheet or GUI
```

**Notes:**

- Python process stays **resident in the background**.
    
- Hotkey can be PowerToys Run, or native Python using `keyboard` library.
    
- CLI is executed via `subprocess`, so you don’t block the Python process.
    
- Output handling is optional — first version can just launch terminal output.
    

---

## **3️⃣ Python Implementation Plan**

**a) Background watcher + hotkey**

```python
import subprocess
import time
import pyperclip
import keyboard  # pip install keyboard

# Track clipboard state
last_clipboard = ""

def on_hotkey():
    text = pyperclip.paste()
    if not text.strip():
        print("No text in clipboard")
        return

    # Call pipx-installed pipeline CLI
    subprocess.Popen(["pipeline", "query", text])

# Register global hotkey
keyboard.add_hotkey("ctrl+shift+p", on_hotkey)

# Keep process alive
print("Clipboard launcher running... Press ESC to quit")
keyboard.wait("esc")
```

- Lightweight, cross-app solution.
    
- User copies text → presses hotkey → CLI is invoked.
    
- Can easily extend to **show notification**, log, or capture output.
    

---

**b) Optional tray integration (advanced)**

- Use `pystray` + `Pillow` to create a system tray icon.
    
- Right-click tray menu could provide:
    
    - Enable/disable clipboard watcher
        
    - Open logs
        
    - Configure CLI options
        

---

**c) Packaging as pipx installable**

- Standard Python CLI package (`setuptools` or `poetry`).
    
- Entrypoint: `clipboard-launcher` → installs script that runs the watcher.
    
- Users can run `pipx run clipboard-launcher` or `pipx install clipboard-launcher`.
    

```
clipboard-launcher/
├── pyproject.toml
├── clipboard_launcher/
│   ├── __main__.py
│   └── watcher.py
└── README.md
```

- `__main__.py` launches the watcher and registers hotkey.
    

---

## **4️⃣ Future COM/Explorer Integration**

When you’re ready to go beyond hotkeys:

1. **COM Add-in** (C# or C++): hooks into apps like Word, Excel, Explorer.
    
    - Exposes a “Run pipeline-eds” command to right-click menus.
        
    - Python CLI can remain the backend.
        
2. **Python bridge**:
    
    - COM add-in triggers Python CLI via `subprocess`.
        
    - CLI can also log, send to Excel, or do anything your watcher currently does.
        
3. **Benefits**:
    
    - First version works fully in Python, pipx-installable.
        
    - Later, you add **native right-click integration** with minimal changes to the Python backend.
        

---

## **5️⃣ Advantages of this approach**

- Works **everywhere**, not just Explorer or Office.
    
- **Python-only first version** → easy to distribute via pipx.
    
- Hotkey + clipboard is intuitive for users.
    
- Future COM integration doesn’t require changing your Python CLI.
    
- Modular: you can add tray, logging, or Excel output later.
    

---

Steps:
**clipboard → hotkey → CLI → output → COM extension** 