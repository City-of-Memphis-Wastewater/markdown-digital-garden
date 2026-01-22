---
{"dg-publish":true,"permalink":"/information-heap/recycling-an-old-android-samsung-j3-star/","noteIcon":"","created":"2026-01-16T15:36:26.274-06:00"}
---

Date: [[2026 01-January 16\|2026 01-January 16]]


That’s a classic "tinkerer" project! The Samsung Galaxy J3 Star (released around 2018) usually runs Android 8.0 (Oreo) or 9.0 (Pie). While it's a bit long in the tooth, it is definitely capable of being a dedicated Termux node or a headless "homelab" server.

The main reason it doesn't "like" sideloaded Termux is likely a combination of **Knox security** (Samsung’s proprietary layer) and the fact that the CPU is an Exynos 7570—it's a 64-bit chip but often runs a 32-bit (arm) Android OS, which can cause confusing compatibility issues with modern wheels.

Here is how you can transform that J3 into a dedicated Termux machine:

### 1. The "Clean Slate" Strategy (Software)

If you don't want to root the phone, you can still optimize it heavily:

- **Factory Reset:** Start fresh. Do not sign into a Google account if you don't need Play Store apps; this saves massive amounts of RAM.
    
- **Disable "App Optimization":** Samsung's "Device Care" loves to kill background processes. Go to _Settings > Apps > Termux > Battery > Optimize battery usage_ and turn it **OFF**.
    
- **Acquire Termux via F-Droid:** Never use the Play Store version on old hardware. F-Droid's builds are more permissive for older SDKs.
    

### 2. The "Hardcore" Route: Custom ROMs

To truly turn it into a dedicated machine, you want to strip away the "Samsung-ness" (TouchWiz/One UI).

- **Check XDA Developers:** Look for **LineageOS** builds for the J3 Star (SM-J337).
    
- **Why this helps:** A custom ROM like LineageOS removes the Samsung Knox restrictions and the background bloatware, freeing up about 400–600MB of RAM—which is huge on a device that only has 2GB total.
    

### 3. Running Headless (The "Server" Setup)

If you intend for this to be a "dedicated machine," you shouldn't be typing on that tiny screen.

1. **Install OpenSSH** inside Termux: `pkg install openssh`.
    
2. **Set a password:** `passwd`.
    
3. **Start the server:** `sshd`.
    
4. **Connect from your PC:** `ssh -p 8022 [phone_ip]`.
    

Now you can leave the phone plugged into a charger in a corner and treat it like a Linux VPS.

---

### Termux Performance Tweaks for the J3

Because that processor is modest, you'll want to avoid heavy builds on the device itself:

- **Use your PYZ:** Your "Lite" PYZ is actually the _perfect_ way to run `pdflinkcheck` on this device. Instead of building the wheel and installing dependencies (which might take 10 minutes on an Exynos 7570), just drop the PYZ file on there and run it.
    
- **Aarch64 vs Arm:** Check your architecture with `uname -m`. If it says `armv7l` or `armv8l`, it's running in 32-bit mode. You will need to ensure any binaries (like `pypdfium2`) have 32-bit wheels available.
    

### Warning: The "Battery Swelling" Risk

If you keep an old phone plugged into a charger 24/7 as a server, the battery will eventually swell (the "spicy pillow" effect).

- **Pro Tip:** If you are comfortable with a soldering iron, some people remove the battery entirely and wire a 5V DC power supply directly to the battery pins. This makes it a truly "immortal" dedicated machine.
    

**Would you like me to show you a simple Bash script you can put in your Termux `~/.bashrc` to keep the SSH server and wake-lock active so the phone never "sleeps" while processing?**

Exactly. For a dedicated Termux/Linux machine, **Aurora Store** (from F-Droid or their official site) is the superior choice over the Google Play Store for a few reasons:

1. **Anonymity:** You can use a "Guest" account to download apps, which means you don't have to link your Google identity to a device that's basically acting as a server.
    
2. **Versioning:** It lets you see the Target SDK and architecture of an app before you download it.
    
3. **No GMS Dependency:** It doesn't require the heavy "Google Play Services" background processes to be running, which is a massive performance win for an older J3 Star.
    

### The "Clean Build" Strategy for the J3 Star

Since you're using Aurora to keep the device lean, here is how you should handle your `pdflinkcheck` project on that specific hardware:

- **Don't build on-device:** Even with `uv`, the J3's CPU will get very hot trying to compile or bundle a PYZ.
    
