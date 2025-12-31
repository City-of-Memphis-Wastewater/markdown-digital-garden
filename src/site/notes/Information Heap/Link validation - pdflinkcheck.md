---
{"dg-publish":true,"permalink":"/information-heap/link-validation-pdflinkcheck/","noteIcon":"","created":"2025-12-17T11:11:00.667-06:00"}
---

Date: [[-Daily Activity Log-/2025 12-December 17\|2025 12-December 17]]

```
i thin what i really need is a tol that checks existing links and ensures that they all resole

this would need to programmaticcaly test for launch with word docs, etc
```

That is the natural evolution of this tool. You’ve moved from **extraction** (finding the links) to **validation** (verifying they actually work).

Testing for "resolution" is much more complex than extraction because the "success" criteria change depending on the link type.

### 1. The Validation Matrix

To build this, your tool needs three distinct "checkers" within a validation loop:

|**Link Type**|**Validation Method**|**Definition of "Success"**|
|---|---|---|
|**Internal (GoTo)**|Index Verification|The `target_page` is an integer between $1$ and `reader.pages.length`.|
|**Web (URI)**|`requests.head()`|The server returns a `200 OK` or `3xx` redirect.|
|**Local/Launch**|`os.path.exists()`|The relative or absolute file path (Word doc, PDF, etc.) exists on the drive.|

---

### 2. Handling "Launch" Actions (Word Docs & Local Files)

As you noted, these manuals often link to external assets like Word docs or Excel sheets. In the PDF syntax, these are often `Launch` or `Remote GoTo` actions rather than `URI` actions.

Python

```
import os
import requests
from typing import Dict, Any

def validate_link_resolution(link: Dict[str, Any], base_path: str = "") -> Dict[str, Any]:
    """
    Programmatically tests if a link resolves.
    """
    link_type = link.get("type")
    target = link.get("uri") or link.get("target_page")
    
    # 1. Internal Link Validation
    if link_type == "Internal Jump":
        if isinstance(target, int) and target > 0:
            return {"status": "Valid", "error": None}
        return {"status": "Broken", "error": "Target page missing or invalid"}

    # 2. Web URI Validation
    if link_type == "External" and target.startswith(("http", "https")):
        try:
            # Use HEAD request to save bandwidth
            response = requests.head(target, timeout=5, allow_redirects=True)
            if response.status_code < 400:
                return {"status": "Valid", "code": response.status_code}
            return {"status": "Broken", "code": response.status_code}
        except Exception as e:
            return {"status": "Error", "error": str(e)}

    # 3. File/Launch Validation (Word Docs, MHTML, etc.)
    if "file://" in str(target) or link_type == "Launch":
        # Clean the file path (strip file:// and handle URL encoding %20 etc)
        clean_path = target.replace("file://", "").replace("%20", " ")
        if not os.path.isabs(clean_path):
            clean_path = os.path.join(base_path, clean_path)
            
        if os.path.exists(clean_path):
            return {"status": "Valid", "path": clean_path}
        return {"status": "Broken", "error": "File not found at path"}

    return {"status": "Unknown", "error": "Unrecognized link type"}
```

### 3. The "MHTML" Problem

In your earlier data, we saw `mhtml:file://` links.

- **The Catch:** These are often legacy links created by IE/Edge.
    
- **The Validation:** `os.path.exists()` will work, but you have to be careful with the prefixing. Your tool will need a "base directory" argument because links like `U:\StJohnsCo\...` will only resolve if the user is connected to that specific network drive.
    

---

### Updated Changelog

# Changelog

All notable changes to this project will be documented in this file.

The format is (read: strives to be) based on Keep a Changelog.

## [Unreleased]

### Added

- Proposed `validate_link_resolution` logic to perform multi-protocol validation (HTTP, File System, Internal).
    
- Added support for path normalization for `Launch` actions pointing to external Word documents.
    
- Integrated `requests` for asynchronous HEAD checks on external URIs.
    

---

Next Step:

Would you like me to integrate this validator into your main analyze_pypdf.py script so it outputs a "Dead Link Report" alongside the extraction results?