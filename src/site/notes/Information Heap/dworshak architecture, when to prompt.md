---
{"dg-publish":true,"permalink":"/information-heap/dworshak-architecture-when-to-prompt/","noteIcon":"","created":"2026-02-11T15:16:44.223-06:00"}
---

Date: [[-Daily Activity Log-/2026 02-February 11\|2026 02-February 11]]

```
title: dworshak ecosystem

one issue: when we get, should we set if the entry is missing? when and when not. We must analyze the pipeline-eds library for what it has been doing in this regard, along side the components that were migrated outof the pipeline-eds library to ultimately create the dworshak ecosystem, which includes dwroshak, dworshak-prompt, dworshak-secret, and the upcoming dworshak-config package

  

a specific question is whether or not dworshak-config should depend on dworshak-prompt and use dworshak-prompt.DworshakPrompt to get a value when it is missing from known config

  

be patient. THis architecture is already mostly stable - assume nothing

  
(dworshak)  oolong@CIT-36NZRL3  ~/dev/dworshak   dev  dworshak secret get
Service: nope
Item: nope
No credential found for nope/nope

  

it is good and expected that a non-avaialble credential should be reported as unknown rather than falling back to being prompted

```

This is a foundational architectural decision for the **Dworshak ecosystem**. When moving from a monolithic utility like `pipeline-eds` to a modular ecosystem, the "Get-or-Prompt" pattern is often the first point of friction.

To maintain the stability and clean separation you've built, we need to distinguish between **automated retrieval** and **interactive resolution**.

---

## 1. The Heritage: `pipeline-eds`

In the original `pipeline-eds` library, the logic was often tightly coupled. If a configuration or secret was missing, the library would default to an interactive prompt because it assumed a human-in-the-loop developer environment.

In the new ecosystem, this "magic" fallback can be a liability, especially in CI/CD or automated pipelines where an unexpected `stdin` request hangs the process.

## 2. Analyzing the "Get-Set" Dilemma

Should `dworshak-config` depend on `dworshak-prompt`? Let’s look at the implications:

### When to "Set" during a "Get"

- **Developer CLI Context:** If a user runs a command and a required config is missing, prompting and then persisting that value is a "Quality of Life" feature.
    
- **Initial Setup:** During a `dworshak init` or first-run scenario.
    

### When NOT to "Set"

- **Service/Library Context:** If `dworshak-config` is being used as a library within another application, it should **fail fast** or return a `None`/Result type. It should not assume it has the right to hijack the terminal for a prompt.
    
- **Immutable Environments:** In production or containerized environments, "setting" on the fly is often impossible or a security risk.
    

---

## 3. Proposed Architecture: The "Provider" Pattern

Instead of a direct dependency where `dworshak-config` _must_ use `dworshak-prompt`, consider an **Inversion of Control** approach.

|**Component**|**Responsibility**|**Dependency**|
|---|---|---|
|**dworshak-config**|Logic for reading/writing config files/env vars.|None (Base)|
|**dworshak-prompt**|Logic for interactive terminal input/validation.|None (Base)|
|**dworshak (CLI)**|Orchestrates the two.|config + prompt + secret|

### Should `dworshak-config` depend on `dworshak-prompt`?

**No.** To keep the architecture stable and modular:

1. **Keep them decoupled:** `dworshak-config` should be a "dumb" getter/setter. It returns an error or a specific `MissingConfigError` if the key isn't found.
    
2. **Higher-level Orchestration:** The logic for "if missing, then prompt" should live in the **consumer** (like the `dworshak` CLI tool) or a specialized **manager class** in `dworshak-config` that accepts an optional `prompt_provider`.
    

Python

```
# Conceptual logic in dworshak-config
def get_config(key, prompt_if_missing=False, prompter=None):
    value = store.get(key)
    if value is None and prompt_if_missing and prompter:
        value = prompter.ask(key)
        store.set(key, value)
    return value
```

---

## 4. Secret vs. Config Logic

Your CLI output shows that `dworshak secret get` correctly reports "No credential found." This is the correct behavior for **secrets**.

