---
{"dg-publish":true,"permalink":"/information-heap/gui-with-no-args-failing-for-pdflinkcheck/","noteIcon":"","created":"2025-12-11T11:08:27.146-06:00"}
---

Date: [[-Daily Activity Log-/2025 12-December 11\|2025 12-December 11]]

```bash
 oolong@CIT-36NZRL3  ~/dev/pdflinkcheck  ↱ dev  uv run python -m pdflinkcheck.cli --help # succeeds in showing CLI help

 Usage: python -m pdflinkcheck.cli [OPTIONS] COMMAND [ARGS]...

 A command-line tool for comprehensive PDF link analysis and reporting.

╭─ Options ────────────────────────────────────────────────────────────────────────────────────────────────╮
│ --help          Show this message and exit.                                                              │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭─ Commands ───────────────────────────────────────────────────────────────────────────────────────────────╮
│ analyze   Analyzes the specified PDF file for all internal, external, and unlinked URI/Email references. │
│ gui       Launch tkinter-based GUI.                                                                      │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────╯

 oolong@CIT-36NZRL3  ~/dev/pdflinkcheck  ↱ dev  uv run python -m pdflinkcheck.cli # no response, no behavior. possibly silent failure.
 oolong@CIT-36NZRL3  ~/dev/pdflinkcheck  ↱ dev  uv run python -m pdflinkcheck.cli gui # succeeds in launching tkinter gui
 oolong@CIT-36NZRL3  ~/dev/pdflinkcheck  ↱ dev 
```

resolution - convert to click, add issue to FastAPI/Typer: [[Information Heap/solved, Issue with no args gui launch, with typer 0.20.0, missing decorator symbol\|solved, Issue with no args gui launch, with typer 0.20.0, missing decorator symbol]]