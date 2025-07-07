---
{"dg-publish":true,"permalink":"/information-heap/free-simple-gui-lgpl-info-and-pavlov-distribution/","noteIcon":"","created":"2025-07-07T14:23:45.503-05:00"}
---

Date: [[-Daily Activity Log-/2025 05-May 07\|2025 05-May 07]]

Great — here's a complete setup:

---

## ✅ 1. `NOTICE` File (for LGPLv3+ compliance with FreeSimpleGUI)

```text
NOTICE

This software includes the FreeSimpleGUI library,
which is licensed under the GNU Lesser General Public License v3 or later (LGPLv3+).

You can find a copy of the LGPLv3 license at:
https://www.gnu.org/licenses/lgpl-3.0.html

FreeSimpleGUI source code is available at:
https://github.com/dfurman/FreeSimpleGUI

No modifications were made to FreeSimpleGUI by this software's author.
```

> If you **do** make changes to FreeSimpleGUI, update that last line to say:  
> "Modifications were made to FreeSimpleGUI. Modified source code is available upon request."

---

## ✅ 2. `LICENSE` File for the Rest of Your Program

If you want your **own code to be closed source and require permission to use**, you typically use a **custom proprietary license**. Here's a simple, legally conservative example:

### 📄 `LICENSE`

```text
Copyright © [2025] [Your Name or Company]

All rights reserved.

This software is proprietary and confidential. No part of this software may be copied, modified, distributed, or used without the express written permission of the copyright holder.

Permission to use, distribute, or modify this software must be obtained in writing. Unauthorized use is strictly prohibited.

This software is provided "as is" without warranty of any kind, express or implied.

Third-Party Components:
This software includes the FreeSimpleGUI library, which is licensed separately under the GNU Lesser General Public License v3 or later (LGPLv3+). See the NOTICE file for details.
```

---

## ✅ Summary

- Your software stays **closed source**, and you protect it with a simple custom license.
    
- You **comply with LGPLv3+** by documenting FreeSimpleGUI usage and license.
    
- You **don’t need to open source anything else**, as long as you don’t modify and embed the LGPL code improperly.
    

Would you like to automate this into a CLI tool that generates these files?

# Update:
Got it — since you're also using the **Autodesk FBX SDK**, which comes with its own strict **proprietary EULA**, we’ll include a clause in both the `NOTICE` and `LICENSE` files to reflect that. Autodesk's license **does not allow redistribution** of the SDK itself and limits usage to applications that interact with Autodesk software.

---

## ✅ Updated `NOTICE` File

```text
NOTICE

This software includes the following third-party components:

1. **FreeSimpleGUI**  
   Licensed under the GNU Lesser General Public License v3 or later (LGPLv3+).  
   License: https://www.gnu.org/licenses/lgpl-3.0.html  
   Source: https://github.com/dfurman/FreeSimpleGUI  
   No modifications were made to FreeSimpleGUI.

2. **Autodesk FBX SDK**  
   Provided under the Autodesk FBX Software Development Kit License Agreement.  
   The FBX SDK is subject to its own terms and conditions, which restrict redistribution.  
   For more information, see: https://www.autodesk.com/developer-network/platform-technologies/fbx-sdk-2020-0  
   This software uses the FBX SDK solely for importing/exporting 3D assets in compliance with Autodesk's terms.
```

---

## ✅ Updated `LICENSE` File (Proprietary)

```text
Copyright © [2025] [Your Name or Company]

All rights reserved.

This software is proprietary and confidential. No part of this software may be copied, modified, distributed, or used without the express written permission of the copyright holder.

Permission to use, distribute, or modify this software must be obtained in writing. Unauthorized use is strictly prohibited.

This software is provided "as is" without warranty of any kind, express or implied.

---

Third-Party Components:

1. **FreeSimpleGUI**  
   This software includes FreeSimpleGUI, which is licensed separately under the GNU Lesser General Public License v3 or later (LGPLv3+).  
   See the NOTICE file or https://www.gnu.org/licenses/lgpl-3.0.html for license terms.

2. **Autodesk FBX SDK**  
   This software uses the Autodesk FBX SDK, which is provided under the Autodesk FBX Software Development Kit License Agreement.  
   The SDK is not redistributed with this software and must be obtained directly from Autodesk.  
   Use of this SDK is governed by Autodesk’s terms: https://www.autodesk.com/developer-network/platform-technologies/fbx-sdk-2020-0

The inclusion of these components does not alter the licensing terms of the proprietary portions of this software.
```

