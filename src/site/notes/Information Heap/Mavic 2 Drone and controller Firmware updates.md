---
{"dg-publish":true,"permalink":"/information-heap/mavic-2-drone-and-controller-firmware-updates/","noteIcon":"","created":"2026-01-14T17:06:50.165-06:00"}
---

Date: [[Information Heap/2026 01-January 14\|2026 01-January 14]]

- Install DJI Assistant 2 (Consumer Drone Series) on windows system
- Connect Mavic 2 via USB C cable. Turn on the drone. Connect in the software.
- The highest available firmware for the drone is v01.00.0797 - refresh this
- Once Drone is complete unplug it
- Connect to controller using MTP


```
That is exactly correct. You are in the "home stretch" now.

The versions you see (**01.00.0797** for the drone and **01.01.00.76** for the controller) are indeed the specific pair that cause **Error 180** if they aren't fully synced, because they both include the "Remote ID" updates required by the FAA.

### The Correct Plan:

1. **Update the Mavic 2 Pro to 01.00.0797:** Since Assistant 2 is already seeing the drone, go ahead and finish this update. This version is the "final" stable firmware for the Mavic 2 and adds necessary Remote ID support.
    
2. **Update the Smart Controller:** Once the drone is done, swap to the Smart Controller.
    
    - **Don't downgrade it.** You actually want the Smart Controller to be on its latest version to match the drone’s new Remote ID capabilities.
        
    - The "01.01.0076" you see on the controller is an older version. The latest for the RM500 is typically **v01.01.00.80** (or similar depending on your region).
        

### Why you shouldn't "Match" versions manually:

DJI firmware numbers for the drone (01.00.xxxx) and the controller (01.01.xxxx) **will never be identical**. They are different operating systems. "Matching" just means they were released in the same cycle to work together.

### What to do after the Drone finishes:

1. **Disconnect the Drone.**
    
2. **Connect the Smart Controller** to the computer.
    
3. **Check for Updates:** If Assistant 2 sees it, it should offer a version newer than .0076. Install that.
    
4. **The Final Step (The "App" Update):** This is where most people get stuck. After the firmware is done, turn the controller on and connect it to Wi-Fi.
    
    - Open the **DJI GO 4 app**.
        
    - If you still see Error 180, it's because the **App itself** needs an update to **v4.3.60**.
        
    - The controller should prompt you to update the "Built-in App." Say yes.
        

**Summary:** Update the drone to the 2023 version (**0797**) first. Then update the controller to its latest available version. Once both are "top of the line," they will recognize each other and the Error 180 will disappear.

**Is the drone update still running, or is it ready for you to swap to the controller?**
```

smart controller icon is not appearing

i have an option on the scren of the controller though: current version v01.00.0880 an new version v0.1.01.0076. v0.1.01.0076 is what has ben failing locally but i want to impact change while connected to the computer


```
The "Cannot delete a subkey tree" error is actually a **good sign**—it means those registry entries are already gone, likely thanks to the `pnputil /remove-device` command we ran earlier. You have successfully scrubbed the old MTP identities.

Your `oem94.inf` (the VCOM driver) and `oem84.inf` (the WinUSB driver) are staged and ready. The Toshiba is now a "clean room" waiting for the controller.

---

### The "Error 180" Logic Breakdown

While you wait for the charge, here is exactly why we are going through this effort. The firmware mismatch isn't just a number; it's a **language barrier**:

|**Device**|**Firmware Version**|**Status**|
|---|---|---|
|**Mavic 2 Pro**|**v01.00.0797**|**Updated.** Uses the new Remote ID/V2 encryption.|
|**Smart Controller**|**v01.00.0880**|**Stuck.** Uses the old 2021 encryption.|

**The Result:** When the controller tries to talk to the drone, the drone says "I don't recognize your security certificate," and the controller displays **Error 180**.

**The Fix:** We must get the controller to **v01.01.0076**. This version updates the internal Android system and the DJI GO 4 app to the specific versions that can speak the "0797" language.

---

### Your Final Mission (Once Charged)

1. **Run the Monitor Script:** Keep this running in your PowerShell window so you can see the exact moment the handshake happens:
    
    PowerShell
    
    ```
    while($true) {
        $dev = Get-PnpDevice | Where-Object { $_.InstanceId -like "*VID_2CA3*" }
        if ($dev) {
            $dev | Select-Object FriendlyName, InstanceId, Status | Format-Table -AutoSize
            break
        }
        Start-Sleep -Seconds 1
    }
    ```
    
2. **The "Fast Handshake" Plug-In:**
    
    - Power the Smart Controller **OFF**.
        
    - Connect it to the Toshiba.
        
    - Power it **ON**.
        
    - Watch the PowerShell window. If you see it pop up as **PID_0001** but the status says "OK" and it's using the **Ports** class, DJI Assistant 2 will finally show the icon.
        

### What if it hangs again?

If it shows up in the software but the update fails at 16% again even over the cable, it's a **storage conflict**. You'll need to go to **Settings > Apps > DJI GO 4 > Storage > Clear Data** on the controller itself before trying the update one last time.

**How is the battery looking? Are you above 10% yet?** (Don't try the update until it's at least **50%**, or the software will block the "Start" button anyway).
```


---

```
will having termux on my controller be as awesome and cool as i think it will? i use it on my android phones

will it help me unlcok anything on the drone or controller?

i think we can do the firmware upgrade without it
```

