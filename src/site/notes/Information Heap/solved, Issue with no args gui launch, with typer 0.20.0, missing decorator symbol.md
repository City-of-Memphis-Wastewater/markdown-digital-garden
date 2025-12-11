---
{"dg-publish":true,"permalink":"/information-heap/solved-issue-with-no-args-gui-launch-with-typer-0-20-0-missing-decorator-symbol/","noteIcon":"","created":"2025-12-11T13:21:33.330-06:00"}
---

Date: [[-Daily Activity Log-/2025 12-December 11\|2025 12-December 11]]



### Title

**`@app.callback()` Fails to Execute Default Action (`invoke_without_command=True`) in Complex Environment, Leading to Silent Exit.**

### Problem Description

When using `typer.Typer(invoke_without_command=True)` to define a default action (GUI launch) via the `@app.callback()` decorator, the application terminates instantly and silently when executed with no arguments. 

### Investigation  / Comparisons
I have completed these comparisons:
1. Use just Click instead of Typer+Click:
	- Same: Environment, package, version of Click, version of Typer
	- Changed: Typer CLI file (buggy) vs comparable Click CLI file (succeeds)
	- Result: The Click CLI works, no problem
	- Files compared:
		- Good: github.com/city-of-memphis-wastewater/pdflinkcheck/tree/main/src/pdflinkcheck/cli.py
		- Bug: github.com/city-of-memphis-wastewater/pdflinkcheck/tree/main/src/bug_record/cli_typer_no_args_no_gui_silent.py 
2. Ensure that the logic in the buggy Typer CLI is comparable to a known-good Typer CLI which successfully launches a GUI when there are no arguments. 
	- Different: environment, package, version of Typer, version of Click
	- Same: Typer logic. 
	- Files compared:
		- Good: github.com/city-of-memphis-wastewater/pipeline/tree/main/src/pipeline/cli.py
		- Bug: github.com/city-of-memphis-wastewater/pdflinkcheck/tree/main/src/bug_record/cli_typer_no_args_no_gui_silent.py
3. Test the buggy Typer CLI, after changing the Click and Typer Versions
	- Same: Same environment, same package
	- Changed: Different version of Click, Typer CLI vs comparable Click CLI
	- Result: Typer CLI still fails after changing to known good version
	- Uninvestigated: The known good version works with Poetry, but this Typer version comparison was only performed with UV for each case. 
		- Bug: github.com/city-of-memphis-wastewater/pdflinkcheck/tree/main/src/bug_record/cli_typer_no_args_no_gui_silent.py 
		- Bug: github.com/city-of-memphis-wastewater/pdflinkcheck/tree/main/src/bug_record/cli_typer_no_args_no_gui_silent.py 

Same information, more table:

| **Comparison Focus**                                                         | **Versions (Buggy File)**                                                      | **Environment & Logic**                                                                                                           | **Outcome**                        | **Conclusion**                                                                                                                                                                                                                         |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Typer Layer Isolation** (Typer vs. Pure Click)                          | Typer v0.20.0 / Click v8.3.1 (in `pdflinkcheck`)                               | Same package environment. Logic changed from **Typer CLI** (buggy) to **Pure Click CLI** (working).                               | **Pure Click Succeeded.**          | **The bug is isolated to the Typer layer.** Typer's context initialization or dispatch fails where Click's core logic succeeds.                                                                                                        |
| **2. Regression Analysis** (Typer v0.20.0 vs. v0.17.5)                       | Failing: v0.20.0 (UV/`pdflinkcheck`) / Succeeding: v0.17.5 (Poetry/`pipeline`) | Logic is comparable (GUI launch on no args). Different dependency managers (`uv` vs. `poetry`) and Typer/Click versions.          | **Older Typer v0.17.5 Succeeded.** | Suggests a **regression or breaking change** in environmental interaction between Typer v0.17.5 and v0.20.0.                                                                                                                           |
| **3. Version Pinning Test** (Typer Pinning in UV, Typer v0.20.0 vs. v0.17.5) | Typer v0.17.5 / Click v8.1.8 (Pinned into `pdflinkcheck` via UV)               | Same environment (`pdflinkcheck` managed by `uv`). Typer CLI was pinned to the "known-good" versions from the `pipeline` project. | **Typer CLI Still Fails.**         | **The bug is not solely due to the Typer/Click version numbers.** It indicates a deeper issue related to the interaction between **Typer logic, the package environment, and potentially the dependency manager (`uv` vs. `poetry`).** |
### Environment Details for Bug

