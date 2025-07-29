---
{"dg-publish":true,"permalink":"/dev/mulch/flaglock/","noteIcon":"","created":"2025-07-29T07:07:45.281-05:00"}
---

Date: [[-Daily Activity Log-/2025 07-July 29\|2025 07-July 29]]

# CG 3
```
Perfect — thanks for the updated CLI code. Based on what you’ve shared, we can now do the following:

---

## ✅ Final List of Flags to Track in `flags.lock`

From your updated `init()` and `workspace()` commands, here are the relevant flags:

### For `init`:

- `target_dir: Path`
    
- `enforce_mulch_folder: bool`
    
- `stealth: bool`
    

### For `workspace`:

- `target_dir: Path`
    
- `name: str`
    
- `here: bool`
    
- `set_default: bool`
    
- `enforce_mulch_folder: bool`
    
- `stealth: bool`
    

We **will not** include `target_dir` or `name` in the _flag comparison_ logic, but we’ll still record them for historical/debugging purposes.

---

## 🔒 `flags.lock.json` Format (Example)

```json
{
  "init": {
    "stealth": true,
    "enforce_mulch_folder": false,
    "target_dir": "/Users/alex/project",
    "timestamp": "2025-07-29T06:20:00Z"
  },
  "workspace": {
    "here": false,
    "set_default": true,
    "stealth": false,
    "enforce_mulch_folder": true,
    "target_dir": "/Users/alex/project",
    "name": "2025-07-29__morning_experiment",
    "timestamp": "2025-07-29T06:22:10Z"
  }
}
```

---

## 🧠 Full `FlagsLockStatus` Class

Here’s the full version that matches your actual CLI setup:

```python
# pipeline/core/flags_lock_status.py

import json
from pathlib import Path
from datetime import datetime
from typing import Optional


class FlagsLockStatus:
    def __init__(self, workspace_root: Path):
        self.lock_path = workspace_root / ".mulch" / "flags.lock.json"
        self.flags = {
            "init": {},
            "workspace": {}
        }
        self._load()

    def _load(self):
        if self.lock_path.exists():
            try:
                with self.lock_path.open("r") as f:
                    self.flags = json.load(f)
            except Exception as e:
                print(f"[warn] Failed to load flags.lock.json: {e}")

    def update_init(self, *, stealth: bool, enforce_mulch_folder: bool, target_dir: Path):
        self.flags["init"] = {
            "stealth": stealth,
            "enforce_mulch_folder": enforce_mulch_folder,
            "target_dir": str(target_dir),
            "timestamp": self._now()
        }
        self._save()

    def update_workspace(self, *, here: bool, set_default: bool, stealth: bool, enforce_mulch_folder: bool, target_dir: Path, name: str):
        self.flags["workspace"] = {
            "here": here,
            "set_default": set_default,
            "stealth": stealth,
            "enforce_mulch_folder": enforce_mulch_folder,
            "target_dir": str(target_dir),
            "name": name,
            "timestamp": self._now()
        }
        self._save()

    def compare(self, command: str, **current_flags) -> bool:
        """Returns True if current flags match stored ones (excluding timestamp, name, target_dir)."""
        stored = self.flags.get(command, {})
        for key, value in current_flags.items():
            if key in {"timestamp", "target_dir", "name"}:
                continue
            if stored.get(key) != value:
                return False
        return True

    def _now(self) -> str:
        return datetime.utcnow().isoformat() + "Z"

    def _save(self):
        self.lock_path.parent.mkdir(parents=True, exist_ok=True)
        with self.lock_path.open("w") as f:
            json.dump(self.flags, f, indent=2)

    def get_flags(self, command: str) -> Optional[dict]:
        return self.flags.get(command)
```

---

## 🧩 Example Integration in `cli.py`

Add near the top:

```python
from pipeline.core.flags_lock_status import FlagsLockStatus
```

Then inside `init()`:

```python
lock = FlagsLockStatus(target_dir)
lock.update_init(
    stealth=stealth,
    enforce_mulch_folder=enforce_mulch_folder,
    target_dir=target_dir
)
```

And in `workspace()`:

```python
lock = FlagsLockStatus(target_dir)
lock.update_workspace(
    here=here,
    set_default=set_default,
    stealth=stealth,
    enforce_mulch_folder=enforce_mulch_folder,
    target_dir=target_dir,
    name=name
)
```

---

## 🟨 Optional: Warn if flags change

You can compare and warn:

```python
if not lock.compare("init", stealth=stealth, enforce_mulch_folder=enforce_mulch_folder):
    typer.secho("⚠️ Flags used for `init` differ from the last run.", fg=typer.colors.YELLOW)
```

Or allow `--force` to override.

---

## ✅ Summary

You now have a fully integrated `flags.lock.json` system that:

- Stores meaningful flag state per command
    
- Resides in `.mulch/`
    
- Compares historical flags with current ones
    
- Can evolve easily (e.g., to support TOML, diffs, audits, reset)
    

Would you like a TOML version instead? Or maybe a `mulch flags show` CLI command to inspect it?
```