- **Secrets (`dworshak-secret`):** Should **never** auto-prompt on a "get" call. Security credentials should be explicitly stored. Fallbacks lead to secrets being stored in the wrong places (like cleartext shells) by accident.
    
- **Config (`dworshak-config`):** Can be more lenient, but the "Prompt" should be an **opt-in behavior** triggered by the CLI layer, not a hardcoded dependency in the library.
    

## Next Steps

Would you like me to analyze a specific code snippet from the current `dworshak-secret` implementation to see how we can mirror its "fail-clear" logic in the upcoming `dworshak-config`?

```
so this infers that every consuming program will need to redefine the get-if-not-found logic

  

instead, an extra arg could help to allow this behavior, while the default is if not found, report None, etc

  

we already have default args for fallback when ci is used, and there is no human

  

to further guard, we need to add a timeout to dworshak-prompt, if it comes into play

mention other projects for reference that have these batteries included capacities
```

That is a sharp pivot toward a "batteries-included" internal library. If you want to avoid every consuming program reinventing the "Get-or-Prompt" wheel, you are essentially moving toward a **Strategy Pattern** where the library knows _how_ to ask, but only does so when the environment permits.

Adding a **timeout** to `dworshak-prompt` is a critical safety valve for this. Without it, a rogue `get` call in a background cron job could hang a process indefinitely.

### 1. Refined Logic: The "Interactive Get"

To satisfy the requirement that consuming programs don't have to redefine logic, `dworshak-config` can include an optional dependency on `dworshak-prompt` (or a bridge) that follows this hierarchy:

1. **Check Store:** Is the value in the config file/env?
    
2. **Check CI/Headless Flag:** If `CI=true` or a `non_interactive` flag is set, return `default` or `None`.
    
3. **Check Timeout/TTY:** If a terminal is attached, trigger `dworshak-prompt` with a strict `timeout`.
    
4. **Auto-persist (Optional):** If the prompt returns a value, save it to the config store before returning it to the caller.
    

---

### 2. Guarding with Timeouts

The `dworshak-prompt` timeout should be two-fold:

- **Total Prompt Timeout:** The max time the library will wait for _any_ input before failing.
    
- **Input Inertia:** Resetting the timer on keystrokes (optional, but standard for CLI UX).
    

If the timeout triggers, the ecosystem should raise a specific `PromptTimeoutError`. This allows the consuming program to decide whether to crash, use a hardcoded default, or skip the operation.

---

### 3. Industry References: "Batteries Included"

Several successful ecosystems handle this "Get-or-Prompt" or "Get-or-Configure" flow effectively:

- **AWS CLI / Boto3:** While mostly non-interactive, the CLI's `configure` command is the prompt gateway. However, `boto3` (the library) remains strictly non-interactive. They separate the _tool_ from the _engine_.
    
- **HashiCorp Vault / Terraform:** They use "Input" variables. If a variable is missing in Terraform, it prompts by default _unless_ `-input=false` is passed. This is the exact behavior you're describing—the logic is baked into the tool, but easily suppressed for automation.
    
- **Poetry / UV (Python Packaging):** When a credential for a private repository is missing during a `publish` or `install`, these tools will prompt for a username/password in real-time but fail immediately if they detect a non-TTY environment (like a GitHub Action).
    
- **Inquirer.js / Yeoman:** These were the pioneers of "automated prompts." They allow you to define a "default" answer that is automatically selected if the session is not interactive or if a timeout occurs.
    

---

### 4. Implementation Strategy for `dworshak-config`

To keep `dworshak-config` from being "heavy," you can implement the prompt logic as an **extra**.

Python

```
# In dworshak-config/core.py
def get(key, default=None, prompt=False, timeout=30):
    val = self._read_config(key)
    if val is not None:
        return val
        
    if prompt and not self.is_ci_env():
        try:
            # Dynamically import or use the injected prompt tool
            from dworshak_prompt import DworshakPrompt
            return DworshakPrompt.ask(key, timeout=timeout, persist=True)
        except (ImportError, TimeoutError):
            return default
            
    return default
```

