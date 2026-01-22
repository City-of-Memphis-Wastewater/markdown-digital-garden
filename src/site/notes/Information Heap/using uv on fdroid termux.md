---
{"dg-publish":true,"permalink":"/information-heap/using-uv-on-fdroid-termux/","noteIcon":"","created":"2026-01-16T15:26:50.373-06:00"}
---

Date: [[2026 01-January 16\|2026 01-January 16]]

```
i have noticed that termux from the play store (which is unsupported and not long for this world) plays better with uv, compared to fdroid termux, due to the sandbox that android uses for sideloaded apk's

how can i continue to use uv without hitting this? uv sync does not break on either termux, but uv build breaks on fdroid termux
```
This is a known and frustrating issue. The difference you're seeing is because the F-Droid version of Termux is built against a more modern Android Target SDK. This triggers stricter SELinux and **no-exec** restrictions on the `/tmp` and `/data` partitions for "sideloaded" (non-Play Store) apps.

When `uv build` runs, it creates temporary build environments and tries to execute build-backend scripts (like `hatchling` or `setuptools`) from a temporary directory.1 On F-Droid Termux, the OS blocks this execution, causing the build to fail.

Here is how you can bypass this "sandbox" restriction on F-Droid Termux.

---

### 1. The "Home Directory" Fix (Recommended)

By default, `uv` uses the system `/tmp` for builds. On Android, you cannot execute anything in `/tmp`. You must force `uv` to use a subdirectory in your `$HOME` folder, where you actually have execution permissions.

Set the `TMPDIR` environment variable before running your build:

Bash

```
# Create a local tmp folder
mkdir -p $HOME/.tmp

# Run the build with TMPDIR redirected
TMPDIR=$HOME/.tmp uv build
```

### 2. Configure `uv` to use Local Temp permanently

To avoid typing that every time, you can export this in your `.bashrc` or `.zshrc`:

Bash

```
export TMPDIR=$HOME/.tmp
```

Since `uv` respects standard Unix environment variables, it will now perform its internal "venv-within-a-build" logic inside your home directory, bypassing the F-Droid exec-shield.

---

### 3. Why `uv sync` works while `uv build` breaks

It’s helpful to understand why one works and the other doesn't:

- **`uv sync`**: Mostly downloads wheels and unpacks them. It's doing "file moves," which Android allows.
    
- **`uv build`**: It has to "spin up" a mini-build-environment. It attempts to run a Python interpreter to execute the build-backend (like `hatchling`). Android sees an app trying to execute a script from a temporary directory and kills the process.
    

### 4. Adjusting your `build_pyz.py` for Termux compatibility

Since your script calls `uv build`, you can make the script itself "Termux-aware" by setting the environment variable programmatically:

Python

```
def build_wheel():
    """Builds the wheel, handling Termux sandbox restrictions."""
    print("\n1. Building project wheel...")
    
    # Create a local tmp if on Termux to bypass no-exec restrictions
    custom_env = os.environ.copy()
    if pyhabitat.on_termux():
        termux_tmp = Path.home() / ".tmp"
        termux_tmp.mkdir(exist_ok=True)
        custom_env["TMPDIR"] = str(termux_tmp)
        print(f"Termux detected: Redirecting TMPDIR to {termux_tmp}")

    run_command(["uv", "build", "--wheel", "--out-dir", str(DIST_DIR)], env=custom_env)
```

---

### Summary of the Fix

|**Problem**|**Root Cause**|**Solution**|
|---|---|---|
|`uv build` crashes|Android "no-exec" on `/tmp`|Set `TMPDIR=$HOME/.tmp`|
|Sideloaded app restriction|Target SDK SELinux policy|Run builds inside `$HOME`|

This will allow you to stay on the F-Droid version (which is the only one receiving updates) while keeping the speed of `uv`.

**Would you like me to integrate this `TMPDIR` logic into the final version of the `build_pyz.py` script for you?**