---

## ✅ Final Notes:

- You **cannot bundle the FBX SDK binaries or headers** in your distribution.
    
- Users must **download the SDK themselves** from Autodesk.
    
- If you dynamically link to the SDK (typical), you remain in compliance — as long as you don’t hide or redistribute it.
    

Would you like these generated automatically into files in your project structure (e.g. with a `poetry run gen_license` command)?

# FBX SDK exposure

Excellent follow-up — this gets into the **nuances of Autodesk's FBX SDK license**, which is **very restrictive** but has some wiggle room depending on **how you use it**.

---

## ✅ What’s Actually True About the Autodesk FBX SDK License?

### ❌ **You Cannot Freely Redistribute the FBX SDK (binaries or headers)**

- The **FBX SDK EULA** explicitly restricts redistribution of the SDK itself.
    
- That includes DLLs, static libraries, header files, etc.
    
- You must **not package** the SDK itself into a public installer or bundle it as part of your distributed files.
    

---

## ✅ But You **Can Use the SDK in Your Software** (With Conditions)

### ✅ You _Can_:

- Build your app with the FBX SDK **and distribute your app** (without bundling the SDK).
    
- Use the SDK to **import/export FBX files** internally.
    
- Distribute your app **with FBX support** baked in — as long as **you’re not redistributing Autodesk’s SDK files**.
    

### ❗ You _Cannot_:

- Redistribute the SDK itself, even dynamically linked DLLs or header files.
    
- Use it in a **public SDK or API** where other developers can indirectly access or depend on the Autodesk SDK through your interfaces.
    

---

## ✅ So in Your Case:

> **“What if the software does not expose the FBX SDK, but just uses it to generate export files?”**

Yes — that's perfectly acceptable. **You’re using the SDK as a tool**, not exposing it or redistributing it.

You can:

- Include FBX export/import as a feature in your 3D software.
    
- Ship your software with compiled binaries that internally use the FBX SDK.
    
- Avoid giving users access to the SDK's functionality **outside** of what your software does.
    

### ⚠️ But:

- You must **not ship the Autodesk SDK DLLs** with your installer (unless Autodesk has granted you special redistribution rights).
    
- You can **document that FBX functionality is available only if the user installs the FBX SDK** (and possibly prompt them or guide them through it).
    

---

## ✅ In Summary:

|Scenario|Allowed?|Notes|
|---|---|---|
|Build software using FBX SDK|✅|As long as you don't redistribute SDK files|
|Bundle FBX SDK DLLs or headers|❌|Violates EULA|
|Use FBX SDK to export .fbx internally|✅|Fine, as long as it’s not externally exposed|
|Expose FBX API functionality to plugins or scripts|⚠️|Risky — could be interpreted as indirect redistribution|

---

Would you like a sample installer approach or user prompt that checks for the FBX SDK and guides users to install it if missing?

# Clayton 4:
the fbx sdk is rolled out in my python project using poetry and pyproject.toml to reference gitlab.irna.fr/api/v4/projects/18692/

My software is packaged into an exe (both setup and portable) using pyinstaller, which obfuscates everything except for configuration files

# ChatGPT 4:
Thanks for the detailed setup — your case is nuanced, but here’s a clear breakdown of how your **FBX SDK integration and distribution via PyInstaller and Poetry** aligns with Autodesk’s license, and what steps you should take to remain compliant.

---

## ✅ Summary of Your Setup

