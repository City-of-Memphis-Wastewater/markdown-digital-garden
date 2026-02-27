---
{"dg-publish":true,"permalink":"/information-heap/standardizing-the-dworshak-ecosystem-cli-approach/","noteIcon":"","created":"2026-02-16T12:29:41.549-06:00"}
---

Date: [[Information Heap/2026 02-February 16\|2026 02-February 16]]

That is exactly the standard "Unix-like" approach for professional CLI tools. Splitting streams allows you to satisfy both the human operator (who needs context) and the machine (which needs clean data).

### The "Clean" CLI Pattern

By separating these streams, you achieve the "management tool" feel you're looking for while maintaining scriptability as a byproduct:

- **`stdout` (Standard Out):** Reserved exclusively for the **result**. In your case, this is the single value or cryptographic token. This allows a user to run `VAR=$(dworshak-config get key)` without having to parse out "Success: value is...".
    
- **`stderr` (Standard Error):** Used for **metadata, progress, and logs**. Even if the command is successful, things like "Connection established," "Decrypting...," or "Warning: secret extra not detected" should go here.
    

---

### Implementation Strategy

If you are using Python (as suggested by your `cli.py` in the logs), here is how that looks in practice:

|**Component**|**Stream**|**Python Example**|
|---|---|---|
|**The Result**|`stdout`|`print(value)` or `sys.stdout.write(value)`|
|**Status/Audit**|`stderr`|`print("Checking credentials...", file=sys.stderr)`|
|**Validation**|Exit Code|`sys.exit(0)` for match, `sys.exit(1)` for mismatch|

#### Why this excels as a management tool:

1. **Auditability:** You can redirect `stderr` to a log file (`dworshak get key 2>> audit.log`) while the actual secret still passes through to the next process or the screen.
    
2. **Noiseless Pipes:** It prevents "chatter" from breaking downstream tools.
    
3. **Human Feedback:** The user isn't staring at a blinking cursor; they see the progress/confirmation on `stderr`, but they know the only thing they can "copy-paste" or pipe is the `stdout` value.
    

---

### A Note on your Cryptography Constraint

As you work on this transition, remember to **come back to block `DworshakGet.secret`** when the `[secret]` extra has not been used to enforce cryptography in the `dworshak-prompt`.

Using `stderr` for a "Missing [secret] extra" warning while returning an empty string or error code on `stdout` would be the perfect way to enforce this safely.

**Would you like me to show you how to configure a Python `logging` handler to automatically direct all non-critical info to `stderr` so `stdout` stays clean?**


----


Your current setup is looking **very solid** — clean, consistent, and practical.

Here's a quick review of the key parts in both `cli.py` (Typer/full) and `cli_stdlib.py` (argparse/lite), plus minor polish suggestions.

### What looks great right now

1. **Core UX pattern is consistent across both CLIs**
   - `set service item [secret] [--overwrite]`
   - Optional positional `secret` → prompt only when missing + interactive
   - `--overwrite` required to replace existing → good safety
   - CI/non-tty → fails clearly when secret missing

2. **Typer version (`cli.py`)**
   - `secret: Optional[str] = typer.Argument(None, ...)` → perfect choice
   - Manual prompt with `hide_input=True` → correct for secrets
   - Overwrite check before storing → prevents silent overwrites
   - Early exit with message when exists → user-friendly

3. **Stdlib version (`cli_stdlib.py`)**
   - `nargs="?", default=None` on `secret` → mirrors Typer perfectly
   - `getpass.getpass(...)` → excellent for hiding input
   - Same overwrite logic + existence check
   - `SECRET-STD:` prefix on stderr → very clear for piping/debugging

4. **Overall policy adherence**
   - Positionals for required + main flow (`service`, `item`, optional `secret`)
   - Flags for tweaks (`--overwrite`, `--path`)
   - Conditional prompt only when safe → matches your "hidden prompting for dworshak-secret set" exception

### Minor issues / improvements to consider

#### 1. Typer version — small logic bug in overwrite handling

Right now you have:

```python
existing_secret = get_secret(service, item)
if existing_secret is not None:
    if not overwrite:
        console.print(f"Credential for {service}/{item} exists. Use --overwrite flag. ")
        raise typer.Exit(code = 0)

# ... later ...
store_secret(service, item, secret)
```

But you **store even if it exists and --overwrite is given** — that's correct.  
However, you **don't show the existing value** (like you do in some earlier versions).  
Consider adding it for better feedback:

```python
existing = get_secret(service, item)
if existing is not None and not overwrite:
    console.print(f"[yellow]Credential for {service}/{item} already exists:[/yellow]")
    console.print(f"  {existing[:8]}...{existing[-4:] if len(existing) > 12 else existing}")  # masked
    console.print("[yellow]Use --overwrite to replace it.[/yellow]")
    raise typer.Exit(0)
```

#### 2. Typer version — prompt text could be friendlier

Change

```python
secret = typer.prompt("secret", hide_input=True)
```

to

```python
secret = typer.prompt(f"Secret for {service}/{item}", hide_input=True)
```

It's more context-aware.

#### 3. Stdlib version — duplicate code block

Look:

```python
elif args.command == "set":
    # ... good block with overwrite check ...

    if not args.secret:
        if pyhabitat.is_likely_ci_or_non_interactive():
            parser.error(...)
        secret = getpass.getpass(...)

    # ── then later ──
    if secret is None and overw  # ← incomplete line, looks like leftover
    secret = getpass.getpass(...)
    existing_secret = get_secret(...)
    if secret:
        store_secret(...)
```

There's **duplicate prompt logic** + an incomplete `if secret is None and overw` line (probably copy-paste artifact).

**Fix**: Remove the second block entirely — keep only the first one (the one with `args.secret` and overwrite check).  
The cleaned version should look like:

```python
elif args.command == "set":
    existing = get_secret(args.service, args.item)

    if existing is not None and not args.overwrite:
        stdlib_notify(f"Credential for {args.service}/{args.item} already exists.")
        stdlib_notify("Use --overwrite / --force to replace.")
        return 0

    secret = args.secret
    if secret is None:
        if pyhabitat.is_likely_ci_or_non_interactive():
            parser.error("secret value required (third argument) in non-interactive mode")
        secret = getpass.getpass(f"Enter secret for {args.service}/{args.item}: ")

    if not secret.strip():
        stdlib_notify("Error: Secret cannot be empty.")
        return 1

    store_secret(args.service, args.item, secret)
    stdlib_notify("Stored successfully.")
    return 0
```

#### 4. Both versions — empty/whitespace secret guard

Add in both:

Typer:

```python
if not secret.strip():
    console.print("[red]Secret cannot be empty or only whitespace.[/red]")
    raise typer.Exit(1)
```

Stdlib:

```python
if not secret.strip():
    stdlib_notify("Error: Secret cannot be empty or only whitespace.")
    return 1
```

#### 5. --path is still a black hole

You have a funny message — consider either implementing it or removing the option entirely (to reduce confusion).

If it's planned → keep the message.  
If not → drop the argument for now.

### Final quick verdict (Feb 16, 2026)

Your CLI is in **excellent shape**:

- Consistent UX between full and lite versions
- Safe secret handling (hidden prompt, no history leak)
- Overwrite protection
- CI-safe
- Follows your stated policy with the one documented exception

Just clean up the duplicate/incomplete code in `cli_stdlib.py`, improve prompt text, add whitespace guard, and optionally mask existing value on conflict.

If you want, I can give you the **cleaned full `set` block** for both files — just say "paste cleaned set blocks" or similar.

Otherwise: ship it! This is production-ready for a personal/secure tool.