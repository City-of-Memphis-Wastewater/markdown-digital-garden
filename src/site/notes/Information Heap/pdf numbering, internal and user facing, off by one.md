---
{"dg-publish":true,"permalink":"/information-heap/pdf-numbering-internal-and-user-facing-off-by-one/","noteIcon":"","created":"2025-12-30T14:03:57.675-06:00"}
---

Date: [[-Daily Activity Log-/2025 12-December 30\|2025 12-December 30]]

```
so we are deciding that the analysis stuff , including analysis_pymupdf and the ffi rust bring and analysis_pypdf should all be zero indexed,

  

but the report and validation prints should be one indexed?

  

let's slow down

  

maybe we need a dedicated file and class for this, for extra clarity

  

considering library use cases, leveraging __init__, and gui and cli use cases

  

if we expose internals, it is important people understand the implecation and that the 0 index to 1 index is handled well and with the principa of least surpise
```

This is a critical architectural crossroad. You are correct to slow down. If the "internal" data in your JSON or dictionary exports is 0-indexed, but the "human" report is 1-indexed, a developer using your library for a GUI or a web dashboard will be constantly confused (the "Principle of Least Surprise" violation).

### The "Universal Truth" Strategy

In PDF development, the most robust way to handle this—and avoid "off-by-one" errors—is to follow the pattern used by professional libraries like `PyMuPDF`:

1. **Data Tier (0-indexed):** All internal dictionaries, JSON exports, and logic checks use 0-based indexing. This is "Physical Page Index."
    
2. **Display Tier (1-indexed):** All strings, logs, and CLI tables use 1-based indexing. This is "User Page Number."
    

### The "Dedicated Class" Proposal

To ensure this is handled with total clarity, we should move the conversion logic into a small, dedicated helper or a `Result` class. This prevents the "double-increment" bug you just found.

Let's look at how to clean up `report.py` and unify the logic:

---

### 1. The Unified Logic in `report.py`

In your `run_report` function, we need to ensure the **Display Logic** adds the `+ 1` only at the moment of string formatting.

Python

```
# --- Section 2: ACTIVE INTERNAL JUMPS ---
log("\n" + "=" * SEP_COUNT)
log(f"## Active Internal Jumps")
log("{:<5} | {:<5} | {:<40} | {}".format("Idx", "Page", "Anchor Text", "Jumps To Page"))

if all_internal:
    for i, link in enumerate(all_internal, 1):
        # We add +1 ONLY for the log/display
        src_page = link['page'] + 1 
        dest_page = link.get('destination_page')
        
        # Handle cases where dest_page might be None or "N/A"
        if isinstance(dest_page, int):
            dest_display = dest_page + 1
        else:
            dest_display = dest_page

        log("{:<5} | {:<5} | {:<40} | {}".format(
            i, src_page, link.get('link_text', 'N/A')[:40], dest_display
        ))
```

---

### 2. A Dedicated `PDFLinkResult` Helper

If you want to make this airtight for GUI/Library users, we can create a `pdflinkcheck/models.py` or add this to `helpers.py`. This class acts as a "Translator."

Python

```
# pdflinkcheck/models.py or helpers.py

class PageRef:
    """
    A simple translator to handle the 0-to-1 index conversion 
    without the 'Double Bump' risk.
    """
    def __init__(self, index: int):
        self.index = index  # The 0-based physical index

    @property
    def human(self) -> int:
        """The 1-based page number for humans."""
        return self.index + 1

    def __str__(self):
        return str(self.human)

    def __repr__(self):
        return f"PageRef(index={self.index}, human={self.human})"
```

---

### 3. Fixing `get_structural_toc` in `report.py`

Your current `get_structural_toc` is trying to calculate `max_page`. If the data coming in is 0-indexed, `max_page` will be one less than reality.

**Corrected Version:**

Python

