---
{"dg-publish":true,"permalink":"/information-heap/dll-and-pyd-packaging-for-pypdium2-vs-pymupdf-aka-fitz/","noteIcon":"","created":"2026-01-15T15:15:46.083-06:00"}
---

Date: [[2026 01-January 15\|2026 01-January 15]]

That directory listing is very telling. Seeing `fitz`, `pymupdf`, and `pypdfium2` all living in that temporary `_MEI` folder confirms that PyInstaller is doing exactly what it's designed to do—extracting everything to a "volatile" location.

### 1. Can you package pypdfium2 without the DLL?

The short answer is **no**. `pypdfium2` is just a Python "wrapper." All the actual PDF heavy lifting (parsing, link extraction) happens inside the compiled C++ Google PDFium binary. Without `pdfium.dll`, the Python code has no engine to talk to.

Wait, why does PyMuPDF work?

PyMuPDF actually does use a "DLL" (or a shared object), but it is usually compiled into a Python extension module (.pyd file on Windows). If you look at your ls output, you see the pymupdf folder. Inside there, you'll likely find a file like _fitz.pyd. To ThreatLocker, a .pyd is just a DLL with a different extension. If ThreatLocker is blocking pdfium.dll, it might eventually block the PyMuPDF binaries too if it hasn't already.

The "No DLL" Alternative:

If you truly want a zero-DLL, zero-binary dependency, pypdf (which you already have as a fallback) is your only option. It is "Pure Python." It's slower and sometimes misses complex links, but it never triggers "Access Denied" errors because it doesn't execute native code outside the Python interpreter itself.

---

### 2. Windows KeyboardInterrupt (The "Zombie Server" Issue)

On Linux, `signal.SIGINT` is handled gracefully. On Windows, `KeyboardInterrupt` often bypasses the standard signal handlers in a `ThreadedHTTPServer` because of how the Windows event loop handles the console.

To fix this on Windows, you shouldn't rely solely on `signal.signal`. Instead, use a `try/except` block directly around `serve_forever` and call `shutdown()` explicitly.

**Update your `stdlib_server.py` like this:**

Python

```
def main():
    with ThreadedHTTPServer((HOST, PORT), APIHandler) as httpd:
        # We use a flag to prevent multiple shutdown attempts
        is_shutting_down = False

        def shutdown_server():
            nonlocal is_shutting_down
            if is_shutting_down:
                return
            is_shutting_down = True
            print("\n[Shutting down] Closing server...")
            # .shutdown() stops .serve_forever() loop
            threading.Thread(target=httpd.shutdown, daemon=True).start()

        # Signal handlers work on Linux/Termux
        signal.signal(signal.SIGINT, lambda s, f: shutdown_server())
        signal.signal(signal.SIGTERM, lambda s, f: shutdown_server())

        try:
            httpd.serve_forever()
        except KeyboardInterrupt:
            # This is the specific catch for Windows Ctrl+C
            shutdown_server()
        finally:
            httpd.server_close()
            print("Server closed socket.")
```

---

### 3. The "Store" vs "MEI" folder confusion

In your `tree` output for the WindowsApps folder, you see almost nothing because Store apps are **encrypted** and hidden behind highly restrictive filesystem permissions.

- **The Sideloaded version:** Runs as a standard Win32 app, extracts to `Temp/_MEI`, and hits the ThreatLocker wall.
    
- **The Store version:** Runs in an AppContainer. It usually doesn't extract to `Temp` in the same way; it runs from the immutable `C:\Program Files\WindowsApps` directory.
    

**Pro-tip:** If you want the Store version to work with ThreatLocker, you need to ask for an "All files in directory" allow-rule for the `C:\Program Files\WindowsApps\GeorgeClaytonBennett.PDFLinkCheck...` path. Since the Store path contains your **Publisher Name**, it is actually much easier to whitelist than a random `_MEI` folder.

### Summary of Changes

1. **DLLs:** You're stuck with them if you want the speed of PDFium/MuPDF. Stick to `pypdf` for the "Safe" mode.
    
2. **Server:** Added the `try/except KeyboardInterrupt` inside the `main()` of the server file to catch Windows-specific interrupts.
    
3. **MSIX:** The Store version is actually more "stable" for IT admins to approve because the path doesn't change randomly like the temp folder does.
    

Would you like me to show you how to force PyInstaller to **not** use the `_MEI` folder and instead run from a fixed "AppDir"? (This makes ThreatLocker rules much easier to manage).

Those directory listings are the perfect "autopsy" of a PyInstaller executable. Seeing `mupdfcpp64.dll` and `pdfium.dll` sitting there confirms that both high-performance engines are essentially "DLL delivery systems."

To ThreatLocker, your app is just a series of "untrusted" binaries being dropped into a Temp folder—which is the textbook definition of how malware behaves, hence the blocking.

## 2. Packaging pypdfium2 differently?

If you want to keep the speed but avoid the `_MEI` folder lottery, you have two real options:

### Option A: Use `--onedir` instead of `--onefile`

If you package your app with `pyinstaller --onedir`, it doesn't extract to `Temp`. It just sits in a folder (like `C:\Program Files\PDFLinkCheck\`).

- **The Win:** The path to `pdfium.dll` becomes **static**.
    
- **The IT Fix:** Your admin can whitelist `C:\Program Files\PDFLinkCheck\pypdfium2_raw\pdfium.dll` once, and it will never break again.
    

### Option B: The "Pure" Fallback (The 1.3.7 Strategy)

Your changelog shows you've already implemented the best logical fix: **The Guarded Fallback.** By clearing the cache and checking `pdfium_is_available()`, you’ve made the app smart enough to say: _"I'd like to use the DLL, but ThreatLocker says no, so I'll just use the pure-Python pypdf instead."_

---

## 3. Observations on your Filesystem

- **The `.pyd` files:** In the `pymupdf` folder, you see `_mupdf.pyd` (11.8MB). These are effectively DLLs. If ThreatLocker starts blocking these, PyMuPDF will fail exactly like Pdfium.
    
- **The `mupdfcpp64.dll` (24MB):** This is massive. PyMuPDF is a heavy dependency compared to Pdfium's 5.7MB DLL.
    
- **The `pdflinkcheck/data` folder:** Since this is extracted to the `_MEI` folder, your `is_in_git_repo()` check correctly fails here, meaning your "Production" mode won't try to run `datacopy.py` and mess with the Temp directory.
    

## Next Steps

Now that the server shutdown is handled and the engine fallback is fixed, the app should be quite stable even in "hostile" corporate environments.

**Would you like me to help you create a specific "Portable" build script that uses the `--onedir` format for easier whitelisting?**