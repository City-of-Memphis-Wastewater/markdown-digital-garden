---
{"dg-publish":true,"permalink":"/information-heap/termux-file-launching/","noteIcon":"","created":"2025-10-01T14:35:46.890-05:00"}
---

Date: [[-Daily Activity Log-/2025 10-October 01\|2025 10-October 01]]


---

# Termux Web Content Serving and Launching Reference

This report outlines the primary strategies for serving and launching web-based content (like Plotly visualizations) from a Python script running in **Termux** (Android).

The core challenge in Termux is reliably opening the Android default browser to view locally generated files or content served by a temporary server.

## 1. Local HTTP Server Approach (Most Robust)

This method involves running a simple Python HTTP server to serve the content. This is the **most robust** solution for Plotly and other JavaScript-heavy visualizations, as it bypasses the strict browser security restrictions applied to local `file://` URIs.

|Component|Description|
|---|---|
|**Server**|Python's built-in `http.server.SimpleHTTPRequestHandler` (run in a separate `threading.Thread`).|
|**Content**|Plotly HTML file must be generated with `include_plotlyjs='full'`.|
|**Target**|A standard local URL: `http://localhost:8000/plot.html`|
|**Launch Method**|`xdg-open`|

### Code Reference (Launch Only)

This is the preferred, unified launch logic for Termux:

Python

```
import subprocess

# Assuming 'tmp_url' is defined as: 'http://localhost:8000/temp_plot.html'
tmp_url = 'http://localhost:8000/temp_plot.html'

try:
    # xdg-open is the preferred cross-desktop utility on Termux/Linux
    subprocess.run(['xdg-open', tmp_url], check=True)
    print(f"Successfully opened browser to: {tmp_url}")

except subprocess.CalledProcessError:
    # Fallback if xdg-open is missing or fails
    print("xdg-open failed. Attempting termux-open...")
    # Note: termux-open is for local files, but sometimes works on simple URLs
    subprocess.run(['termux-open', tmp_url], check=True)
    
except FileNotFoundError:
    print("xdg-open not found. Ensure 'xdg-utils' is installed in Termux.")
```

## 2. Direct File Launch Approaches (Less Reliable for Plotly)

These methods attempt to launch the browser directly with the local file path. They often lead to the "white screen" issue due to browser security restrictions on embedded JavaScript.

|Method|Utility|Reliability|
|---|---|---|
|**`xdg-open` (File)**|Standard Linux/Termux utility.|Low (Blocked by browser security on `file://` URI).|
|**`termux-open`**|Termux-specific utility for file/URL opening.|Medium (More robust, but often still subject to `file://` security).|
|**`am start`**|Android `ActivityManager` command line utility.|Complex (Requires exact package names; not generally portable).|

### Code Reference (File-Based Launch)

These are less recommended for Plotly, but useful for static content (like simple images or raw text files).

#### A. Using `xdg-open` (File)

Python

```
import subprocess
from pathlib import Path

local_html_path = Path("/data/data/com.termux/files/usr/tmp/plot.html") 

try:
    # xdg-open converts the path to a file:// URI internally
    subprocess.run(['xdg-open', str(local_html_path)], check=True)
    print(f"Successfully opened file: {local_html_path}")
    
except subprocess.CalledProcessError as e:
    print(f"xdg-open failed: {e}")
```

#### B. Using `termux-open`

Python

```
import subprocess

local_html_path = "/data/data/com.termux/files/usr/tmp/plot.html"

try:
    # termux-open is designed to interface with Android Intents
    subprocess.run(['termux-open', local_html_path], check=True)
    print(f"Successfully opened file using termux-open: {local_html_path}")
    
except FileNotFoundError:
    print("The 'termux-open' command was not found. Is the termux-api package installed?")
```

#### C. Using `am start` (Advanced/Non-Portable)

This is highly specific and should be avoided unless necessary, as it requires knowledge of the target browser's package and activity names.

Python

```
import subprocess

local_file_path = "/data/data/com.termux/files/usr/tmp/plot.html"
file_uri = f'file://{local_file_path}'

browser_package = 'com.android.chrome'
browser_activity = 'com.google.android.apps.chrome.Main'

command = [
    'am',
    'start',
    '-a', 'android.intent.action.VIEW',
    '-d', file_uri,
    '-n', f'{browser_package}/{browser_activity}'
]

try:
    # Requires permissions to use the 'am' command
    subprocess.run(command, check=True)
    print(f"Successfully launched Chrome via am start to view {file_uri}.")
except Exception as e:
    print(f"am start failed (requires specific permissions/setup): {e}")
```

## Summary of Best Practice

For maximum reliability and to prevent the "white screen" problem with interactive content like Plotly:

**Always use the Local HTTP Server approach, and launch the resulting URL with `xdg-open` in Termux.**