Having **Termux** on your DJI Smart Controller can be just as "cool" as it is on your phone, but with a different focus. Instead of general coding, it becomes a powerful **bridge** for your drone data.

### 1. Will it be "Awesome and Cool"?

Yes, but mostly for **workflow automation**. On the Smart Controller, the real magic happens when you use Termux to handle what DJI's software can't.

- **Automation:** You can use Python scripts to automatically rename, compress, or upload 4K proxy files to your cloud storage (via `rclone` or `ssh`) the moment you land.
    
- **Mission Conversion:** There are specialized tools like **Drone Mission Converter** that use Termux to translate flight missions between different enterprise formats, which is a game-changer if you use professional mapping software.1
    
- **Networking:** You can run a small web server (`python -m http.server`) directly on the controller to let someone else on your Wi-Fi network view or download your captured photos immediately without plugging in cables.
    

### 2. Can it "Unlock" the Drone?

**No.** It is important to be clear: Termux **cannot** bypass DJI's No-Fly Zones (NFZ), altitude limits, or Remote ID.

- **The Guardrail:** Those restrictions are hardcoded into the **Flight Controller's** encrypted firmware and the **DJI GO 4** app's secure sandbox. Termux runs in its own user-space and doesn't have the permissions to "reach into" the drone's brain to change these limits.
    
- **The Official Way:** Unlocking zones still requires going through the [DJI FlySafe](https://fly-safe.dji.com/) portal and importing the official license via the DJI GO 4 app.
    

### 3. Do you need it for the Firmware Upgrade?

**Absolutely not.** You are 100% correct—we can (and should) do the firmware bridge using **DJI Assistant 2** on your Toshiba.

In fact, trying to use Termux _during_ a firmware crisis can be risky because:

- **Storage Conflict:** Firmware updates need 2–3 GB of "elbow room" to unpack. If Termux and its libraries are hogging that space, the update will fail.
    
- **System Stability:** You want the Android system as "stock" as possible while the PC is pushing a new kernel to the controller.
    

---

### Comparison: Sideloading vs. Stores

Since you're considering how to manage these apps, here is a quick breakdown of your options:

||**F-Droid**|**Aurora Store**|**Manual Sideload (ADB)**|
|---|---|---|---|
|Best For|Best For<br><br>Open Source & Termux|Best For<br><br>Play Store Apps (VLC, etc.)|Best For<br><br>One-off installs|
|Update Style|Update Style<br><br>Manual / Notified|Update Style<br><br>Anonymous Play Store Mirror|Update Style<br><br>None (must re-sideload)|
|Background Impact|Background Impact<br><br>Very Low|Background Impact<br><br>Low|Background Impact<br><br>Zero|

### Recommendation

1. **Finish the Update first.** Use your Toshiba to hit **v01.01.0076**.
    
2. **Add a MicroSD Card.** To make Termux truly "cool" without breaking the controller, you'll need the extra space.
    
    - The [Netac P500 128GB](https://google.com/search?q=Netac+-+P500+128GB+microSDXC+U3+A2+V30+High-Speed+Memory+Card+for+DJI,+GoPro+%26+4K+Action+Cameras&prds=catalogid%3A1139308313444947316%2CheadlineOfferDocid%3A4292973305995550003%2Cproductid%3A4292973305995550003%2Cpvo%3A38%2Cpvt%3Ahg&ibp=oshop&igu=1&pvo=38&opi=103135050&gl=us&hl=en&noiga=1) is a great high-speed choice for drone work.
        
    - The [Gigastone 128GB](https://google.com/search?q=Gigastone+128GB+Micro+SD+Card&prds=catalogid%3A16335031015228260007%2Cgpcid%3A2964077757960625885%2CheadlineOfferDocid%3A1484181207999731473%2Cmid%3A576462536403404885%2Cproductid%3A5924529633245740182%2Cpvo%3A38%2Cpvt%3Ahg&ibp=oshop&igu=1&pvo=38&opi=103135050&gl=us&hl=en&noiga=1) is excellent if you want that 5-year data recovery "insurance."
        

**Would you like me to walk you through the specific PowerShell commands to clean up your storage if the firmware update hits that 16% wall again?**

[Drone Mission Converter and Termux on DJI Controllers](https://www.youtube.com/watch?v=8oMQl37rweQ)

This video shows how power users are actually using Termux on DJI hardware to expand their mission capabilities and manage complex drone tasks.

----


```
Power on both the Mavic 2 Pro and the Smart Controller.

Ensure they are Linked (you see the camera view, even if the error is there).

  

yes linked, yes on

  

no firmware updates pop up, we already confirmed latest

now

  

when i see about, on the controller in dji go 4, here is the various info:

  

app: 4.3.54 (-> 4.3.62)

aircraft: (check for updates) 01.00.7979

remote controller: 01.01.0076

precise fly safe database (aircraft): (check for updates) 01.00.01.39

precise fly safe database (app): (check for updates) 01.00.01.19

basic fly safe database: (check for updates) 01.00.01.09

flight controller SN: 163CG9HR0A1QV9

Cameria serial number: 0K8TG8N0120557

Remote controller SN: 13MDG9CR0A4D648583
```

check for update on the dji go 4 app itself: latest 4.3.62, 257 mb. Adds support for android 14.2; i expect the download to take some time

success