- You're using **FBX SDK via a private GitLab-hosted wrapper or wheel** (e.g. from `gitlab.irna.fr/api/v4/projects/18692/`).
    
- Your project is **built with Poetry** and packaged into an **EXE using PyInstaller** (obfuscating code).
    
- Your app **does not expose** the FBX SDK, only uses it internally for import/export.
    

---

## 📜 Legal Implications & Risk Areas

### ✅ **What You're Doing Right**

- ✅ Using the SDK only internally (not offering FBX as a plugin interface or API to others).
    
- ✅ Obfuscating and bundling the SDK via PyInstaller means you're not distributing it separately or as a library.
    
- ✅ Not modifying or directly redistributing Autodesk's SDK source, headers, or standalone DLLs.
    
- ✅ Using a private Git-based source for installation, likely a compiled Python wrapper.
    

### ⚠️ **Potential Risks**

1. **Redistribution of compiled FBX binaries inside your EXE** — PyInstaller may still bundle `.dll` or `.so` files from the SDK.
    
2. If the FBX SDK was not statically or completely bundled, or if the GitLab package replicates Autodesk binaries or headers, **you are technically redistributing Autodesk binaries**.
    
3. If Autodesk audits your setup and sees unlicensed distribution (even via a compiled EXE), you could be non-compliant.
    

---

## ✅ How to Stay Safe (and Professional)

### 1. **Add a NOTICE Clause**

Update your `NOTICE` file with this stronger disclaimer:

```text
3. **Autodesk FBX SDK**
This software uses the Autodesk FBX SDK internally for FBX file import/export. The SDK is not publicly exposed through this software's API or UI.

The FBX SDK is licensed under the Autodesk FBX Software Development Kit License Agreement. For terms, visit:
https://www.autodesk.com/developer-network/platform-technologies/fbx-sdk-2020-0

Any required components of the FBX SDK are used internally and only as permitted under the SDK's licensing terms.
```

---

### 2. **Document the SDK Source (If Using a Private Wrapper)**

Make clear that you’re **not packaging Autodesk binaries** separately, but relying on a **custom-built internal dependency**. You might add to `pyproject.toml` metadata:

```toml
[tool.poetry.dependencies]
fbx-wrapper = { git = "https://gitlab.irna.fr/api/v4/projects/18692/", rev = "commit_or_tag" }
```

And document in your README or LICENSE:

> The `fbx-wrapper` dependency is a private, internal Python binding to Autodesk’s FBX SDK. It is used strictly under Autodesk’s SDK license. The FBX SDK itself is not redistributed independently.

---

### 3. **Optional: Include a Runtime SDK Check**

To stay extra compliant, you could:

- Dynamically link to FBX DLLs.
    
- Prompt the user to install the FBX SDK (from Autodesk) if missing.
    
- Or, document that FBX functionality is not guaranteed unless the user installs the SDK themselves (safer in some jurisdictions).
    

---

## ✅ Conclusion

**You're probably compliant** given:

- You don’t expose the SDK,
    
- You bundle everything with PyInstaller,
    
- The SDK is used only internally,
    
- You don’t re-host Autodesk binaries or headers publicly.
    

Still, **add strong license notes** in `NOTICE` and `LICENSE`, and consider **keeping your EXE's FBX functionality modular** in case you ever need to disable or separate it later.

Would you like a PyInstaller hook to isolate the FBX SDK parts and optionally disable them at runtime?


# Clayton 5:
I use inno installer to convert my pyinstaller portable exe into a setup exe

How can i now make a msix which can be submitted to the app store?
# Chat GPT 5:
To convert your Python-based application (currently a **PyInstaller-built EXE wrapped in an Inno Setup installer**) into an **MSIX package** suitable for submission to the **Microsoft Store**, you’ll need to follow these steps:

---

## ✅ Overview