| **Component**          | **Value**                              | **Notes**                        |
| ---------------------- | -------------------------------------- | -------------------------------- |
| **Operating System**   | Ubuntu 24.04.1.68.0 WSL2 on Windows 11 |                                  |
| **Python Version**     | 3.12.3                                 |                                  |
| **Dependency Manager** | `uv`                                   | `uv tree` output provided below. |
| **Typer Version**      | `0.20.0` (and `0.17.3`)                | (Installed via `uv`)             |
| **Click Version**      | `8.3.1` (and `8.1.8`)                  | (Typer dependency)               |


**`uv tree` Output for Current Project:**

```
pdflinkcheck v1.1.5
├── pyhabitat v1.0.53
├── pymupdf v1.26.6
├── rich v14.2.0
│   ├── markdown-it-py v4.0.0
│   └── pygments v2.19.2
├── typer v0.20.0
│   ├── click v8.3.1
│   └── rich v14.2.0 (*)
...
```

### Description of the Bug

The bug manifests only in the production environment (the `pdflinkcheck` repository).

**Failing Code Location:** `github.com/city-of-memphis-wastewater/pdflinkcheck/tree/main/src/bug_record/cli_typer_no_gui_no_args_silent.py`

**Key Expectation:** The application is configured to run the callback (`main` function) when no subcommand is given:

```python
console = Console()
app = typer.Typer(
    add_completion=False,
    invoke_without_command = True,
    no_args_is_help = False,
)

@app.callback() 
def main(ctx: typer.Context):
	if ctx.invoked_subcommand is None:
	start_gui()
```


**Actual Behavior (Failing):**

The process terminates instantly and silently. The code within main() function is bypassed entirely. Print statements were included at the top of the app.callback designated function `main()`) for troubleshooting and were not triggered.



### Proof of Typer-Specific Failure

When the failing Typer logic is replaced with its pure Click equivalent in the same environment, the issue disappears:

| **Structure**          | **Default Action Execution**      | **Conclusion**                              |
| ---------------------- | --------------------------------- | ------------------------------------------- |
| **Typer (0.20.0)**     | Fails (Silent Exit)               | The Typer wrapper's context handling fails. |
| **Pure Click (8.3.1)** | Works (GUI launches successfully) | Click's core dispatch logic is correct.     |

### Workaround

The current necessary fix is to manually intercept the process call before the Typer dispatcher runs:

Python

```
if __name__ == "__main__":
    # Intercept the no-argument execution BEFORE calling app()
    if len(sys.argv) <= 1:
        _launch_default_gui()
    app()
```

### Additional Context

A minimal reproducible example (MRE) could not be created for this bug, suggesting the failure is a subtle interaction between Typer's initialization and one or more complex dependencies (e.g., `rich`, `pyhabitat`) when run in the specified environment.


---

## Environmental Comparison between Typer v0.20.0 (failing) in uv-managed package and Typer v0.17.5 in poetry managed package (succeeding)


**`@app.callback()` Fails to Execute Default Action (`invoke_without_command=True`) in Complex Environment (Typer 0.20.0), Works in Older Version (Typer 0.17.5).**

### Problem Description

When using `typer.Typer(invoke_without_command=True)` to define a default action via the `@app.callback()` decorator, the application terminates instantly and silently when executed with no arguments.

Crucially, implementing **comparable logic** in a separate project using **Typer 0.17.5 works correctly.** This indicates a **regression or change in environmental interaction** between Typer versions, specifically in how the application's context is initialized in the presence of various dependencies.

