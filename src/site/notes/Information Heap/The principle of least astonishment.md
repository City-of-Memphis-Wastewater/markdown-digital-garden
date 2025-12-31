---
{"dg-publish":true,"permalink":"/information-heap/the-principle-of-least-astonishment/","noteIcon":"","created":"2025-12-29T18:15:33.006-06:00"}
---

Date: [[-Daily Activity Log-/2025 12-December 29\|2025 12-December 29]]

```

Is including extract_toc_*() into extract_links_*() a misnomer? 
Are they links?

```
Technically, it is a bit of a misnomer, but there is a strong "structural" argument for grouping them.

In the PDF specification, both **Hyperlinks** (on-page annotations) and **TOC entries** (bookmarks) are powered by the same underlying mechanism: **Actions** and **Destinations**.

### 1. Are they "Links"?

- **Technically:** They are "Outline Items" that point to "Destinations."
    
- **Functionally:** To a user, a bookmark is just a link that lives in a sidebar instead of on a page.
    
- **Programmatically:** They both answer the question: _"When I click this, where do I go?"_
    

### 2. The Case for Keeping Them Separate

If you merge them into a single function called `extract_links_*`, a developer might be surprised to find TOC data in the result. In CLI design, the **Principle of Least Astonishment** suggests that a function should do exactly what its name says.

### 3. The "Hybrid" Compromise (Recommended)

If you want to refactor for maintainability and performance without using a misnomer, use a "Data Object" approach:

1. **Keep the specific names** in `__init__.py` for the public API (`extract_links_pypdf`, `extract_toc_pypdf`).
    
2. **Use a private "All" function** in the backend modules to avoid redundant processing.
    

**Example of how you could refactor `analyze_pypdf.py`:**

Python

```
def _extract_everything_pypdf(pdf_path):
    # Open the PDF once
    # Extract TOC and Links in one pass
    return {"links": [...], "toc": [...]}

def extract_links_pypdf(pdf_path):
    return _extract_everything_pypdf(pdf_path)["links"]

def extract_toc_pypdf(pdf_path):
    return _extract_everything_pypdf(pdf_path)["toc"]
```

### 4. Summary Table

|**Feature**|**Hyperlinks (Links)**|**Bookmarks (TOC)**|
|---|---|---|
|**Location**|On the PDF page|In the Navigation Pane|
|**PDF Object**|`/Annot` (Link type)|`/Outlines`|
|**Destination**|`/Dest` or `/URI`|`/Dest` or `/Action`|
|**Visibility**|Requires "finding" on page|Always visible in sidebar|

---

### Updated Changelog

# Changelog

All notable changes to this project will be documented in this file.

The format is (read: strives to be) based on Keep a Changelog.

## [Unreleased]

### Changed

- Evaluated architectural merge of TOC and Link extraction; decided to maintain separate API endpoints to follow the Principle of Least Astonishment.
    
- Planned internal backend refactoring to use a single-pass extraction method for improved performance while preserving distinct public function names.
    

---

**Would you like me to show you how to implement that "single-pass" private function in your PyMuPDF or pypdf backends to speed things up?**

---


Since `report.py` already acts as your unified orchestrator, you’ve already solved the "user-facing" part of the problem. When a user runs the CLI or calls `run_report`, they get everything (links + TOC) in one shot.

The distinction I was making is purely for the **low-level API**. If a third-party developer uses your package as a library, they might only want the TOC without scanning thousands of page annotations. Keeping the functions separate in `__init__.py` respects that, even if `report.py` calls both of them behind the scenes.

### The "Clean" Final Polish for `__init__.py`

To make your `__init__.py` perfectly consistent with your new naming strategy and the "all-in-one" nature of the Rust engine, here is the final recommended structure for your lazy-loaded proxies:

Python

```
# lazy loaded library functions
def run_report(*args, **kwargs):
    from pdflinkcheck.report import run_report_and_call_exports as _run
    return _run(*args, **kwargs)

# --- pypdf ---
def extract_links_pypdf(*args, **kwargs):
    from pdflinkcheck.analyze_pypdf import extract_links_pypdf as _extract
    return _extract(*args, **kwargs)

def extract_toc_pypdf(*args, **kwargs):
    from pdflinkcheck.analyze_pypdf import extract_toc_pypdf as _extract
    return _extract(*args, **kwargs)

# --- PyMuPDF ---
def extract_links_pymupdf(*args, **kwargs):
    from pdflinkcheck.analyze_pymupdf import extract_links_pymupdf as _extract
    return _extract(*args, **kwargs)

def extract_toc_pymupdf(*args, **kwargs):
    from pdflinkcheck.analyze_pymupdf import extract_toc_pymupdf as _extract
    return _extract(*args, **kwargs)

# --- Rust (Consistency achieved) ---
def extract_links_rust(*args, **kwargs):
    from pdflinkcheck.ffi import extract_links_rust as _extract
    return _extract(*args, **kwargs)

def extract_toc_rust(*args, **kwargs):
    # Even if this just calls the same underlying Rust engine, 
    # it keeps the API predictable for people switching backends.
    from pdflinkcheck.ffi import extract_toc_rust as _extract
    return _extract(*args, **kwargs)
```

### Why this works so well for you:

1. **Logical Consistency:** If a user knows `extract_toc_pypdf` exists, they will naturally try to call `extract_toc_rust`. Providing that function (even as a wrapper) prevents "Method Not Found" frustration.
    
2. **Performance:** Since these are proxies, there is **zero overhead** for the MSIX launch.
    
3. **The `report.py` Logic:** Your `run_report` remains the "boss" that knows which of these to pull together for the final UI/JSON output.
    

---

### Updated Changelog

# Changelog

All notable changes to this project will be documented in this file.

The format is (read: strives to be) based on Keep a Changelog.

## [Unreleased]

### Added

- Standardized `extract_toc_rust` and `extract_links_rust` lazy proxies in `__init__.py` to provide a consistent API across all backends.
    

### Changed

- Refined `report.py` to act as the primary orchestration layer while keeping individual backend functions accessible for library use.
    
- Verified that top-level imports are fully deferred, ensuring maximum launch speed for MSIX-packaged environments.
    

---

**Does this look like a solid "stopping point" for the API refactor, or should we look at how the `ffi.py` returns that data to make sure the keys match exactly what `report.py` expects?**

---