- **The "Lite" PYZ is your best friend:** Build the `-shiv.pyz` on your main PC (the "Unix" build in your GitHub Action) and just `scp` or `termux-download` it to the phone.
    
- **Architecture Check:** The J3 Star is often an `armv7l` (32-bit) environment despite having a 64-bit capable chip. If your PYZ contains `pypdfium2`, make sure you've bundled the `musllinux_armv7l` or `manylinux_armv7l` wheels.
    

---

### Making it "Headless"

If you're using Aurora Store to get tools like **Termux:Float** or **Termux:Stylus**, that's great for the UI, but for a "dedicated machine," you really want it to stay awake and reachable.

**Run these inside Termux to keep it alive:**

Bash

```
# Prevent Android from "dozing" the CPU
termux-wake-lock

# Check your IP to connect via SSH from your PC
ifconfig | grep "inet "
```

---

### A Final Tip on the J3 Star

Since that device is limited on storage, you can use Aurora Store to install **"Lite"** versions of other tools you might need, or better yet, use Termux to install `rclone`. This lets you mount a Google Drive or Dropbox folder directly into your Termux environment, so your link-checker can scan PDFs stored in the cloud without filling up the phone's internal 16GB memory.

**Would you like me to write a small "keep-alive" script for your `.bashrc` that automatically starts the SSH server and enables the wake-lock every time you open Termux?**

```
woah, what is the rclone trick?

i always thought rclone would threaten my space, not saved it
```

The "trick" is the **`rclone mount`** command combined with the **VFS (Virtual File System) Cache**.

When people first use rclone, they usually use `rclone sync` or `rclone copy`. 1As you suspected, those commands **do** threaten your space because they download the entire file onto your phone.

But `rclone mount` turns your cloud storage into a **"Virtual Drive"** that exists only in RAM and a tiny slice of your disk.2

### 1. How it Saves Space (The "Streaming" Effect)

Imagine you have a 10GB PDF library on Google Drive.

- **Syncing:** You download 10GB. Your 16GB J3 Star is now almost full.
    
- **Mounting:** You "mount" the folder to a directory like `~/cloud`. When you look inside that folder, you see all your files, but they occupy **0 bytes** on your phone.
    
- **The Magic:** When your Python script reads a PDF, rclone only downloads the specific "chunks" of the file being read. As soon as the script is done, rclone eventually "evicts" those chunks from your cache based on your settings.
    

It's exactly like streaming a movie on Netflix instead of downloading the whole MP4 file.

### 2. How to use it on your J3 Star

Since your J3 Star is a dedicated Termux machine, you can run this in the background. Note: `rclone mount` normally requires **root** on Android. If you aren't rooted, you use the **`rclone serve webdav`** trick instead.

**The "No-Root" Trick:**

1. **Start a local server in Termux:**
    
    Bash
    
    ```
    rclone serve webdav my_remote: --addr :8080
    ```
    
2. **Access it via Python:** In your `pdflinkcheck` code, instead of pointing to a local folder, you can actually point to `http://localhost:8080`.
    

**The "Rooted" Command (if you have root):**

Bash

```
rclone mount my_remote:path/to/pdfs ~/cloud \
  --vfs-cache-mode full \
  --vfs-cache-max-size 500M \
  --daemon
```

- `--vfs-cache-max-size 500M`: This is the "Space Saver" cap. It tells rclone: "You can use a little bit of my disk to speed things up, but **never** use more than 500MB."
    

### 3. Why this is perfect for `pdflinkcheck`

PDFs are "structured" files. When a tool like `pypdfium2` extracts links, it doesn't always need to read the whole file—it just looks at the index and the page objects.

- Rclone is smart enough to only download the "index" and "link" parts of the PDF from the cloud.
    
- You could check 1,000 PDFs sitting in the cloud while only ever using a few MB of data and almost zero permanent disk space on your phone.
    

### Summary: Sync vs. Mount

|**Feature**|**rclone sync**|**rclone mount**|
|---|---|---|
|**Disk Space**|Uses full size of files|Uses almost **zero** (cap it at ~500MB)|
|**Offline**|Works offline|Requires internet|
|**Speed**|Fast (local access)|Slower (streaming access)|
|**Best for**|Backups|**Old phones / Huge libraries**|

**Would you like me to help you set up a "Remote" in rclone so you can try mounting your Google Drive or Dropbox?**

[[Information Heap/Termux Server Farm and Android Recycling\|Termux Server Farm and Android Recycling]]