```
def get_structural_toc(structural_toc: list) -> str:
    # ... (setup lines) ...

    # Determine max page width using 1-based numbers
    valid_pages = [item['target_page'] for item in structural_toc if isinstance(item['target_page'], int)]
    # Add +1 to the max physical index to find the widest user-facing number
    max_page_num = (max(valid_pages) + 1) if valid_pages else 1
    page_width = len(str(max_page_num))
    
    for item in structural_toc:
        indent = " " * 4 * (item['level'] - 1)
        target = item.get('target_page')
        
        # FORMATTING THE DISPLAY
        if isinstance(target, int):
            display_page = str(target + 1).rjust(page_width)
        else:
            display_page = str(target).rjust(page_width)
            
        lines.append(f"{indent}{item['title']} . . . page {display_page}")

    # ... (rest of logic) ...
```

---

### Summary of the "Single Source of Truth" Plan

|**Layer**|**Value Type**|**Reasoning**|
|---|---|---|
|**Analysis Engines** (`pypdf`, `rust`, `fitz`)|**0-indexed**|Matches the actual PDF array indices.|
|**JSON/Dict Data**|**0-indexed**|Developers can use this directly to jump to pages in code.|
|**Validation Logic**|**0-indexed**|`if target < 0` or `if target >= total_pages` is clean math.|
|**CLI Reports**|**1-indexed**|Normal humans don't count from Page 0.|
|**Exported Text Files**|**1-indexed**|Matches what a reader sees in their PDF viewer.|

### Updated Changelog

# Changelog

All notable changes to this project will be documented in this file.

The format is (read: strives to be) based on Keep a Changelog.

## [Unreleased]

### Added

- Standardized the "Display vs Data" boundary: internal indices remain 0-based while all reports are formatted to 1-based.
    

### Changed

- Refactored `report.py` display tables to apply `+ 1` offset at the point of string formatting.
    
- Updated `get_structural_toc` to correctly calculate alignment padding based on 1-indexed page numbers.
    

---

### Next Step

Should I draft a **`models.py`** file for you that defines a standard `LinkResult` dictionary structure, so that no matter which engine (`rust`, `pypdf`, `fitz`) is used, the data is guaranteed to be 0-indexed before it hits the report?


---

```
please make a table of files in this project in which we must be clear about the indexing rules

  

ffi.py, analyze_pymupdf.py, analyze_pypdf.py, __init__.py, validate.py, report.py

  

there may be o indexing inside of files for refernce which also include 1 indexing for printing, so file by file is not granular enough

  

indicated whichi files use only 0 indexing, which are mixed and why, and is any use 1-indexing only
```

To establish a "Single Source of Truth," we must draw a hard line between **Physical Indices** (0-based math for the machine) and **Logical Page Numbers** (1-based strings for the human).

The following table breaks down the indexing strategy across your project files to prevent "double-bump" bugs.

### Indexing Map: Physical (0) vs. Logical (1)

| **File**              | **Context**      | **Index Rule**      | **Reasoning**                                                                                          |
| --------------------- | ---------------- | ------------------- | ------------------------------------------------------------------------------------------------------ |
| `ffi.py` (Rust)       | Data Extraction  | **0-indexing only** | Rust's `pdf-extract` and `lopdf` crates are 0-indexed. Data should stay raw.                           |
| `analysis_pypdf.py`   | Data Extraction  | **0-indexing only** | `pypdf` is 0-indexed. Your previous `+ 1` hacks have been removed.                                     |
| `analysis_pymupdf.py` | Data Extraction  | **Mixed**           | **Internal:** 0-indexed. **TOC:** `get_toc()` is natively 1-indexed. Needs normalization.              |
| `validate.py`         | Logic/Validation | **Mixed**           | **Logic:** Uses `START_INDEX=0` for boundary checks. **Strings:** Formats error messages as 1-indexed. |
| `report.py`           | Output/Reporting | **Mixed**           | **Data:** Keeps dictionary values at 0. **Display:** Formats CLI tables as 1-indexed.                  |
| `helpers.py`          | Translation      | **Mixed**           | The `PageRef` class acts as the "Border Control" between 0 and 1.                                      |
| `__init__.py`         | API Surface      | **0-indexing only** | If exposing a library, users expect 0-indexed lists of pages/links.                                    |


The following table maps the specific variables and code sections where these two systems collide.

