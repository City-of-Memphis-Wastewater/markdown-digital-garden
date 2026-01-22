---
{"dg-publish":true,"permalink":"/information-heap/mavic-connection-for-photo-and-image-transfer/","noteIcon":"","created":"2026-01-15T13:30:08.697-06:00"}
---

Date: [[2026 01-January 15\|2026 01-January 15]]

```
$deviceName = "Smart Controller" # Change this to your drone's name
$shell = New-Object -ComObject Shell.Application
$thisPC = $shell.NameSpace(0x11) # 0x11 is the 'This PC' virtual folder
$drone = $thisPC.Items() | Where-Object { $_.Name -eq $deviceName }

if ($drone) {
    # This navigates into the drone's internal folders
    $storage = $drone.GetFolder.Items() | Where-Object { $_.Name -match "Storage" }
    $dcim = $storage.GetFolder.Items() | Where-Object { $_.Name -eq "DCIM" }
    
    # List all files in the media folder
    $media = $dcim.GetFolder.Items() | Get-Member
    Write-Host "Connected to $($drone.Name). Accessing DCIM..." -ForegroundColor Green
    $dcim.GetFolder.Items() | Select-Object Name, Size
} else {
    Write-Error "Drone not found. Check the name in File Explorer."
}
```
Based on community research and DJI's file structure, you are looking for specific extensions and three primary "hotspot" folders.

### 1. Expected File Types

- **Video:** `.mp4` (standard) and `.mov`. You may also see `.SRT` files (subtitle files containing GPS/telemetry data) and `.LRF` files (Low-Resolution Files used for the app's preview).
    
- **Images:** `.jpg` (standard) and `.dng` (RAW files).
    

### 2. The "Community Confirmed" Paths

Since you are in the **Internal shared storage**, DJI almost always buries the files in one of these three locations:

| **Content Type**       | **Likely Folder Path**                             |
| ---------------------- | -------------------------------------------------- |
| **High-Res Downloads** | `Android\data\dji.go.v5\files\DCIM\OriginalFiles\` |
| **Video Cache**        | `Android\data\dji.go.v5\files\MediaCaches\`        |
| **Screen Records**     | `Movies\` or `DCIM\DJI Album\`                     |
| **Screenshots**        | `Pictures\Screenshots\`                            |

---

### 3. The "Find All" PowerShell Script

Since we can't `cd`, let's use a recursive function to scan the **Internal shared storage** for those specific extensions. This will save you from clicking through every subfolder.

**Paste and run this block:**

PowerShell

```
# Define the file types you want
$extensions = '(\.mp4|\.mov|\.jpg|\.dng)

### Why it might be slow

MTP is significantly slower than a standard hard drive because the controller's processor has to "hand over" the file list to Windows one item at a time. If you have hundreds of files, the script above might take a minute or two to finish.

**Would you like me to update the script to automatically copy anything it finds into your current `dev` folder?**

[DJI RC Pro: Finding Your Files](https://www.youtube.com/watch?v=NLw71QhBN3U)

This video is specifically relevant because it provides a visual guide to the internal folder hierarchy of the DJI RC series, showing exactly where cache, original media, and screen recordings are stored.

---


```
function Sync-Drone2026 {
    $dest = Join-Path (Get-Location) "DroneMedia_2026"
    if (!(Test-Path $dest)) { New-Item -ItemType Directory -Path $dest | Out-Null }
    
    $shell = New-Object -ComObject Shell.Application
    $thisPC = $shell.NameSpace(0x11)
    $ctrl = $thisPC.Items() | Where-Object { $_.Name -eq "Smart Controller" }

    if (!$ctrl) { Write-Error "Smart Controller not found!"; return }

    # Storage targets to scan
    $storages = $ctrl.GetFolder.Items() | Where-Object { $_.Name -match "storage|SD card" }

    foreach ($drive in $storages) {
        Write-Host "Checking $($drive.Name)..." -ForegroundColor Cyan
        $destFolder = $shell.NameSpace($dest)
        
        # Recursive internal function to find and copy
        function Copy-Media($folder) {
            foreach ($item in $folder.Items()) {
                if ($item.IsFolder) { Copy-Media $item.GetFolder }
                elseif ($item.Name -match '^2026.*(\.mp4|\.mov|\.jpg|\.dng)

function Search-MTPStorage($folder) {
    foreach ($item in $folder.Items()) {
        if ($item.IsFolder) {
            # Recursive search through subfolders
            Search-MTPStorage $item.GetFolder
        } elseif ($item.Name -match $extensions) {
            # Report finding and its virtual path
            Write-Host "Found: $($item.Name)" -ForegroundColor Green
            Write-Host "Path:  $($item.Path)" -ForegroundColor Gray
        }
    }
}

Write-Host "Searching Internal shared storage... this may take a minute." -ForegroundColor Cyan
Search-MTPStorage $targetStorage.GetFolder
```

### Why it might be slow

MTP is significantly slower than a standard hard drive because the controller's processor has to "hand over" the file list to Windows one item at a time. If you have hundreds of files, the script above might take a minute or two to finish.

**Would you like me to update the script to automatically copy anything it finds into your current `dev` folder?**

[DJI RC Pro: Finding Your Files](https://www.youtube.com/watch?v=NLw71QhBN3U)

This video is specifically relevant because it provides a visual guide to the internal folder hierarchy of the DJI RC series, showing exactly where cache, original media, and screen recordings are stored.

---


{{CODE_BLOCK_2}}) {
                    if (!(Test-Path (Join-Path $dest $item.Name))) {
                        Write-Host "  + $($item.Name)" -ForegroundColor Green
                        $destFolder.CopyHere($item, 16)
                    }
                }
            }
        }
        Copy-Media $drive.GetFolder
    }
    Write-Host "Done! Files are in $dest" -ForegroundColor Yellow
}
```

function Search-MTPStorage($folder) {
    foreach ($item in $folder.Items()) {
        if ($item.IsFolder) {
            # Recursive search through subfolders
            Search-MTPStorage $item.GetFolder
        } elseif ($item.Name -match $extensions) {
            # Report finding and its virtual path
            Write-Host "Found: $($item.Name)" -ForegroundColor Green
            Write-Host "Path:  $($item.Path)" -ForegroundColor Gray
        }
    }
}

Write-Host "Searching Internal shared storage... this may take a minute." -ForegroundColor Cyan
Search-MTPStorage $targetStorage.GetFolder
```

### Why it might be slow

MTP is significantly slower than a standard hard drive because the controller's processor has to "hand over" the file list to Windows one item at a time. If you have hundreds of files, the script above might take a minute or two to finish.

**Would you like me to update the script to automatically copy anything it finds into your current `dev` folder?**

[DJI RC Pro: Finding Your Files](https://www.youtube.com/watch?v=NLw71QhBN3U)

This video is specifically relevant because it provides a visual guide to the internal folder hierarchy of the DJI RC series, showing exactly where cache, original media, and screen recordings are stored.

---


{{CODE_BLOCK_2}}