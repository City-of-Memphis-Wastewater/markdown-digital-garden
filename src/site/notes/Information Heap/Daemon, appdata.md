---
{"dg-publish":true,"permalink":"/information-heap/daemon-appdata/","noteIcon":"","created":"2025-07-07T14:23:44.751-05:00"}
---

Date: [[2025 05-May 02\|2025 05-May 02]]

# ChatGPT 0:
Got it. You're absolutely right—`projects/eds_to_rjn` shouldn’t try to be self-contained if it relies on `src/pipeline` as its core library. The better approach is to treat `src/pipeline` as the engine and let each `projects/<project>/` folder supply a scriptable, minimal entry point that gets called by the daemon.

Here's a slow, clear roadmap to reach your goal:

---

### ✅ Immediate Goals

1. **Refactor `ProjectManager`** to:
    
    - Check `%APPDATA%\memphis_pipeline\projects\<project>\` first.
        
    - Fallback to `projects/<project>/` in the repo.
        
    - Auto-create config/cache folders in `%APPDATA%` if needed.
        
2. **Update Daemon Logic**:
    
    - Load default project from `default-project.toml`.
        
    - Dynamically `import` and run a callable like `run_main()` or `run_daemon()` from `projects/<project>/main.py`.
        
3. **Define a project entry point** in `projects/eds_to_rjn/main.py`:
    
    ```python
    def run_daemon():
        from src.pipeline.corelogic import do_all_the_things  # Example
        do_all_the_things()
    ```
    
4. **Daemon startup script** (`__main__.py` or `controller.py`):
    
    - Reads default project.
        
    - Imports that project’s main.
        
    - Calls `run_daemon()`.
        

---

Would you like help rewriting the `ProjectManager` first so it can search `%APPDATA%` and gracefully fall back or scaffold paths?