### Granular Indexing Map (Focus on "Mixed" Logic)
| **File**                           | **Variable / Context**  | **Index Type** | **Purpose / Action**                                                     |
| ---------------------------------- | ----------------------- | -------------- | ------------------------------------------------------------------------ |
| **`analysis_pymupdf.py`**          | `page_num` in loop      | **0**          | Standard `range(doc.page_count)` loop.                                   |
|                                    | `link.get('page')`      | **0**          | PyMuPDF internal link target is 0-indexed.                               |
|                                    | `doc.get_toc()`         | **1**          | **Native Exception:** PyMuPDF returns TOC pages as 1-based.              |
|                                    | `target_page` (Return)  | **0**          | **Normalization:** We must subtract 1 from TOC results before returning. |
| **`validate.py`**                  | `target_page`           | **0**          | The integer value being tested against `total_pages`.                    |
|                                    | `START_INDEX`           | **0**          | The constant used for the lower bound check.                             |
|                                    | `reason` string         | **1**          | **Translation:** f-string uses `{target_page + 1}` for the log.          |
|                                    | `issues["target_page"]` | **0**          | Stored in the issue dict as 0 for potential programmatic use.            |
| **`report.py`**                    | `all_internal` list     | **0**          | Dictionary values for `page` and `destination_page`.                     |
|                                    | `log()` table rows      | **1**          | **Translation:** Table columns are calculated as `{val + 1}`.            |
|                                    | `get_structural_toc`    | **1**          | **Translation:** Title alignment and page display use 1-based math.      |
| **`helpers.py`**                   | `self.index`            | **0**          | The underlying storage of the `PageRef` class.                           |
|                                    | `self.human`            | **1**          | The property that returns the incremented value.                         |
| **`ffi.py` / `analysis_pypdf.py`** | All variables           | **0**          | **Strict 0:** No `+1` offsets exist in these files at any level.         |

---

### Detailed File Analysis

#### 1. `analysis_pymupdf.py` (The "Trickiest" File)

PyMuPDF is inconsistent.

- **Action:** You must subtract 1 from `doc.get_toc()` results to store them as 0-indexed in your data, or maintain a clear comment that TOC is the exception.
    
- **Current Bug:** Your `link_dict['page']` currently adds 1. **Change this to 0-indexed** to match the Rust and pypdf engines.
    

#### 2. `validate.py` (The Validator)

This is where indices are checked for "reasonableness."

- **Rule:** Keep all math 0-based (`if target_page >= total_pages`).
    
- **Rule:** Only add `+ 1` inside f-strings for the `reason` field.
    

#### 3. `report.py` (The Presenter)

This file should treat the incoming data as "raw 0-indexed."

- **Rule:** Never modify the dictionary values.
    
- **Rule:** Use `PageRef(link['page']).human` or `link['page'] + 1` only when calling `log()` or `print()`.
    

---

### Recommended Normalized Schema

To ensure the Principle of Least Surprise, every extraction function (`extract_links_...`) should return this standardized dictionary structure:

Python

```
# The "Least Surprise" Internal Dictionary
{
    'page': 0,                   # Physical Index (Always 0-indexed)
    'destination_page': 10,      # Physical Index (Always 0-indexed)
    'link_text': "Click Here",
    'type': "Internal"
}
```


### Changed

- Standardized the core data pipeline to use **0-based Physical Indexing** across all engines (`pypdf`, `pymupdf`, `rust`).
    
- Moved 1-based "Human Page Numbering" exclusively to the reporting and validation string-formatting layers.
    
- Refactored `PageRef` helper to provide a safe translation interface between indices and user-facing numbers.
    

### Fixed

- Fixed an inconsistency in `analysis_pymupdf.py` where link locations were being reported as 1-indexed while other engines reported 0-indexed.
    

---

**Would you like me to rewrite the `analyze_toc_fitz` and `link_dict` logic in `analysis_pymupdf.py` to strip out the current `+1` offsets and normalize them to 0?**

----

```
does fitz give us 1-indexing? i thinit does for the target page but not for the source page
```

You are absolutely correct. PyMuPDF (fitz) is famously inconsistent here, which is why it has been the primary source of "off-by-one" errors in your project.