|Step|Task|
|---|---|
|1️⃣|Prepare your PyInstaller EXE for MSIX compatibility|
|2️⃣|Create a Windows Application Manifest|
|3️⃣|Use the **MSIX Packaging Tool** to wrap the app|
|4️⃣|Sign the MSIX with a trusted certificate|
|5️⃣|Submit to Microsoft Store (via Partner Center)|

---

## 🔧 1. **Prepare Your EXE**

Make sure your PyInstaller app behaves like a standard Windows app:

- No console window unless required (use `--noconsole`)
    
- Single EXE or folder structure (MSIX supports both)
    
- No admin privileges required (MSIX apps cannot request them)
    

> If your app depends on writing to system folders, registry, or `Program Files`, **refactor to use `AppData` or `LocalAppData` instead.**

---

## 📝 2. **Create an App Manifest**

Create a file like `AppxManifest.xml`. Here's a simple example:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  IgnorableNamespaces="uap">

  <Identity Name="com.yourcompany.myapp"
            Publisher="CN=Your Company, O=Your Company, C=US"
            Version="1.0.0.0" />

  <Properties>
    <DisplayName>MyApp</DisplayName>
    <PublisherDisplayName>Your Company</PublisherDisplayName>
    <Logo>Assets\StoreLogo.png</Logo>
  </Properties>

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17763.0" MaxVersionTested="10.0.22000.0" />
  </Dependencies>

  <Resources>
    <Resource Language="en-us" />
  </Resources>

  <Applications>
    <Application Id="MyApp"
                 Executable="myapp.exe"
                 EntryPoint="Windows.FullTrustApplication">
      <uap:VisualElements DisplayName="MyApp"
                          Description="3D visualization software"
                          Square150x150Logo="Assets\Logo.png"
                          Square44x44Logo="Assets\SmallLogo.png"
                          BackgroundColor="transparent"/>
    </Application>
  </Applications>
