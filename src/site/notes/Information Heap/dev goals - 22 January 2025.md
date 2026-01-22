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
```