### The PyMuPDF Indexing Split

|**Feature**|**PyMuPDF Indexing**|**Description**|
|---|---|---|
|**Source Page**|**0-indexed**|When iterating with `range(doc.page_count)` or using `load_page(n)`, it is 0-based.|
|**Link Dictionary**|**0-indexed**|The `page` key inside `page.get_links()` refers to the physical index of the target.|
|**Table of Contents**|**1-indexed**|`doc.get_toc()` returns page numbers exactly as a human sees them in a viewer.|

---

### Why this is dangerous

If you treat the TOC output the same way you treat the Link output, your "GoTo" links will work perfectly, but your "Bookmarks" will always point to one page too far (or crash on the last page).

### Implementation with `PageRef`

To handle this without the brittle `page_num - 1` math, we use `PageRef` to act as the "Source of Truth" translation layer.

#### 1. Correcting the TOC (The 1-based outlier)

Since `fitz` gives us a human number (1, 2, 3...), we have to manually create the index for `PageRef`.

Python

```
# analysis_pymupdf.py

def analyze_toc_fitz(doc):
    toc = doc.get_toc()
    toc_data = []
    for level, title, page_num in toc:
        # We know fitz gives us a human number. 
        # We convert it to a physical index for our internal storage.
        ref = PageRef(page_num - 1) 
        
        toc_data.append({
            'level': level,
            'title': title,
            'target_page': ref.index  # Stored as 0
        })
    return toc_data
```

#### 2. Correcting the Links (The 0-based standard)

Since `fitz` gives us a physical index (0, 1, 2...), we pass it directly to `PageRef`.

Python

```
# analysis_pymupdf.py

# Inside the loop for page.get_links()
p_index = link.get('page') 
if p_index is not None:
    # fitz gives us 0, so we pass it straight to PageRef
    dest_ref = PageRef(int(p_index))
    
    link_dict.update({
        'destination_page': dest_ref.index, # Stored as 0
        'target': f"Page {dest_ref.human}"  # Displayed as 1
    })
```

### Changed

- Clarified PyMuPDF indexing behavior: Source/Link keys are 0-indexed, while TOC entries are 1-indexed.
    
- Standardized all `analysis_pymupdf.py` outputs to 0-indexed physical integers.
    
- Used `PageRef` to encapsulate the conversion logic, ensuring the `+1` offset only exists in the reporting/display strings.
    

---

**Would you like me to show you the equivalent logic for `analysis_pypdf.py`? (Hint: pypdf is much more consistent—it is 0-indexed everywhere.)**

```
can i rely on them to continue to be famously inconsistent?
```

In the world of PDF libraries, **yes—you should absolutely assume they will remain inconsistent.** The inconsistency isn't a "bug" that PyMuPDF (or others) will likely "fix." It stems from the fact that these libraries bridge two different worlds: the **PDF Specification** (which uses 0-based arrays for objects) and the **Human User Experience** (where the first page is always "1").

Here is why you should rely on the `PageRef` class rather than assuming the libraries will ever normalize:

### 1. The "Legacy Contract"

If PyMuPDF suddenly changed `get_toc()` to be 0-indexed, they would break thousands of production scripts worldwide. Most PDF libraries prioritize **backward compatibility** over "API cleanliness." If it’s 1-indexed today, it likely will be 1-indexed for the life of that major version.

### 2. Different Specifications for Different Features

- **Annotations/Links:** These are low-level PDF objects. The spec defines them by their index in the Page Tree. Naturally, the library gives you the 0-based index.
    
- **Bookmarks/TOC:** These are "Logical" structures meant for the user. Since the PDF "Page Label" might be "i", "ii", or "1", but the TOC needs to point to a physical spot, libraries often provide a "Human Page Number" as a convenience.
    

---

### The Strategy: "Trust, but Normalize"

Since you can't rely on the libraries to be consistent with _each other_, you must rely on your **Normalization Border** in the extraction scripts.

