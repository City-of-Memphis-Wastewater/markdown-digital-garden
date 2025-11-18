---
{"dg-publish":true,"permalink":"/information-heap/clean-keyboard-interrupt-triggered-from-the-cli-while-two-or-more-servers-or-threads-are-or-have-been-running/","noteIcon":"","created":"2025-11-14T21:40:26.228-06:00"}
---

Date: [[2025 11-November 14\|2025 11-November 14]]

This is a fantastic place to stop and clarify the architecture. You correctly identified that your previous code was over-coupled—specifically, the core run function (`run_web_gui`) was directly importing and managing global state (`GLOBAL_SHUTDOWN_EVENT`) and making decisions about which server to launch.

The core confusion stems from trying to mix the responsibilities of **Server Utilities** and **CLI Execution**.

I'm going to lay out a clean, three-file architecture that clearly separates these concerns.

---

## Proposed Architecture for Decoupling

|**File**|**Concern**|**Responsibility**|**Dependencies**|
|---|---|---|---|
|**`pipeline/core/app.py`**|**Application Definition**|Defines the web application instance (FastAPI/Starlette) and its routes.|FastAPI/Starlette|
|**`pipeline/server/web_utils.py`**|**Server Utilities**|Provides generic, reusable functions for managing the web server lifecycle (finding ports, launching browser, starting Uvicorn in a thread).|Uvicorn, Python standard libs|
|**`pipeline/cli.py`**|**CLI Execution & Control**|Manages the main thread, signal handling (Ctrl+C), defines global state (`GLOBAL_SHUTDOWN_EVENT`), and calls the run loop.|`web_utils.py`, `core/app.py`|

---

## 1. Updating Server Utilities (`web_utils.py`)

The most important change here is **removing the dependency on `GLOBAL_SHUTDOWN_EVENT`**. This file should not know about the shutdown mechanism; it should just launch the server and return the control objects.

## 2. Defining the Application (`app.py`)

This file is necessary to have an object to pass to the launcher. It should be totally decoupled from the CLI.

Web Application Definition

Nov 14, 9:38 PM

Open

## 3. CLI Execution and Control (`cli.py`)

This file is the entry point. It defines the global shutdown mechanism, imports the application instance, and uses the decoupled `launch_server_in_thread` function to execute the non-blocking run logic.

I have fully corrected the argument handling within `run_web_gui`.

CLI Entrypoint and Run Loop

Nov 14, 9:38 PM

Open

By structuring the code this way, you achieve clear separation:

1. **Decoupling:** `run_web_gui` doesn't know _how_ the server starts; it only knows that `launch_fn` returns the objects it needs for monitoring.
    
2. **Centralized State:** `GLOBAL_SHUTDOWN_EVENT` is defined and managed strictly within the CLI entrypoint (`cli.py`).
    
3. **Reusability:** `web_utils.py` contains utility functions that could be used by _any_ Python project that needs to start a Uvicorn server in a separate thread.