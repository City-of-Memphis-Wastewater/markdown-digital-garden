---
{"dg-publish":true,"permalink":"/information-heap/rust-core-packaging/","noteIcon":"","created":"2025-12-29T13:40:05.154-06:00"}
---

Date: [[-Daily Activity Log-/2025 12-December 29\|2025 12-December 29]]

Where should the `.so` files live?

Keeping them in `src/pdflinkcheck/` (next to `ffi.py`) is the most common "Wheels" approach, but it can get messy.

#### Option A: The `data/` directory (Better)

Since you already have a `src/pdflinkcheck/data/` folder, moving them there keeps your code directory clean. You would just update `ffi.py`:

Python

```
here = Path(__file__).resolve().parent / "data"
```

#### Option B: Separate "Core" Package (The "Pro" Way)

If you want to keep this project "coupled but clean," you can move the Rust code to its own repository or a separate folder and package it as `pdflinkcheck-core`.

- **Pros:** The Python code stays tiny. People who don't need the speed (or are on Termux) don't have to download 10MB of binaries.
    
- **Cons:** You have to manage versioning between two packages.
    

#### Option C: The Hybrid (Recommended)

Keep it in this repo, but use your build system to place the binaries in a specific folder that is marked as "Package Data."

---

### 3. Distribution Strategy: The "Online Repo" Idea

You mentioned referencing and building as needed. In the Rust/Python world, there is a specific tool for this: **Maturin**.

If you use **Maturin**, you can treat the Rust folder as a standard Python dependency. When a user runs `pip install`, it:

1. Checks if a pre-compiled "Wheel" (binary) exists for their OS.
    
2. If not, and they have Cargo installed, it compiles it on the fly.
    

This is how heavy-hitters like `polars` or `cryptography` work. It removes the need for you to manually `wget` and `cp` files into folders.
		
---

**Would you like me to show you a minimal `pyproject.toml` configuration that would let you use Maturin to manage this Rust core as a proper Python extension?** It would eliminate the need for your `ffi.py` "find the library" logic entirely.