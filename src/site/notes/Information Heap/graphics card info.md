---
{"dg-publish":true,"permalink":"/information-heap/graphics-card-info/","noteIcon":"","created":"2025-11-25T10:47:33.454-06:00"}
---

Date: [[-Daily Activity Log-/2025 11-November 25\|2025 11-November 25]]

```
 oolong@CIT-36NZRL3  ~/dev  sudo lshw -C display
  *-display:0
       description: 3D controller
       product: Basic Render Driver
       vendor: Microsoft Corporation
       physical id: 3
       bus info: pci@933d:00:00.0
       version: 00
       width: 32 bits
       clock: 33MHz
       capabilities: bus_master cap_list
       configuration: driver=dxgkrnl latency=0
       resources: irq:0
  *-display:1
       description: 3D controller
       product: Basic Render Driver
       vendor: Microsoft Corporation
       physical id: 5
       bus info: pci@f135:00:00.0
       version: 00
       width: 32 bits
       clock: 33MHz
       capabilities: bus_master cap_list
       configuration: driver=dxgkrnl latency=0
       resources: irq:0
       
---


PS C:\Users\george.bennett\dev> powershell.exe "Get-CimInstance win32_VideoController | Select-Object Name"

Name
----
NVIDIA T550 Laptop GPU
Intel(R) Iris(R) Xe Graphics

```

That output confirms two very important things about your machine and your WSL setup:

## 1. Hardware Confirmation (Windows Host)

The PowerShell command confirms you have a **Hybrid GPU setup**:

|**Device Name**|**Type**|**Note**|
|---|---|---|
|**NVIDIA T550 Laptop GPU**|**Dedicated GPU (dGPU)**|This is a professional-grade discrete graphics card.|
|**Intel(R) Iris(R) Xe Graphics**|**Integrated GPU (iGPU)**|This is the graphics unit built into your Intel processor.|

Your machine **does** have a powerful dedicated GPU, but it also relies on the integrated Intel GPU. The Edge errors were happening because the browser was trying to use these hardware graphics processors, but the connection through WSLg was failing, especially since you have a more complex dual-GPU setup.

## 2. WSL GPU Configuration (Linux Guest)

The `sudo lshw -C display` output shows why the Edge errors occurred:

- **`description: 3D controller`**
    
- **`product: Basic Render Driver`**
    
- **`vendor: Microsoft Corporation`**
    
- **`configuration: driver=dxgkrnl`**
    

This indicates that your WSL environment is primarily seeing the **virtualized graphics components provided by Microsoft's WSLg infrastructure (`dxgkrnl` is the DirectX Graphics Kernel driver)**, not the raw NVIDIA or Intel hardware directly, which can lead to conflicts and failures when an application like Edge tries to initialize its own GPU process.

Your solution—**disabling GPU acceleration in Edge**—is the perfect workaround for this environment instability.

## 3. Command Error

The `lspci` command failed because the utility isn't installed on your WSL distribution by default, and you encountered a common package lookup error:

- **`zsh: command not found: lspci`**
    
- **`E: Unable to locate package lspci`**
    

The `lspci` utility is part of the `pciutils` package.

### 🛠️ Next Step: Installing the Hardware Utility

To install `lspci` and other PCI utilities, use this command:

Bash

```
sudo apt update && sudo apt install pciutils
```

- `sudo apt update` refreshes the list of available packages.
    
- `sudo apt install pciutils` installs the package that contains `lspci`.
    

Would you like to install the `pciutils` package now to complete your hardware check within the WSL environment?