</Package>
```

---

## 🧰 3. **Use the MSIX Packaging Tool (GUI or CLI)**

### 🔽 Install

Download from the Microsoft Store:  
**MSIX Packaging Tool**  
[https://apps.microsoft.com/store/detail/msix-packaging-tool/9n5lw3jbcxkf](https://apps.microsoft.com/store/detail/msix-packaging-tool/9n5lw3jbcxkf)

### 🧱 Use It

1. Select **Application Package > Create package on this computer**
    
2. Point it to your folder (EXE and all resources)
    
3. Add your `AppxManifest.xml`
    
4. It will bundle everything and create an `.msix` file
    

> 🧪 Alternatively, use PowerShell:

```powershell
MakeAppx pack /d MyAppFolder /p MyApp.msix
```

---

## 🔐 4. **Sign the MSIX**

MSIX apps must be digitally signed:

### Options:

- Use a certificate from a **trusted CA** (required for Store submission)
    
- For testing only: use a **self-signed certificate** and install it to `Trusted Root`
    

Example (test only):

```powershell
New-SelfSignedCertificate -Type CodeSigningCert -Subject "CN=MyApp" -CertStoreLocation "Cert:\LocalMachine\My"
```

Then:

```powershell
SignTool sign /fd SHA256 /a /f mycert.pfx /p password MyApp.msix
```

---

## 📦 5. **Submit to the Microsoft Store**

1. Register at the **Microsoft Partner Center**  
    [https://partner.microsoft.com/](https://partner.microsoft.com/)
    
2. Create an **App submission**
    
3. Upload your `.msix` file
    
4. Fill in metadata, screenshots, pricing, etc.
    
5. Submit for certification
    

---

## ✅ Notes Specific to Python Projects

- PyInstaller apps are **fully compatible** with MSIX **as long as:**
    
    - You avoid hardcoded system paths
        
    - You don’t request admin rights
        
    - You statically bundle your dependencies
        

---

Would you like a full example with the directory structure and build script for packaging into `.msix` from your PyInstaller output?

# Clayton 6:
🔐 4. Sign the MSIX  
MSIX apps must be digitally signed:

Options:  
Use a certificate from a trusted CA (required for Store submission)

For this step, what is the typical cost? How long does it last?

# ChatGPT 6:
Great question — here's a breakdown of the **cost, validity, and options** for getting a **code signing certificate** from a **trusted Certificate Authority (CA)**, which is required for Microsoft Store MSIX submission:

---

## 🔐 Code Signing Certificate — Overview

|Provider|Type|Cost (USD)|Validity|Notes|
|---|---|---|---|---|
|**DigiCert**|OV|~$500–700/year|1–3 years|High-trust, widely accepted|
|**Sectigo (formerly Comodo)**|OV|~$70–120/year|1–3 years|Affordable and common|
|**GoDaddy**|OV|~$200–300/year|1–3 years|Easy for domain owners|
|**GlobalSign**|OV|~$350–450/year|1–2 years|Enterprise-grade|
|**SSL.com**|OV/EV|~$80–400/year|1–3 years|Offers EV for SmartScreen reputation|

> 💡 **OV = Organization Validated**, **EV = Extended Validation**  
> For the Microsoft Store, **OV is enough**. EV helps with SmartScreen warnings for sideloaded apps.

---

## ⏳ Certificate Validity

- Certificates typically last **1 to 3 years**
    
- Some CAs offer multi-year discounts
    
- You must **renew and re-validate** before expiration to keep signing
    

---

## ✅ Required for Microsoft Store?

Yes. Microsoft requires:

- A valid **OV certificate issued by a Trusted Root CA**
    
- You **cannot use a self-signed certificate**
    
- You **do not** need EV unless sideloading outside the store
    

---

## 📌 Note: SmartScreen Reputation

If you distribute your app outside the Microsoft Store (via direct download), SmartScreen may block it even if signed. EV certificates **bypass SmartScreen faster**, but store apps are **trusted automatically**.

---

Would you like recommendations for where to buy a low-cost OV certificate that works for MSIX submissions?

# Clayton 8:
What is the best way to provide a superuser shell within a gui application? A separate window can launch?

# Chat GPT 8:
To provide a **superuser shell** (i.e., an interactive terminal with elevated privileges) **within a GUI application**, while keeping it **separate but integrated**, the best approach depends on your platform (Windows, macOS, Linux) — but here’s a general **cross-platform strategy** that’s both user-friendly and secure:

---

## ✅ Best-Practice Design

**Launch a separate terminal window** from your GUI that:

1. Opens an elevated shell (`sudo`, `runas`, etc.)
    
2. Runs with its own privileges
    
3. Can return exit code or output to the GUI (optional)
    

---

## 🪟 On Windows

You can use `runas` or `powershell Start-Process`:

### ✅ Example (Python)

```python
import subprocess

def launch_admin_shell():
    subprocess.run([
        "powershell", "-Command",
        "Start-Process powershell -Verb runAs"
    ])
```

This opens a **new PowerShell window as Administrator**.

---

## 🐧 On Linux/macOS

You can launch `gnome-terminal`, `xterm`, or similar with `sudo -s` or `su`.

### ✅ Example (Python)

```python
import subprocess

def launch_root_shell():
    subprocess.run(["x-terminal-emulator", "-e", "sudo -s"])
```

> Replace `x-terminal-emulator` with `gnome-terminal`, `konsole`, or `xterm` as needed.

---

## 🔐 Security Tips

- **Don't bundle a hidden shell** in the main GUI — it's dangerous and confusing.
    
- **Do not store the user's sudo password** or escalate silently.
    
- **Explicitly prompt** the user and **log the request or reason** in your app.
    

---

## 💡 UX Tips

- Add a button like **"Open Admin Shell"** in a settings or advanced menu.
    
- Optionally pass environment variables or project-specific context.
    
- If using PyInstaller, mark the subprocess as detached to avoid freezing the GUI.
    

---

Would you like a working cross-platform Python function that tries to detect and launch the right terminal with elevated rights based on OS?

# Clayton 9:
Can a "Open Admin Shell" button launch a special instance of powershell or cmd or windows console host?
# ChatGPT 9:

Yes, an **"Open Admin Shell"** button in your GUI can absolutely launch a special instance of:

- **PowerShell**
    
- **CMD**
    
- or even a custom shell like **Windows Terminal** or **Console Host**
    

…**with admin privileges** and optional environment variables or working directory.

---

## ✅ Example: Launch PowerShell as Administrator from Python

```python
import subprocess

