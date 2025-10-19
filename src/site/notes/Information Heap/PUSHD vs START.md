---
{"dg-publish":true,"permalink":"/information-heap/pushd-vs-start/","noteIcon":"","created":"2025-10-03T03:47:59.639-05:00"}
---

Date: [[-Daily Activity Log-/2025 10-October 03\|2025 10-October 03]]

## Command Comparison: START vs. PUSHD (Windows Command Prompt)

|Feature|START|PUSHD|
|---|---|---|
|**Primary Goal**|**Process Management & Execution.** To launch a program, open a document, or run a command in a new window/process.|**Directory Stack Management.** To save the current working directory to a memory stack and simultaneously change to a new location.|
|**Introduction**|**MS-DOS (Likely version 5.0+, definitely by Windows 95/NT).** A very old, fundamental command for interfacing with the shell/desktop environment.|**Windows NT / Later CMD Extensions.** Introduced later than `START` to provide advanced, stack-based directory management, mirroring similar functionality in Unix shells (like Bash).|
|**Related Command**|`WAIT` (flag), `TITLE` (first quoted argument), `/B` (run without new window).|`POPD` (Pop Directory). This command retrieves and removes the topmost directory from the stack, returning the user to that saved location.|
|**Scope of Action**|Affects the operating system's process table and window management. It creates new resources (a process/window).|Affects the current shell instance's environment variables (`%CD%` and the internal directory stack).|
|**Syntax Example**|`START "My Title" /MIN python.exe pipeline.pyz --run`|`PUSHD "%~dp0"` (Saves current path, changes directory to batch file path)|
|**Typical Usage**|Launching GUI programs, opening documents in their default editor, or, as in our case, running a console application in a new, independent window that can be explicitly controlled (e.g., minimized or waited upon).|Essential for complex batch scripts that must navigate into a specific directory (like the `dist/` folder) to run an executable, and then automatically return to the original path when finished. Also used to temporarily map UNC network paths to a drive letter.|
|**Community Perception**|Extremely common. Often the first command developers learn to use to prevent a batch file window from closing immediately. Widely used in desktop shortcuts and simple `.bat` files.|Highly regarded by advanced scripters for reliability and robustness. Considered superior to saving the original path in an environment variable (`SET ORIGINAL_DIR=%CD%`) because `PUSHD` supports nested calls (the stack).|

### Detailed Usage Notes

#### PUSHD and POPD Stack

The `PUSHD` command operates on a Last-In, First-Out (LIFO) stack. Every time `PUSHD` is called, the current location is pushed onto the stack. `POPD` then retrieves and removes the most recently pushed location, making it the current working directory. This nesting capability is crucial for calling functions or subroutines that temporarily change the directory.

#### START and Quoting Behavior

A common pitfall with `START` is its quoting rule: If the first argument to `START` is enclosed in quotation marks, it is **always** treated as the new window's **Title**. To run a command or path that contains spaces and must be quoted, an empty title must be provided first:

- **Wrong (Treats executable path as title):** `START "C:\Program Files\App.exe"`
    
- **Correct (Runs App.exe with no title):** `START "" "C:\Program Files\App.exe"`
    
- **Correct (Runs App.exe with "Pipeline App" title):** `START "Pipeline App" "C:\Program Files\App.exe"`