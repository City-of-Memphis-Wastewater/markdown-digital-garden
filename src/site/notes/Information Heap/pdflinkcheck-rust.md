---
{"dg-publish":true,"permalink":"/information-heap/pdflinkcheck-rust/","noteIcon":"","created":"2026-01-01T15:29:51.869-06:00"}
---

Date: [[-Daily Activity Log-/2026 01-January 01\|2026 01-January 01]]

Since you are building a Rust-based Python package and want to distribute it on PyPI, the industry standard is to use **Maturin**.

While you are using `uv` for project management, **Maturin** is the "build backend" that understands how to compile Rust code into a Python wheel and handle the shared library linking (like your `libpdfium` requirement).

Here is how to restructure your new `pdflinkcheck-rust` directory to work as a standalone PyPI package.

### 1. The Directory Structure

Move your Rust files to the root of the new project so Maturin can find them easily.

Plaintext

```
pdflinkcheck-rust/
├── Cargo.toml            # Moved from rust_pdflinkcheck/
├── pyproject.toml        # The UV/Maturin config
├── README.md
├── src/                  # Your Rust source (from rust_pdflinkcheck/src)
│   └── lib.rs
├── pdflinkcheck_rust/    # Python wrapper (optional, for the __init__.py)
│   └── __init__.py
└── lib/                  # If you want to bundle libpdfium
    └── libpdfium.so
```

---

### 2. Configure `pyproject.toml`

Update your `pdflinkcheck-rust/pyproject.toml` to use `maturin`. This tells `uv` and `pip` how to build the project.

Ini, TOML

```
[project]
name = "pdflinkcheck-rust"
version = "0.1.0"
description = "Rust core for pdflinkcheck: fast PDF link & TOC extraction"
requires-python = ">=3.10"
authors = [{ name = "George Clayton Bennett", email = "george.bennett@memphistn.gov" }]
dependencies = []

[build-system]
requires = ["maturin>=1.5,<2.0"]
build-backend = "maturin"

[tool.maturin]
# This points to the Rust code
manifest-path = "Cargo.toml"
# This ensures the .so / .pyd file is placed inside a folder of this name
module-name = "pdflinkcheck_rust.rust_pdflinkcheck"
python-source = "."
```

---

### 3. Update `Cargo.toml`

Ensure your `Cargo.toml` includes `pyo3`. Since your existing `ffi.py` used `ctypes`, you have two choices:

1. **Stick with `ctypes`**: Keep your current Rust code and just package the `.so` file.
    
2. **Move to PyO3 (Recommended)**: This allows you to `import pdflinkcheck_rust` directly in Python without manual `ctypes` mapping.
    

If moving to **PyO3**, add this to `Cargo.toml`:

Ini, TOML

```
[dependencies]
# ... your other deps ...
pyo3 = { version = "0.22", features = ["extension-module"] }

[lib]
name = "rust_pdflinkcheck"
crate-type = ["cdylib"]
```

And in your `src/lib.rs`, wrap your function:

Rust

```
use pyo3::prelude::*;

#[pyfunction]
fn analyze_pdf(path: String) -> PyResult<String> {
    // Call your existing logic here
    let result = internal_analyze_logic(path); 
    Ok(result)
}

#[pymodule]
fn rust_pdflinkcheck(m: &Bound<'_, PyModule>) -> PyResult<()> {
    m.add_function(wrap_pyfunction!(analyze_pdf, m)?)?;
    Ok(())
}
```

---

### 4. Integration via "Extras"

To achieve your goal of `uv add pdflinkcheck --extra rust`, go back to your **primary** `pdflinkcheck` project (the one with the GUI/CLI) and modify its `pyproject.toml`:

Ini, TOML

```
[project.optional-dependencies]
rust = [
    "pdflinkcheck-rust>=0.1.0"
]
full = [
    "pdflinkcheck[rust]",
    "pymupdf>=1.26.7"
]
```

---

### 5. Managing `libpdfium`

Since you mentioned `libpdfium` is required at runtime, you have a few options for PyPI:

- **The "Heavy" way:** Include the `.so` inside the Python package and use `__init__.py` to add that directory to the `PATH` or `LD_LIBRARY_PATH` at runtime.
    
- **The "External" way:** Keep it as a prerequisite the user must install (current behavior in your README).
    

### Next Steps

1. **Install Maturin**: Run `uv pip install maturin`.
    
2. **Test Build**: Run `maturin develop` inside the `pdflinkcheck-rust` folder. This will compile the Rust code and install it into your current virtual environment.
    
3. **Verify**: Try `python -c "import pdflinkcheck_rust; print('Success')"` in your terminal.
    

Would you like me to help you rewrite the `ffi.py` logic to use the new native import instead of `ctypes`?

It is not a choice between the two; rather, **PyO3** and **Maturin** are partners that handle different parts of the process.

- **PyO3** is the **bridge** (the code). It provides the Rust traits and macros that allow you to write functions in Rust that Python can understand.
    
- **Maturin** is the **builder** (the tool). It handles the heavy lifting of compiling that Rust code into a Python-installable "wheel" and publishing it to PyPI.
    

Since you are restructuring for a standalone `pdflinkcheck-rust` package, you will use them together as follows:

### 1. The code level (PyO3)

In your `src/lib.rs`, you use **PyO3** to "export" your analysis logic so it can be imported like a normal Python module. This replaces the manual `ctypes` FFI you were using previously.

Rust

```
use pyo3::prelude::*;

// This macro makes the function callable from Python
#[pyfunction]
fn analyze_pdf(path: String) -> PyResult<String> {
    // Your existing analysis logic here...
    Ok("{}".to_string()) 
}

// This macro defines the actual Python module name
#[pymodule]
fn rust_pdflinkcheck(m: &Bound<'_, PyModule>) -> PyResult<()> {
    m.add_function(wrap_pyfunction!(analyze_pdf, m)?)?;
    Ok(())
}
```

### 2. The packaging level (Maturin)

In your `pyproject.toml`, you specify **Maturin** as the build backend. This tells tools like `pip` and `uv` to use Maturin when they need to compile your package from source.

Ini, TOML

```
[build-system]
requires = ["maturin>=1.5,<2.0"]
build-backend = "maturin"

[project]
name = "pdflinkcheck-rust"
# ... other metadata ...

[tool.maturin]
# Points to your Cargo.toml
manifest-path = "Cargo.toml"
# This feature is essential for PyO3 modules
features = ["pyo3/extension-module"]
```

### Why this is better than your current `ctypes` approach:

1. **Safety:** PyO3 handles the conversion between Python objects and Rust types safely, preventing the memory crashes common with raw pointers.
    
2. **Ease of Use:** Instead of a complex `ffi.py` trying to find `.so` files in different paths, you simply do `import rust_pdflinkcheck`.
    
3. **Standardization:** This is the same workflow used by major projects like **Polars** and **Pydantic**.
    

### Summary of Workflow with `uv`

1. **Initialize:** `uv add maturin` in your project.
    
2. **Develop:** Run `maturin develop --uv` to compile and install the Rust module into your current virtual environment for testing.
    
3. **Build:** Run `maturin build` to create the `.whl` files for distribution.
    

Advanced PyO3: Creating A Python CLI With RUST

This video demonstrates how to use PyO3 and Maturin together to build a high-performance Python CLI tool.