This approach ensures that the "orchestration" lives inside the ecosystem libraries, so the end-user just calls `dworshak_config.get(key, prompt=True)` and it "just works."

Would you like to define the specific behavior for how `dworshak-config` should detect the `CI` environment—should it strictly look for a `DW_NON_INTERACTIVE` env var, or follow the standard `CI` boolean?

```
(cat src/pipeline_eds/security_and_config.py)

this is what i have been replacing with the dworshak ecosystem

  

what if dworshak-prompt imports dworshak-config and dworshak-secret, and dwroshak-prompt has a class DworshakGet, which can multiplex to the config or the secret based on an argument similar to hide_input, and then also can simply get thevalue if it exists or prompt for it and store it appropriately if not
```

Looking at your `pipeline_eds` legacy code, you were essentially building a **multi-modal orchestration layer** (Console -> GUI -> Web). Migrating this to the Dworshak ecosystem is a massive cleanup, but the core "logic" of how to resolve a missing value remains the same.

Your proposal—having `dworshak-prompt` act as the **Multiplexor/Orchestrator**—is a valid architectural choice, but it flips the dependency graph in a specific way. Let's analyze the "DworshakGet" approach.

### 1. The Dependency Flow

If `dworshak-prompt` imports `dworshak-config` and `dworshak-secret`, the hierarchy looks like this:

- **dworshak-config / dworshak-secret:** Low-level "dumb" storage (Files, Vaults).
    
- **dworshak-prompt:** The "Smart" layer. It knows how to ask a human AND how to save the answer.
    
- **dworshak (CLI):** The entry point.
    

**The "DworshakGet" Logic:**

You could implement a `DworshakGet.resolve()` method that takes a `secret: bool` flag.

- If `secret=True`: Check `dworshak-secret`. If missing, prompt with `hide_input=True` and save to secret vault.
    
- If `secret=False`: Check `dworshak-config`. If missing, prompt with `hide_input=False` and save to config file.
    

---

### 2. Guarding the Automated Pipeline

To avoid the "hanging CI" problem you mentioned, `DworshakGet` needs to be strictly gated. Based on your legacy code's use of `ph.interactive_terminal_is_available()`, the new logic should look like this:

|**State**|**Action**|
|---|---|
|**Value Exists**|Return value immediately (No prompt).|
|**Missing + CI/Non-Interactive**|Raise `CredentialsNotFoundError` (No prompt).|
|**Missing + Interactive + No Timeout**|Prompt user, Save result, Return value.|
|**Missing + Interactive + Timeout Hit**|Raise `PromptTimeoutError`.|

---

### 3. Proposed Interface: `DworshakGet`

Instead of every program writing `if val is None: val = prompt()`, they do this:

Python

```
from dworshak_prompt import DworshakGet

# Simple config retrieval with auto-prompt/save
db_host = DworshakGet.config("DB_HOST", default="localhost")

# Secret retrieval (always prompts if missing, never shows input)
api_key = DworshakGet.secret("PROVIDER_API_KEY", timeout=45)
```

### 4. Why Timeout is Non-Negotiable

In your legacy `pipeline_eds` code, you handled `KeyboardInterrupt` (the `{Ctrl}+C + {Enter}` quirk). In a "batteries-included" `dworshak-prompt`, a timeout is your safety net for **Headless-but-Detected-as-Interactive** environments (like some poorly configured Docker TTYs).

If `DworshakGet.secret()` is called:

1. It starts a background timer.
    
2. If no input is received in $N$ seconds, it kills the prompt and raises an exception.
    
3. This prevents "Ghost Processes" from eating resources in your ecosystem.
    

---

### 5. Architectural Reference: The "Manager" Pattern

This is very similar to how **Manager Classes** work in Java or Go ecosystems, or specifically how the **`click.prompt`** logic works in Python, but expanded.

- **Django's `cleansed_data`:** Centralizes resolution and validation.
    
- **Dotenv-expand:** Often used to resolve variables, though it rarely prompts.
    

### Risks to Watch Out For

