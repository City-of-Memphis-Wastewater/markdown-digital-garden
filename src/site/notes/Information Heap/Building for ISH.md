---
{"dg-publish":true,"permalink":"/information-heap/building-for-ish/","noteIcon":"","created":"2025-10-06T04:18:24.031-05:00"}
---

Date: [[-Daily Activity Log-/2025 10-October 06\|2025 10-October 06]]


TLDR
```
apk add --no-cache py3-numpy
apk add py3-cryptography

```
---

### **1️⃣ Architecture compatibility**

- iSH on iPhone emulates **x86_64 Linux** using **user-mode x86 emulation**, regardless of the ARM hardware of your iPhone.
- PRoot Alpine on Termux is running **ARM Linux** (because Termux runs natively on ARM).
    

**Implication:**

- A binary built on ARM (Termux) will **not run on iSH** because iSH expects x86_64 ELF.
- You need to **build for x86_64** if the target is iSH.

---

### **2️⃣ Using PRoot Alpine on Termux**

- PRoot lets you run a “full Alpine chroot” on ARM Termux.    
- Inside that environment, Python, pip, PyInstaller, etc., will all be **ARM binaries**.
- So PyInstaller will produce an **ARM ELF**, not x86_64.

---

### **3️⃣ Options to produce an iSH-compatible binary**

1. **Cross-compile with PyInstaller**:
    
    - PyInstaller **does not natively support cross-compilation** from ARM → x86_64.        
    - You’d need an x86_64 chroot or container environment to build the binary.
        
2. **Build directly in iSH**:
    
    - Install Alpine packages inside iSH (`apk add python3 py3-pip py3-setuptools py3-wheel py3-cryptography py3-cffi py3-musl-dev binutils`)
    - Set up a venv in iSH.
    - Install your requirements and PyInstaller in that environment.
    - Run `build_executible.py` → the output will be **x86_64 ELF**, compatible with iSH.
        

---

### **4️⃣ TL;DR**

- **ARM Termux → ARM ELF** → ❌ won’t run on iSH.
- **iSH → x86_64 ELF** → ✅ will run natively in iSH.
- If the  goal is a single-file executable that runs on iSH, you need to **build in an x86_64 environment** (iSH itself or an x86_64 Alpine chroot/VM).
    

Once copied to the right folder on an ISH installation, is there a way to launch the executible through iOS shortcuts, without running from the terminal directly?
---
Yes — you **can** trigger a command-line executable in iSH via iOS Shortcuts, but it’s a bit indirect because iOS can’t natively run Linux binaries — it relies on iSH as the execution environment. Here’s how it works:

---

### **1. Make sure your executable is ready**

- Copy the binary into iSH’s filesystem (e.g., `~/pipeline`).
    
- Ensure it’s executable:
    

```sh
chmod +x ~/pipeline/pipeline-eds-0.3.10-py310-alpine3.17.0-x86_64
```

- Test in iSH:
    

```sh
~/pipeline/pipeline-eds-0.3.10-py310-alpine3.17.0-x86_64 --help
```

---

### **2. Create a small shell script as a wrapper**

iOS Shortcuts can call scripts via URL schemes. It’s easier to wrap your executable in a script:

```sh
#!/bin/sh
# ~/pipeline/run_pipeline.sh
~/pipeline/pipeline-eds-0.3.10-py310-alpine3.17.0-x86_64 "$@"
```

Make it executable:

```sh
chmod +x ~/pipeline/run_pipeline.sh
```

---
### **3. Use iSH’s URL scheme in Shortcuts**

- iSH supports opening a terminal and running a command via **x-callback-url**:
```text
ish://term?command=/root/pipeline/run_pipeline.sh%20arg1%20arg2
```

- In Shortcuts, you can use **“Open URLs”** with this URL.
- `%20` is a space — so you can pass arguments if needed.
---

### **4. Optional: Auto-launch iSH in background**
- iSH itself doesn’t run headless — opening a shortcut launches the iSH app first.
- Once iSH is open, the script runs automatically.
---

### **Summary**
- You can trigger your ELF through iOS Shortcuts.
- You **cannot** run it completely headless — iSH must start.
- Wrapping the binary in a small shell script makes passing arguments easier.    
- CLI outputs appear in the iSH terminal window.
    

---

If you want, I can write a **ready-to-use shortcut URL** that launches your `pipeline-eds` binary with a preset command, so you can run it from your iOS home screen with one tap.

ish://term?command=/root/pipeline/run_pipeline.sh%20trend%20--help ; This goes into the safari browser bar?
---

Yep

Cheat Sheet
---

Here’s a concise cheat sheet for iSH URL encoding when launching commands via `ish://term?command=...`. Anything that could be interpreted by the URL parser or the shell needs attention:

|Character|URL-safe?|URL Encoding|Notes|
|---|---|---|---|
|Space|❌|`%20`|Must always encode spaces|
|Hyphen `-`|✅|—|Safe, used in options like `--help`|
|Underscore `_`|✅|—|Safe|
|Period `.`|✅|—|Safe, e.g., filenames|
|Slash `/`|✅|—|Path separator|
|Backslash `\`|❌|`%5C`|Rarely needed, mostly Windows paths|
|Question mark `?`|❌|`%3F`|Otherwise interpreted as URL query start|
|Ampersand `&`|❌|`%26`|Otherwise treated as URL param separator|
|Equal `=`|❌|`%3D`|Needed if part of value assignment in URL|
|Hash `#`|❌|`%23`|Otherwise treated as URL fragment|
|Plus `+`|❌|`%2B`|Otherwise treated as space in some contexts|
|Colon `:`|✅|—|Generally safe, e.g., `/usr/bin`|
|Semicolon `;`|❌|`%3B`|Could be interpreted as shell command separator|
|Quotes `'` / `"`|❌|`%27` / `%22`|Must encode if part of argument|
|Dollar `$`|❌|`%24`|Avoid unless you want shell variable expansion|
|Parentheses `()`|❌|`%28` / `%29`|Encode if literal argument|
|Comma `,`|✅|—|Safe, used in arguments like lists|

**Rule of thumb:**

- Encode **spaces** and any characters that could be interpreted by either **URL parser or shell**.
    
- Most letters, numbers, `_`, `-`, `.`, `/` are safe as-is.
    

If you want, I can draft a **ready-to-use iOS Shortcut template** that lets you enter arguments for iSH commands and automatically encodes everything properly. This avoids manual `%20` and other encodings.