### Environment Details (Failing Project: `pdflinkcheck`)

| **Component**          | **Value**                                            | **Notes**                                                                         |
| ---------------------- | ---------------------------------------------------- | --------------------------------------------------------------------------------- |
| **Operating System**   | Ubuntu 24.04.1.68.0 WSL2 on Windows 11               |                                                                                   |
| **Python Version**     | 3.12.3                                               |                                                                                   |
| **Dependency Manager** | `uv` (Managing dependencies via `pyproject.toml`)    |                                                                                   |
| **Typer Version**      | **`0.20.0`** (Failing Version)                       | Pinned in `pyproject.toml`.                                                       |
| **Click Version**      | `8.3.1`                                              | Typer dependency.                                                                 |

### Environmental Analysis (Working vs. Failing)

This section highlights the critical version difference that separates the working project from the failing one.

#### 1. Working Reference Project (`pipeline`)

- **Repository:** `github.com/city-of-memphis-wastewater/pipeline`
    
- **CLI Code:** github.com/city-of-memphis-wastewater/pipeline/tree/main/src/pipeline/cli.py
    
- **Dependency Manager:** `Poetry`
    
- **Typer Version:** **`0.17.5`** (Pinned in `pyproject.toml` for Termux compatibility).
    
- **Click Version:** `8.1.8`
    
- **Status:** **Works Correctly.** The default action executes as intended when run with no arguments.
    

#### 2. Failing Project (`pdflinkcheck`)

-  **Repository:** `github.com/city-of-memphis-wastewater/pdflinkcheck`
    
- **CLI Code, Bug Record, Typer v0.20.0 + Click v8.1.8:** github.com/city-of-memphis-wastewater/pdflinkcheck/tree/main/src/bug_record/cli_typer_no_args_no_gui_silent.py
	
- **Dependency Manager:** `uv`
    
- **Typer Version:** **`0.20.0`**
    
- **Click Version:** `8.3.1`
    
- **Status:** **Fails.** Silent exit when run with no arguments.
    

**Conclusion from Analysis:** The issue appears to be a regression in Typer between versions `0.17.5` and `0.20.0`, or a critical change in how Typer interfaces with Click version 8.3.1, specifically regarding argument validation for the main callback.

---

## Proof of Typer-Specific Failure (Comparison between Typer v0.20.0 CLI version to Click CLI Click v8.3.1 CLI version in uv-managed  package)

A minimal reproducible example (MRE) could not be created for this bug, suggesting the failure is an environmental interaction. However, the fact that the equivalent structure in was produced in **pure Click v8.3.1**suggests this is a Typer issue.

The Typer v0.20.0 + Click v8.3.1 version which demonstrates the bug: github.com/city-of-memphis-wastewater/pdflinkcheck/tree/main/src/bug_record/cli_typer_no_gui_no_args_silent.py

The pure-Click  v8.3.1 CLI version has been implemented in `pdflinkcheck` to circumvent the problem: github.com/city-of-memphis-wastewater/pdflinkcheck/tree/main/src/pdflinkcheck/cli.py

----
# Actual solution
I had a missing decorator symbol

```
# BROKEN
app.callback() <---- Missing @ symbol for decorator
def main(ctx: typer.Context):
    if ctx.invoked_subcommand is None:
		start_gui()

# FIXED
@app.callback()
def main(ctx: typer.Context):
    if ctx.invoked_subcommand is None:
		start_gui()

```

```
# once the @ symbol was added to `@app.callback()`, everything works in the test file.
  SHOW_NO_ARG_BUG=0 uv run python src/bug_record/cli_typer_no_args_no_gui_silent.py
SHOW_NO_ARG_BUG 0
GUI is expected to launch.
pdflinkcheck: start_gui ...
pdflinkcheck: gui closed.
  SHOW_NO_ARG_BUG=1 uv run python src/bug_record/cli_typer_no_args_no_gui_silent.py
SHOW_NO_ARG_BUG 1
GUI is expected to fail.
pdflinkcheck: start_gui ...
pdflinkcheck: gui closed.
 
```