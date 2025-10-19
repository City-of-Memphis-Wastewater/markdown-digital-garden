---
{"dg-publish":true,"permalink":"/information-heap/multi-interface-input-seeking/","noteIcon":"","created":"2025-10-01T16:12:10.633-05:00"}
---

Date: [[-Daily Activity Log-/2025 10-October 01\|2025 10-October 01]]

Let's say my library for my CLI program can also serve a library for a GUI program. Currently, I seek inputs with Python's input() for the console.  Is there a way to instead have the prompt cause a pop-up window, or an error in GUI to direct the user to some configuration tab in GUI?
---

That's the right direction for a robust library! The key is to check the execution environment and branch your logic accordingly.

You want your library to have three modes of operation for getting credentials:

1. **CLI Mode (Interactive):** Use `typer.prompt` and `getpass.getpass` (which you already have).
    
2. **GUI Mode (Non-interactive):** Use a simple GUI module to display a _temporary popup window_ to get the input.
    
3. **Library Mode (Non-interactive):** If no terminal or GUI is available, raise a specific error that the caller (like a main GUI application) can catch and handle (e.g., by directing the user to a permanent configuration screen).
    

Here is a common pattern to achieve this, using a lightweight, standard-library-friendly GUI module like **`tkinter`** for the popup, or a popular dedicated popup library like **`PySimpleGUI`** or **`easygui`** (though those require an extra install).

### Recommended Pattern using `tkinter` (Standard Library)

You can use the built-in `tkinter` library for simple popups without adding external dependencies.1

#### 1. Create a Helper to Check Interactivity

A simple check to see if a terminal is available is to use `sys.stdin.isatty()`.2

Python

```
import sys
import os
import getpass
from typing import Optional

def is_interactive_cli() -> bool:
    """Check if the script is running in an interactive terminal."""
    # Check if a tty is attached to stdin
    return sys.stdin.isatty() and os.isatty(sys.stdin.fileno())

# You might want another check if a display server is running, but isatty is usually sufficient.
```

#### 2. Create a Helper for GUI Popup Input

This uses `tkinter` to create a simple popup window for input.

Python

```
# new_file/gui_input_fallback.py (or a new function in security.py)
import tkinter as tk
from tkinter import simpledialog

def gui_get_input(prompt_message: str, hide_input: bool = False) -> Optional[str]:
    """
    Displays a modal GUI popup to get input when no terminal is available.
    """
    try:
        root = tk.Tk()
        root.withdraw()  # Hide the main window
        
        # Determine the display character for password input
        show_char = '*' if hide_input else None

        # Use the simpledialog module for a standard input box
        value = simpledialog.askstring(
            title="Configuration Required",
            prompt=prompt_message,
            show=show_char  # '*' for hidden/password input
        )
        
        root.destroy()
        return value
        
    except Exception:
        # Fails if tkinter is not installed or the display is not running
        return None
```

#### 3. Update Your Core Functions

Now you modify your helper functions (`_get_config_with_prompt` and `_get_credential_with_prompt`) to fall back to the GUI prompt.

Python

```
# In pipeline/security.py (updated version)
# ... imports (including your new gui_get_input function)
# ... CredentialsNotFoundError definition

def _prompt_for_value(prompt_message: str, hide_input: bool) -> str:
    """Handles prompting with a fallback from CLI to GUI."""
    
    if is_interactive_cli():
        # 1. CLI Mode (Interactive)
        typer.echo(f"\n --- One-time configuration required --- ")
        if hide_input:
            new_value = getpass.getpass(f"{prompt_message}: ")
        else:
            new_value = input(f"{prompt_message}: ")
        return new_value
        
    else:
        # 2. GUI Mode (Non-interactive fallback)
        typer.echo(f"\n --- Non-interactive process detected. Opening GUI prompt. --- ")
        value = gui_get_input(prompt_message, hide_input)
        
        if value is not None:
            return value
        else:
            # 3. Library Mode (No Terminal or GUI Display)
            raise CredentialsNotFoundError(
                f"Configuration for '{prompt_message}' is missing and "
                f"neither an interactive terminal nor a GUI display is available. "
                f"Please use a configuration utility or provide the value programmatically."
            )

def _get_credential_with_prompt(service_name: str, item_name: str, prompt_message: str, hide_password: bool = True, overwrite: bool = False) -> str:
    # ... (same credential check logic as before) ...
    
    if credential is None or overwrite:
        # Use the new centralized prompting function
        new_credential = _prompt_for_value(
            prompt_message=prompt_message, 
            hide_input=hide_password
        )
        
        # ... (rest of the saving logic) ...
        return new_credential
    
    return credential

# You would update _get_config_with_prompt in a similar way.
```

### Alternative: Error and Configuration Tab

If the application that is importing your library is a _full GUI application_ (like an Electron app or a large desktop app), you don't want popups popping up randomly.

In this case, the **best approach** is to stick to the third method above:

1. **Remove all `input`, `getpass`, and GUI popup logic from your library.**
    
2. In the functions like `get_eds_api_credentials`, if the value is missing, always **raise a specific exception** (`CredentialsNotFoundError`).
    

The main GUI application that imported your library would then do this:

Python

```
try:
    creds = get_eds_api_credentials("Maxson")
    # Use credentials
except CredentialsNotFoundError as e:
    # Catch the error, display an unmissable error popup in the GUI,
    # and direct the user to the application's permanent "Settings" tab.
    gui_app.display_error("Missing Credentials", str(e))
    gui_app.switch_to_settings_tab()
```

This makes your library cleaner and forces the caller to handle the missing configuration in a way that is consistent with their user interface.