| **Engine**         | **Source of Truth** | **Your Action**                                               |
| ------------------ | ------------------- | ------------------------------------------------------------- |
| **Rust (pdfium)**  | Always 0            | Pass directly to `PageRef(index)`.                            |
| **pypdf**          | Always 0            | Pass directly to `PageRef(index)`.                            |
| **PyMuPDF (fitz)** | **Mixed**           | Use `PageRef(val)` for Links; use `PageRef(val - 1)` for TOC. |

By using `PageRef`, you make your code **future-proof**. If PyMuPDF ever _does_ change their API, you only have to change one line in your `analysis_pymupdf.py` extraction logic, and the rest of your `validate.py` and `report.py` logic remains untouched.

---

```
(deciding the mixing in analyze_pymupdf.py)

but are the target and the detination page as two dict entries both internal dev facing at this point? and should be 0 indexed
```

This is a crucial distinction. Let's clarify the "personality" of those two dictionary keys: **`destination_page`** and **`target`**.

### 1. `destination_page` (The "Internal" Coordinate)

This is strictly **internal/developer-facing**. It is an integer.

- **Indexing:** **0-indexed**.
    
- **Purpose:** This is what `validate.py` uses to check if the link is broken (e.g., `if link['destination_page'] >= total_pages`). It is also what a "Redact" or "OCR" script would use to know exactly which array index to look at.
    
- **Standard:** All engines (Rust, pypdf, PyMuPDF) **must** output this as 0-indexed.
    

### 2. `target` (The "Public" Label)

This is **user-facing**. It is a string.

- **Indexing:** **1-indexed** (within the text of the string).
    
- **Purpose:** This is a "pre-baked" label for reports. When the user opens the final `.json` or `.txt` report, they see `"target": "Page 5"`. They don't want to see `"target": 4`.
    
- **Standard:** This is generated using `PageRef.human` to ensure the "+1" only happens at the very last second for the sake of the human reader.
    

---

### The Final Alignment Table for `analysis_pymupdf.py`

Based on your table, here is the exact logic we are implementing to ensure **0-indexing** across the board for the return data:

|**Feature**|**Raw Library Value**|**Action Needed**|**Final Stored Index (destination_page)**|
|---|---|---|---|
|**Source Page**|`page_num` (0)|None|**0**|
|**Link Target**|`link.get('page')` (0)|None|**0**|
|**TOC Target**|`entry[2]` (**1**)|**Subtract 1**|**0**|

---

### Snippet: The "Normalization Border" in PyMuPDF

Here is how we ensure that by the time the data leaves `analysis_pymupdf.py`, everything is a "Universal Truth" 0-index:

Python

```
# --- Handling the TOC Exception ---
def analyze_toc_fitz(doc):
    toc_data = []
    for level, title, page_num in doc.get_toc():
        # page_num is 1 (Human). We normalize to 0 (Physical).
        ref = PageRef(page_num - 1) 
        toc_data.append({
            'level': level,
            'title': title,
            'target_page': ref.index # Now it is 0!
        })
    return toc_data

# --- Handling the Link Consistency ---
# Inside extract_links_pymupdf...
p_index = link.get('page')
if p_index is not None:
    # p_index is already 0 (Physical).
    dest_ref = PageRef(int(p_index))
    
    link_dict.update({
        'destination_page': dest_ref.index, # Stored as 0
        'target': f"Page {dest_ref.human}"  # Stored as "Page 1" for humans
    })
```

### Why this is the right goal:

If `validate.py` receives a list of links and some are 0-indexed and some are 1-indexed, it will mark valid links as "Out of Bounds" errors. By forcing the **extraction layer** to normalize everything to 0, `validate.py` can be simple, fast, and 100% certain.

---

### Updated Changelog

# Changelog

All notable changes to this project will be documented in this file.

The format is (read: strives to be) based on Keep a Changelog.

## [Unreleased]

### Changed

- Refined the data contract for `analysis_pymupdf.py`: `destination_page` (int) and `target_page` (int) are now guaranteed 0-indexed physical offsets.
    
- Consolidated human-readable page labeling into the `target` string key using `PageRef.human`.
    

---

**Would you like me to generate the updated `analysis_pymupdf.py` file one last time with these specific TOC and link normalization fixes applied?**