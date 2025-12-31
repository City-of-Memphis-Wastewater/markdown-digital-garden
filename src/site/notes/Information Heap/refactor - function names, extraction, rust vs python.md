---
{"dg-publish":true,"permalink":"/information-heap/refactor-function-names-extraction-rust-vs-python/","noteIcon":"","created":"2025-12-29T15:20:38.595-06:00"}
---

Date: [[-Daily Activity Log-/2025 12-December 29\|2025 12-December 29]]

```
it looks like the ffi stuff is formatted a little differently than the two python engines, with the two main functions

(paste cat src/pdflinkcheck/__init__.py)

i think i can actually refactor the pypdf code and the pymupdf code so that extract_toc* is included in extrac_links, because toc can be considered a link

  

and the accomplishing consistency between python and rust is a bit more maintainable?

  

or maybe this is a non issue and i can change forward, i just need to expose more specific naming in ffi.py, to follow the current pattern

  

def extract_links_rust(*args, **kwargs):

    import pdflinkcheck.ffi

    return pdflinkcheck.ffi.analyze_pdf_rust(*args, **kwargs)

  

analyze versys extract is inconsistent

```
You are spot on. Inconsistency in a public API is like a "splinter" for developers—it’s small, but you feel it every time you use it. Standardizing the naming now will save you from a "breaking change" headache later.

### 1. Fixing the Naming Inconsistency

If your Python backends use `extract_links_*`, your Rust backend should too. It makes it much easier to write generic code that swaps engines.

**In `src/pdflinkcheck/ffi.py`**, I would recommend aliasing your main function or renaming it to match the pattern:

Python

```
# ffi.py
def extract_links_rust(pdf_path: str):
    # This calls your Rust core and returns the unified data
    return analyze_pdf_rust(pdf_path)
```

**Then in `__init__.py`**, it looks much cleaner:

Python

```
def extract_links_rust(*args, **kwargs):
    from pdflinkcheck.ffi import extract_links_rust as _run
    return _run(*args, **kwargs)
```

---

### 2. Should you merge TOC into Links?

Merging them into a single call (like your Rust backend does) is actually **very efficient** for certain engines.

- **Rust:** Already does this in one pass.
    
- **PyMuPDF:** Loading the document once to get both is faster than opening/closing it twice.
    
- **pypdf:** Similar benefits; you iterate the objects once.
    

However, keep the **return structure** distinct. Even if one function returns both, I'd suggest a dictionary structure like:

Python

```
{
    "links": [...],
    "toc": [...]
}
```

### 3. Recommendation: The "Bridge" Pattern

If you want maximum maintainability, you can create a unified "Extraction" interface. Instead of having 6-10 different functions in `__init__.py`, you could eventually move toward:

1. `extract_all_pypdf()`
    
2. `extract_all_pymupdf()`
    
3. `extract_all_rust()`
    

Then, your specific `extract_links_*` and `extract_toc_*` functions just become thin wrappers that call the `extract_all` version and return just the piece requested.

---

### Changed

- Standardized API naming conventions across Python and Rust backends (changing `analyze_pdf_rust` references to `extract_links_rust`).
    
- Refactored `__init__.py` to use consistent lazy-loading patterns for all engine types.
    

---

**Would you like me to rewrite the `extract_links_rust` function in your `ffi.py` right now to ensure it perfectly matches the signature and return type of your Python engines?**