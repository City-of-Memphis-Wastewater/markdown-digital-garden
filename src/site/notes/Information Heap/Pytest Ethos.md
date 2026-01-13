---
{"dg-publish":true,"permalink":"/information-heap/pytest-ethos/","noteIcon":"","created":"2026-01-08T14:24:10.294-06:00"}
---


Date: [[2026 01-January 08\|2026 01-January 08]]

```
where do tests run? mostly in ci on a remote server or mostly locally?

should every function be included in tests? how do you not over design tests and slow down development
```

Testing is a balancing act. If you test everything, you never ship; if you test nothing, you ship broken code.

---

### Where do tests run?

In a professional workflow, they run in **both** places, but they serve different purposes:

- **Locally (The "Inner Loop"):** You run tests while coding to get immediate feedback. These should be fast (seconds, not minutes). You usually run a specific subset of tests related to the feature you are touching.
    
- **CI/Remote (The "Gatekeeper"):** Tests run automatically when you push to GitHub or open a Pull Request.1 This is the **Source of Truth**. It tests against multiple Python versions (`3.9`, `3.13`) and OSs (`Ubuntu`, `Windows`) that you don't have on your own machine.
    

![Image of continuous integration workflow diagram](https://encrypted-tbn3.gstatic.com/licensed-image?q=tbn:ANd9GcRCAtqhZTmeVYb6hnhDqKdOuRaR-tJ5j0gHxChQYjugMsKVmO5UmWsR0T8maj5YQhT9x2Xr1RXjHR8A4Kp-txy_iw_W8vRCbTxuDbPMW6p9ZBaqN6Q)


---

### Should every function be included?

**No.** Striving for "100% Code Coverage" is often a trap that leads to brittle tests.2

- **Test the "Public Interface":** Focus on the functions your users actually call (e.g., `get_secret`). If `get_secret` works, it implies that the internal helper functions it uses are also working.
    
- **Test "Edge Cases":** Don't just test when things go right. Test when the database is missing, when the password is empty, or when the CLI isn't installed.
    
- **Skip "Trivial" Code:** You don't need to test a function that simply returns a constant or a standard library call that is already well-tested by the Python core team.
    

---

### How to avoid over-designing (and slowing down)

To keep development fast, follow these three rules:

#### 1. The "Pyramid" Strategy

Write many small, fast **Unit Tests** (testing one function in isolation) and only a few heavy **Integration Tests** (testing the CLI, the DB, and the network all at once).

#### 2. Mock the "Expensive" stuff

Since your new design calls the `dworshak` CLI, a real test would require installing that CLI and setting up a real vault every time. That is slow and "heavy."

- **The solution:** "Mock" the subprocess call. Tell the test: _"When the code calls the CLI, just pretend it returned 'password123' instead of actually running the command."_ This makes tests run in milliseconds.
    

#### 3. Test for Value, not Volume

Ask yourself: _"If I change this code later, will this test catch a real bug, or will it just break because the test is too specific?"_

- **Bad Test:** Checking that a specific log message was printed.
    
- **Good Test:** Checking that the function returns the correct string from the vault.
    

---

### A "Low-Overhead" Next Step

Since `dworshak-access` is so lightweight, I recommend starting with **three tests** in a single file:

1. **Success:** Call `get_secret` and get a string back (Mocked).
    
2. **Missing CLI:** Verify a `RuntimeError` happens if `dworshak` isn't installed.
    
3. **Bad Item:** Verify a `KeyError` happens if the CLI returns a non-zero exit code.
    

**Would you like me to write the code for those three specific tests using `pytest` and `unittest.mock`?**