- **Circular Dependencies:** Ensure `dworshak-config` doesn't also try to import `dworshak-prompt`. Keep the storage libs "clean" and let `dworshak-prompt` be the "heavy" one that knows about storage.
    
- **Scope Creep:** Don't let `dworshak-prompt` become the whole ecosystem. It should just be the **Resolver**.
    

### Next Step

Would you like me to draft a skeleton for the `DworshakGet` class that implements this "Get-Prompt-Set" workflow with the integrated timeout logic?

```
well timeout would also be used if the user is there but simply never notices the pop up
```

That is a critical insight. In an ecosystem that supports **GUI** and **Web** fallbacks—like your legacy `pipeline_eds` code did—the "Human-in-the-loop" isn't always looking at the terminal. A backgrounded terminal or a web-popup appearing on a different monitor can stall a developer's workflow just as effectively as a hung CI pipeline.

### 1. The "Ghost Prompt" Problem

A timeout protects against the **"I didn't see the window"** syndrome. Without it, the process stays alive, holding onto file locks, memory, or network sockets, while the developer thinks the program is "just running slow."

By implementing the timeout in `dworshak-prompt`, you enforce a **fail-fast** philosophy across the entire Dworshak ecosystem.

### 2. Strategic Implementation of `DworshakGet`

If `dworshak-prompt` is the orchestrator, it should handle the logic of "Try to find it, and if I have to ask, I'll only wait so long."

#### Proposed Logic Flow:

1. **Check Config/Secret:** Look for the key in `dworshak-config` or `dworshak-secret`.
    
2. **Evaluate Context:** Is this a TTY? Is `CI=true`?
    
3. **Initiate Timed Prompt:** * Spawn the prompt (Console, GUI, or Web).
    
    - Start a timer (e.g., 60 seconds).
        
4. **On Timeout:** Raise `DworshakTimeoutError`.
    
5. **On Success:** * **Set:** Persist the value back to the appropriate store (`config` or `secret`).
    
    - **Return:** Give the value to the calling program.
        

---

### 3. The Dependency Graph

To avoid circularity, the hierarchy stays clean:

- **Level 0:** `dworshak-config`, `dworshak-secret` (Low-level I/O).
    
- **Level 1:** `dworshak-prompt` (The "Smart" layer that imports Level 0).
    
- **Level 2:** Consuming Apps / `dworshak` CLI.
    

### 4. Reference: Advanced Timeout Handling

Other systems use this to prevent "zombie" interactive sessions:

- **Sudo:** `timestamp_timeout` in `/etc/sudoers` is a classic example of "interactive security" timing out for safety.
    
- **Expect (Tool):** The legendary `expect` scripting language relies entirely on the `timeout` variable to decide when an interactive session has failed to meet a condition.
    
- **SSH:** `ClientAliveInterval` handles the "is someone there?" check to prevent hanging open connections.
    

### 5. "Batteries-Included" Example

Here is how that `DworshakGet` class might look inside `dworshak-prompt`:

Python

```
from dworshak_config import DworshakConfig
from dworshak_secret import DworshakSecret

class DworshakGet:
    @staticmethod
    def value(key: str, is_secret: bool = False, timeout: int = 60):
        # 1. Attempt Retrieval
        store = DworshakSecret() if is_secret else DworshakConfig()
        val = store.get(key)
        
        if val is not None:
            return val

        # 2. Prompt with Timeout (the 'Batteries')
        try:
            from .prompt_logic import ask_user
            new_val = ask_user(key, secret=is_secret, timeout=timeout)
            
            # 3. Auto-Set if found
            if new_val:
                store.set(key, new_val)
            return new_val
            
        except TimeoutError:
            print(f"Error: Prompt for '{key}' timed out after {timeout}s.")
            raise
```

### Why this is the right move for Dworshak:

It honors your requirement that **a non-available credential should be reported as unknown rather than falling back to being prompted** _by default_, but provides a single, unified way to opt-in to "Get-or-Prompt" behavior across all your tools.

Should we define a standard "Dworshak Environment" variable (like `DW_TIMEOUT`) that sets a global default for these prompts, so you don't have to pass `timeout=60` every time?