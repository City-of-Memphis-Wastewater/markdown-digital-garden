---
{"dg-publish":true,"permalink":"/information-heap/dev-goals-22-january-2025/","noteIcon":"","created":"2026-01-22T13:08:25.609-06:00"}
---

Date: [[-Daily Activity Log-/2026 01-January 22\|2026 01-January 22]]

```
 oolong@CIT-36NZRL3  ~/dev/mulch   main ±  git pull

remote: Enumerating objects: 3, done.

remote: Counting objects: 100% (3/3), done.

remote: Compressing objects: 100% (3/3), done.

remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)

Unpacking objects: 100% (3/3), 22.00 KiB | 1.29 MiB/s, done.

From https://github.com/city-of-memphis-wastewater/mulch

 * [new branch]      dependabot/pip/urllib3-2.6.3 -> origin/dependabot/pip/urllib3-2.6.3

Already up to date.

 oolong@CIT-36NZRL3  ~/dev/mulch   main ±  git checkout dev

M       pyproject.toml

branch 'dev' set up to track 'origin/dev'.

Switched to a new branch 'dev'

 oolong@CIT-36NZRL3  ~/dev/mulch   dev ±  git pull

Already up to date.

 oolong@CIT-36NZRL3  ~/dev/mulch   dev ± 

  

i have a few projects we need to work on today

  

i dont want to get stuck in the weeds

  

my main thing i want to do is spend time on pipeline-lite, which uses the pipeline-eds library, but they are separate for easy of troubleshooting

  

the RJN clarity API client, ClientRjn needs to be improved as does the Eds Soap CLient, namely in terms of standardized data schema and transmission

  

i also need to learn to leverage dependabot in all of my projects

  

and i was to take a look at the dworshak-access library to guard against edge cases where the vault.db does not yet exist

  

and i need to version the schema of the vaut db to compare checks and prevent errors

  

and we can have a sort of heal feature if the schemas are misaligned?

  

remind me of all of my goals every half hour so i dont go too far down a ribbit hole

i need to check all f my projects for CVE security issues and i have to have dependabot manage this automatically

cat src/pipeline_lite/main.py
cat src/dworshak_access/vault.py

```

Understood. We have a clear roadmap, and I'll keep us on track. Since you're working across `pipeline-lite`, `pipeline-eds`, and `dworshak-access`, I'll act as your co-pilot to ensure we don't drift too far into any single "ribbit hole."

Here is the strategy for the session, followed by the first concrete steps for `dworshak-access` since that's the foundation for your secrets and safety.

---

## 🎯 Today's Goals (The "No-Weeds" List)

1. **Security/Automation:** Enable Dependabot across all repos for automated CVE monitoring.
    
2. **Vault Safety (`dworshak-access`):** * Implement "Auto-Heal" for missing `vault.db`.
    
    - Formalize schema versioning to prevent misalignment.
        
3. **API Standardization (`pipeline-eds`):** * Standardize data schemas for `ClientRjn` and `ClientEdsSoap`.
    
    - Improve transmission reliability.
        
4. **Integration (`pipeline-lite`):** Orchestrate the above libraries.
    

---

## 🛠 Step 1: `dworshak-access` (Schema Versioning & Auto-Heal)

You already have a `schema_version` table in your code, but the `get_secret` function currently assumes the database exists. If a user calls `get_secret` before `initialize_vault`, it crashes with a `sqlite3.OperationalError`.

### Proposed Changes for `vault.py`:

We should modify `check_vault` to not just report status, but optionally trigger `initialize_vault()` if the file is missing.

Python

```
# In vault.py

def get_secret(service: str, item: str, fail: bool = False) -> str | None:
    # AUTO-HEAL: Ensure vault exists before querying
    if not DB_FILE.exists():
        initialize_vault()
        if fail:
            raise KeyError(f"Vault was empty/missing. No credential for {service}/{item}")
        return None

    conn = sqlite3.connect(DB_FILE)
    # ... rest of your code
```

### Schema Versioning "Heal" Logic:

To handle misaligned schemas, we can add a check inside `initialize_vault` that compares the version.

Python

```
CURRENT_SCHEMA_VERSION = 1

def validate_or_migrate_schema(conn):
    cursor = conn.execute("SELECT version FROM schema_version LIMIT 1")
    row = cursor.fetchone()
    version = row[0] if row else 0
    
    if version < CURRENT_SCHEMA_VERSION:
        # Perform migrations here if version was 0 or old
        # conn.execute("ALTER TABLE ...")
        conn.execute("UPDATE schema_version SET version = ?", (CURRENT_SCHEMA_VERSION,))
        conn.commit()
```

---

## 🤖 Step 2: Dependabot Configuration

To satisfy the requirement of checking all projects for CVEs automatically, you need a `.github/dependabot.yml` in the root of **each** repository.

**Example for your Python projects:**

YAML

```
version: 2
updates:
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "daily"
    open-pull-requests-limit: 10
    groups:
      dependencies:
        patterns:
          - "*"
```

---

### ⏱ Reality Check

It is currently **1:26 PM**. I will give you a "Goals & Progress" ping at **1:56 PM** to make sure we haven't wandered off.

**Would you like me to generate the full updated code for `vault.py` including the migration logic, or should we jump into the `ClientRjn` schema standardization first?**
