---
{"dg-publish":true,"permalink":"/information-heap/com-component-object-model/","noteIcon":"","created":"2025-09-07T08:48:13.571-05:00"}
---

Date: [[2025 09-September 07\|2025 09-September 07]]



## 🧩 What COM Is

**COM** = **Component Object Model**

- A Microsoft technology from the 1990s.
    
- It defines how software components can talk to each other on Windows, even if they’re written in different languages (C++, VB6, .NET, etc.).
    
- Think of it as Windows’ _plug-in system_ for applications and the OS.
    

---

## 🔑 Where You See COM in Practice

- **Office add-ins**: Excel, Word, Outlook — when you load a "COM Add-in," it’s literally a DLL that exposes COM interfaces.
    
- **Windows Shell extensions**: The right-click menus, thumbnail providers, property sheets in Explorer → these are COM objects registered in the registry.
    
- **ActiveX controls**: Old but still around — embeddable components in IE, Excel forms, etc.
    
- **System APIs**: A lot of Windows internals (DirectX, WMI, Shell.Application, etc.) are exposed as COM objects.
    

---

## ⚙️ How It Works

- A COM component is identified by a **CLSID** (a GUID like `{00024500-0000-0000-C000-000000000046}` for Excel).
    
- Programs ask Windows: _“Give me the object for this CLSID”_.
    
- Windows loads the DLL/EXE that implements it.
    
- The program then calls methods on it via **interfaces** (kind of like abstract base classes).
    

---

## 📌 Why It Matters to You

- If you want **Excel context menu integration** beyond VBA, you’d write a **COM add-in** (usually in C# or C++).
    
- If you want **Explorer text/context menu extensions** that go deeper than the registry, you’d write a **COM shell extension**.
    
- Python can **consume COM objects** (e.g., `import win32com.client; excel = win32com.client.Dispatch("Excel.Application")`), but writing a COM component in Python is possible only with extra libraries (pywin32), and it’s awkward compared to C#/.NET.
    

---

✅ **In short:**  
COM is Windows’ old but still critical "component glue system."

- Registry tricks = _lightweight shell hooks_.
    
- COM = _real integrations_ (Explorer extensions, Office add-ins, etc.).
    

---

Next steps: 
- Map out what a **COM add-in for Excel** would look like (C# example)
- **simpler VBA approach**: adding a `pipeline-eds` right-click action [[Information Heap/Python and VBA\|Python and VBA]]

Alternatively, [[Information Heap/Leverage PowerTools for hotkey functionality to call a script using input from the clipboard tray\|Leverage PowerTools for hotkey functionality to call a script using input from the clipboard tray]]