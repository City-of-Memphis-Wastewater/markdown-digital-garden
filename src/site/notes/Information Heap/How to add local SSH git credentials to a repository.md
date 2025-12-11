---
{"dg-publish":true,"permalink":"/information-heap/how-to-add-local-ssh-git-credentials-to-a-repository/","noteIcon":"","created":"2025-12-09T19:03:29.955-06:00"}
---

Date: [[Information Heap/2025 12-December 09\|2025 12-December 09]]

```
git remote set-url origin git@github.com:City-of-Memphis-Wastewater/repo.git
```

When Git sees a remote URL starting with `git@...`, it completely bypasses the username/password (or PAT) prompt and instead delegates the authentication task to your system's built-in **SSH client** .