---
{"dg-publish":true,"permalink":"/information-heap/deleting-buggy-versions-and-releases/","noteIcon":"","created":"2025-12-11T17:19:51.554-06:00"}
---

Date: [[-Daily Activity Log-/2025 12-December 11\|2025 12-December 11]]

```
when i have builds and versions that fail for various reasons, i update quickly 

"real men test in prod"

going back and removing versions from pypi and github releases is time consuming

is there an easier way to do this rather than clicking around the web app?

is leaving broken versions normal, as long as we assume the most recent version is good?

it is probably better to remove all buggy versions, so that remaining version differences represent stable feature changes, rather than 
```

That's a very common challenge when adopting a fast, continuous release cycle—the administrative overhead of cleaning up failed versions. Your philosophy of "real men test in prod" is certainly fast-paced, but the cost is the cleanup!

Let's break down your questions about managing broken versions on PyPI and GitHub Releases.

---

## 🛠️ Efficiently Removing Broken Versions

You are right, clicking around the web UI for both PyPI and GitHub is time-consuming.

### 1. Removing from PyPI (The Hard Part)

PyPI's policy is strict: **you cannot typically delete a specific version once it's been uploaded.**

- **Why?** This policy prevents the "supply chain attack" known as **dependency confusion**. If package A depends on version 1.1.0 of your package, and you delete 1.1.0, someone else could upload a malicious 1.1.0 and compromise all users of package A.
    
- The Best Alternative: YANKING
    
    Instead of deletion, you must Yank the release. A yanked release remains on PyPI but is hidden from search results and is not installed by default by modern package managers unless explicitly requested.
    

|**Tool**|**Command**|**Notes**|
|---|---|---|
|**`twine`**|`twine check`|Can be used to check local packages before upload.|
|**PyPI Web UI**|Click "Manage" on the release page.|Use this for a quick one-off.|
|**PyPI API/`twine`**|Currently, yanking specific files requires using the **PyPI web UI** or direct API calls, as `twine` does not yet have a direct CLI command for yanking versions.|The simplest path is often a well-structured script that uses the PyPI API.|

### 2. Removing from GitHub Releases

Unlike PyPI, GitHub gives you full control. Deleting a GitHub Release can be done efficiently via the API.

|**Method**|**Command/Tool**|**Notes**|
|---|---|---|
|**GitHub CLI**|`gh release delete <tag-name>`|**Recommended.** If you use the GitHub CLI locally or in a script, this is the quickest way to remove a release and its associated tag.|
|**GitHub REST API**|`DELETE /repos/{owner}/{repo}/releases/{release_id}`|Requires getting the `release_id` first, which is cumbersome for scripting unless you have a list.|
|**Shell Script**|Use `curl` to call the API endpoints (less clean than `gh`).||

### 💡 The Automated Solution: A Rollback Script

The most efficient way to handle frequent mistakes is to create a single shell or Python script that takes a version number as an argument and performs all the necessary cleanup steps for both platforms:

1. **Delete GitHub Release:** Uses the `gh release delete` command.
    
2. **Yank PyPI Release:** Uses the PyPI web UI (the unavoidable step) or a direct API script.
    
3. **Delete Local Tag:** `git tag -d <tag>`
    
4. **Delete Remote Tag:** `git push --delete origin <tag>`
    

---

## ⚖️ Is Leaving Broken Versions Normal?

This question reflects a trade-off between **safety/quality** and **speed/overhead**.

### 1. The Argument Against Leaving Broken Versions (Your View)

You are absolutely right: it is **better practice** to remove or clearly mark broken versions.

- **Trust and Confidence:** Every time a user tries to install an old version and finds it broken, it erodes trust.
    
- **Support Overhead:** Broken versions lead to useless bug reports.
    
- **Dependency Pinning:** If a user or downstream project pins a dependency to a broken version, they are permanently stuck unless they manually fix it.
    

### 2. The Practical Reality (The Industry Standard)

The reality is that once a version is published, most companies **do not delete or yank it**, especially if it's a minor or patch release. They rely on the next, **fixed version** to rapidly supersede it.

- **The Default Assumption:** Users (and package managers) are trained to assume that **the most recent version is the desired one.** If an issue is found in version 1.1.0, the developer immediately publishes 1.1.1, and the problem effectively vanishes for almost all new installs.
    
- **Yanking as a Last Resort:** Yanking is generally reserved for security issues, legal problems, or fundamentally broken builds (e.g., missing dependencies). For routine bugs, the 1.1.1 fix is usually sufficient.
    

**Conclusion:** Given your high-velocity workflow, the most practical and scalable approach is to:

1. **Yank** versions only if they are critically flawed (e.g., installer fails, security issue).
    
2. **Immediately release a fix** (e.g., 1.1.1) for normal bugs, letting the fix supersede the mistake.
    
3. Use the `gh release delete` command in a script to manage the GitHub side of cleanup easily.
    

Would you like me to help you draft a simple shell script using the GitHub CLI to delete your broken releases and tags?

---
```
will releasing github tags also delete the associated the github release?
```

That is a very important point regarding the relationship between GitHub Tags and Releases.

The short answer is: **No, deleting a tag does not automatically delete the corresponding release.**

Here is a breakdown of the two entities and how deletion works:

---

## 🏷️ Tags vs. Releases on GitHub

Tags and Releases are distinct objects, even though a Release must be tied to a Tag.

|**Feature**|**Git Tag (e.g., v1.0.0)**|**GitHub Release (e.g., "Release v1.0.0")**|
|---|---|---|
|**Location**|Stored in the underlying **Git repository** history.|Stored in the **GitHub database** as a separate object.|
|**Creation**|Created by `git tag <name>` and pushed by `git push --tags`.|Created via the web UI, API, or an Action (like `softprops/action-gh-release`).|
|**Contents**|Just a pointer to a specific commit.|Includes the Tag, the Title/Description (changelog), and **uploaded binary files/artifacts** (`.zip`, `.exe`, `.tar.gz`, etc.).|
|**Deletion Rule**|**Deleting the tag does NOT delete the release.**|**Deleting the release DOES delete the associated tag** (if you choose the default option on the web UI/API to also delete the tag).|

## 🛠️ Deletion Scenarios (The Command Line Approach)

Since you are looking for efficiency, here is how you manage them using the **GitHub CLI (`gh`)**—the recommended tool for this task:

### 1. Delete _Only_ the Tag

If you delete the tag, the **Release remains intact** but loses its anchor in the repository history.

- **Delete Local Tag:**
    
    Bash
    
    ```
    git tag -d v1.1.0
    ```
    
- **Delete Remote Tag:**
    
    Bash
    
    ```
    git push --delete origin v1.1.0
    ```
    

### 2. Delete the Release (and its Assets)

This is the cleanup step you need. This command removes the Release description, the binary files, and **prompts you to delete the associated tag** as well (it may delete the tag automatically, depending on the tool version and settings).

- **Delete GitHub Release:**
    
    Bash
    
    ```
    gh release delete v1.1.0
    ```
    
    This command is the most efficient way to clean up the visible release, including the artifacts you uploaded. It will also offer to clean up the associated Git tag.
    

### Conclusion

To efficiently wipe a broken version from GitHub (both the tag pointer and the web presence with the files), you should focus on deleting the **Release** first, as it's the container for all the heavy assets.

I would strongly recommend creating a single cleanup script that calls `gh release delete <tag>` and then ensures the remote tag is removed with `git push --delete origin <tag>`.