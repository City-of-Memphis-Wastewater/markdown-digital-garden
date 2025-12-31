---
{"dg-publish":true,"permalink":"/information-heap/rust-so-placement/","noteIcon":"","created":"2025-12-29T20:12:58.474-06:00"}
---

Date: [[-Daily Activity Log-/2025 12-December 29\|2025 12-December 29]]

Since this is a critical runtime requirement for the Rust engine, you should document it in two places: one for **contributors** (who build from source) and one for **users** (who just install the package).

### 1. Where to Document

#### **In `README.md` (or `DEVELOPMENT.md`)**

Add a section specifically for the Rust engine requirements.

Markdown

````
### 🦀 Rust Engine Requirements
The Rust engine requires `libpdfium`. 

**For Development:**
If you are running the project from source, ensure the dynamic linker can find the shared objects:
```bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$(pwd)/rust_pdflinkcheck/target/release
````

For Production:

The libpdfium library must be present in src/pdflinkcheck/data/ or in your system's library path.

````

---

### 2. Where to include it in Build Scripts

Since you are likely using `uv` or `setuptools`, you have two main options to automate this so you don't have to run that `export` command manually every time.

#### **Option A: The `justfile` (Recommended for Dev workflow)**
If you don't have a `justfile` or `Makefile`, this is the perfect time to add one. It handles the environment variables for you.

```makefile
# justfile
analyze FILE:
    @echo "Running analysis on {{FILE}}..."
    LD_LIBRARY_PATH="{{invocation_directory()}}/rust_pdflinkcheck/target/release:$LD_LIBRARY_PATH" \
    uv run pdflinkcheck analyze {{FILE}} -p rust
````

#### **Option B: `pyproject.toml` (For Packaging)**

If you want the `.so` files to be included when you build the wheel (so `pip install` works), you need to tell your build backend to include the `data` directory.

If using **Hatch** (common with `uv` projects):

Ini, TOML

```
[tool.hatch.build.targets.wheel]
packages = ["src/pdflinkcheck"]
artifacts = ["src/pdflinkcheck/data/*.so", "src/pdflinkcheck/data/*.dll"]
```

#### **Option C: Internal Python Hack (The "Path Injector")**

You can actually modify the search path inside `src/pdflinkcheck/ffi.py` before loading the library. This is the "cleanest" for the user because they don't have to touch their shell.

Python

```
# In src/pdflinkcheck/ffi.py
import os
import ctypes

def _load_rust():
    path = _find_rust_lib()
    if not path:
        return None

    # On Linux, we can try to pre-load libpdfium or 
    # tell ctypes where to look if it's in the same folder
    if os.name == "posix":
        # This helps the Rust lib find its sibling libpdfium.so
        lib_dir = os.path.dirname(path)
        os.environ["LD_LIBRARY_PATH"] = f"{lib_dir}:{os.environ.get('LD_LIBRARY_PATH', '')}"

    try:
        return ctypes.CDLL(path)
    except OSError:
        return None
```

_> **Note:** Modifying `os.environ["LD_LIBRARY_PATH"]` inside Python doesn't always work after the process has started (the linker often reads it only at startup), so the **justfile** or **system install** is more reliable._

---

### Updated Changelog

# Changelog

All notable changes to this project will be documented in this file.

The format is (read: strives to be) based on Keep a Changelog.

## [Unreleased]

### Added

- Documented `LD_LIBRARY_PATH` requirements for Rust engine development in `README.md`.
    
- Added build-system instructions to include `src/pdflinkcheck/data/*.so` artifacts in distributed wheels.
    

---

**Would you like me to generate a complete `justfile` or `Makefile` that handles the Rust build, the library copying, and the Python execution in one command?**