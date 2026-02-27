---
{"dg-publish":true,"permalink":"/information-heap/cli-policy/","noteIcon":"","created":"2026-02-15T19:17:43.860-06:00"}
---

Date: [[2026 02-February 15\|2026 02-February 15]]

Here’s a clear demonstration of **Option 2** applied to both versions of your CLI:

**Policy recap (Option 2)**  
- **Required / core arguments** → positionals only  
- **Optional / tweak arguments** → long flags only (`--path`, `--overwrite`, `--message`, etc.)  
- **Prompt fallback** → only when safe (i.e. not in CI / non-interactive context), and only for the missing piece that makes sense to prompt for  
- Use `pyhabitat.is_likely_ci_or_non_interactive()` (which you already improved) to decide whether prompting is allowed

### Typer version (`cli.py`) — updated to Option 2

```python
# src/dworshak_env/cli.py
import sys
import typer
from pathlib import Path
from typing import Optional
from rich.console import Console

# Assume this is now in pyhabitat
from pyhabitat import is_likely_ci_or_non_interactive

from .core import DworshakEnv
from ._version import __version__

console = Console()
app = typer.Typer(
    name="dworshak-env",
    help=f"Store and retrieve plaintext, single-key configuration values to typical .env file. (v{__version__})",
    no_args_is_help=True,
    add_completion=False,
    context_settings={"help_option_names": ["-h", "--help"]},
)


@app.callback(invoke_without_command=True)
def main(
    ctx: typer.Context,
    version: Optional[bool] = typer.Option(None, "--version", is_flag=True, help="Show the version."),
):
    if version:
        typer.echo(__version__)
        raise typer.Exit(0)


@app.command()
def get(
    key: str = typer.Argument(..., help="The key (e.g. PORT, API_KEY)"),
    path: Optional[Path] = typer.Option(None, "--path", help="Custom .env file path"),
):
    """Retrieve a value from the .env file."""
    env_mgr = DworshakEnv(path=path)
    value = env_mgr.get(key=key)

    if value is not None:
        typer.echo(value)  # raw value → stdout for capture
    else:
        typer.echo(f"Error: key '{key}' not found", err=True)
        raise typer.Exit(1)


@app.command()
def set(
    key: str = typer.Argument(..., help="The key (e.g. PORT, API_KEY)"),
    value: Optional[str] = typer.Argument(
        default=None,
        help="The value to store. If omitted → prompt (interactive) or error (CI/non-tty)",
    ),
    path: Optional[Path] = typer.Option(None, "--path", help="Custom .env file path"),
    overwrite: bool = typer.Option(False, "--overwrite", "--force", help="Overwrite without confirmation"),
):
    """
    Store or update a value in the .env file.

    Preferred usage:
      dworshak-env set PORT 8080
      dworshak-env set API_KEY          # → prompts if interactive
      dworshak-env set API_KEY xxxxx --overwrite --path .custom.env
    """
    env_mgr = DworshakEnv(path=path)
    existing = env_mgr.get(key=key)

    # Early exit if exists and no overwrite
    if existing is not None and not overwrite:
        typer.echo(existing)           # raw value to stdout
        return

    # Decide how to get the value
    is_non_interactive = is_likely_ci_or_non_interactive()

    if value is None:
        if is_non_interactive:
            raise typer.BadParameter(
                "Value is required in non-interactive mode. "
                "Provide it as the second positional argument."
            )
        # Safe to prompt
        prompt_text = f"Value for {key}"
        value = typer.prompt(prompt_text, hide_input=False)  # change to True if secret-like

    # Perform the set
    final_value = env_mgr.set(key=key, value=value, overwrite=overwrite)

    if final_value is not None:
        typer.echo(f"Stored [{key}] successfully.", err=True)
        typer.echo(final_value)  # raw value to stdout
    else:
        typer.echo(f"Error: Failed to set [{key}]", err=True)
        raise typer.Exit(1)


if __name__ == "__main__":
    app()
```

### Stdlib / lite version (`cli_stdlib.py`) — updated to Option 2

```python
# src/dworshak_env/cli_stdlib.py
import sys
import argparse
from pathlib import Path

# Assume improved version is here
from pyhabitat import is_likely_ci_or_non_interactive

from .core import DworshakEnv
from ._version import __version__


def notify(msg: str, file=sys.stderr):
    print(msg, file=file, flush=True)


def main() -> int:
    parser = argparse.ArgumentParser(
        prog="dworshak-env",
        description=f"Store and retrieve .env values (v{__version__})",
        add_help=False,
    )
    parser.add_argument("-h", "--help", action="help")
    parser.add_argument("--version", action="version", version=f"%(prog)s {__version__}")

    subparsers = parser.add_subparsers(dest="command", required=True)

    # get
    get_p = subparsers.add_parser("get", help="Retrieve a value")
    get_p.add_argument("key", help="The key")
    get_p.add_argument("--path", type=Path, help="Custom .env path")

    # set – positionals for core, flags for tweaks
    set_p = subparsers.add_parser("set", help="Store or update a value")
    set_p.add_argument("key", help="The key")
    set_p.add_argument("value", nargs="?", default=None, help="Value to store (optional → prompt if interactive)")
    set_p.add_argument("--path", type=Path, help="Custom .env path")
    set_p.add_argument("--overwrite", "--force", action="store_true", help="Overwrite without asking")

    args = parser.parse_args()

    env_mgr = DworshakEnv(path=args.path)

    if args.command == "get":
        val = env_mgr.get(args.key)
        if val is not None:
            print(val)  # raw stdout
            return 0
        notify(f"Error: key '{args.key}' not found")
        return 1

    elif args.command == "set":
        existing = env_mgr.get(args.key)

        # Early exit: exists + no overwrite → print current value
        if existing is not None and not args.overwrite:
            print(existing)  # raw stdout
            return 0

        value = args.value

        # Handle missing value
        if value is None:
            if is_likely_ci_or_non_interactive():
                parser.error("value is required in non-interactive mode (provide as second argument)")
            # Safe to prompt
            try:
                value = input(f"Value for {args.key}: ").strip()
            except KeyboardInterrupt:
                notify("\nInterrupted.")
                return 130
            except EOFError:
                notify("Error: No input received.")
                return 1

        # Perform set
        final = env_mgr.set(args.key, value, overwrite=args.overwrite)

        if final is not None:
            notify(f"Stored [{args.key}] successfully.")
            print(final)  # raw stdout
            return 0
        else:
            notify(f"Error: Failed to set [{args.key}]")
            return 1

    return 0


if __name__ == "__main__":
    sys.exit(main())
```

### Summary – what changed / what this achieves

- `set` now uses **two positionals**: `key` (required), `value` (optional → prompt if safe)
- `--path`, `--overwrite` are **long flags only** (tweaks)
- No flags exist for `key` or `value` → no competing ways to supply the same thing
- Prompt logic is guarded by `is_likely_ci_or_non_interactive()` in both implementations
- Stdlib version uses plain `input()` (you can switch to `getpass.getpass()` if values should be hidden)
- Stdout = raw value only (good for `export MYKEY=$(dworshak-env get MYKEY)`)
- Stderr = status / errors / human messages

This matches the **Option 2** ethos you wanted: clean, predictable, script-friendly, interactive where appropriate, and safe in CI/containers.

Let me know if you want to adjust prompt hiding, add `--message`, or handle any other edge case!