def open_admin_powershell():
    subprocess.run([
        "powershell", "-Command",
        "Start-Process powershell -Verb runAs"
    ])
```

This will:

- Prompt the user with **UAC**
    
- Launch a **new PowerShell window** running as **Administrator**
    
- Inherits working directory from your app (unless overridden)
    

---

## 🪟 Alternative: CMD as Admin

```python
def open_admin_cmd():
    subprocess.run([
        "powershell", "-Command",
        "Start-Process cmd -Verb runAs"
    ])
```

---

## 🧪 Customize Environment or Start Directory

You can launch with:

- Custom starting path:
    
    ```powershell
    Start-Process powershell -Verb runAs -WorkingDirectory "C:\MyApp"
    ```
    
- Preloaded variables or scripts:
    
    ```powershell
    Start-Process powershell -Verb runAs -ArgumentList "-NoExit", "-Command", "$env:MY_VAR='123'; cd 'C:\MyApp'"
    ```
    

In Python:

```python
def open_admin_shell_with_env():
    subprocess.run([
        "powershell", "-Command",
        "Start-Process powershell -Verb runAs -ArgumentList '-NoExit','-Command','cd C:\\MyApp; $env:MYVAR=42'"
    ])
```

---

## 🧩 Can I Launch Windows Terminal?

Yes — if the user has **Windows Terminal** installed:

```python
def open_admin_windows_terminal():
    subprocess.run([
        "powershell", "-Command",
        "Start-Process wt -Verb runAs"
    ])
```

You can also pass it profiles, commands, directories, etc.

---

Would you like to include a dropdown or settings menu in your GUI for selecting **which shell** to launch (PowerShell, CMD, etc.)?

# Clayton 10:
How is this different from "Don't bundle a hidden shell in the main GUI — it's dangerous and confusing." ?
# ChatGPT 10:
Great follow-up — here’s the clear distinction between **launching an admin shell from a button** and the **"dangerous hidden shell" anti-pattern**:

---

## ✅ Safe Approach (What You're Doing)

- You launch **a visible, standalone terminal** (PowerShell/CMD/WT) as admin when the user clicks a button like **"Open Admin Shell"**.
    
- The user:
    
    - Is prompted via UAC (Windows security)
        
    - Sees the terminal window appear
        
    - Has full visibility and control
        
- This is **expected**, **auditable**, and **user-initiated**.
    

🛡️ **Security posture**: High — user consents, system enforces access, no deception.

---

## 🚫 Dangerous Anti-Pattern

- The GUI **spawns a hidden or embedded shell** with elevated privileges **without the user clearly knowing**.
    
    - Often done with `subprocess.Popen(..., shell=True, creationflags=...)` and hidden windows.
        
- The shell:
    
    - May run background admin tasks invisibly
        
    - Might be hijacked or abused if input isn't sanitized
        
    - Can be a **privilege escalation risk**
        

⚠️ This is what’s meant by:

> “Don’t bundle a hidden shell in the main GUI — it’s dangerous and confusing.”

Especially bad: when the app runs with admin privileges **just to allow GUI access to system tasks**, and then pipes everything through that process.

---

## 👁️ Summary

|Method|User-Visible|Requires UAC|Scope Controlled|Security Risk|
|---|---|---|---|---|
|Button opens PowerShell (Admin)|✅ Yes|✅ Yes|✅ High|🔐 Low|
|Hidden elevated shell in background|❌ No|❌ Possibly|❌ Unclear|⚠️ High|

---

Would you like help creating a self-contained function that launches a chosen shell with elevated rights, optionally passing some context?