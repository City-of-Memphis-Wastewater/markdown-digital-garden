---
{"dg-publish":true,"permalink":"/software/power-shell/","noteIcon":"","created":"2025-07-07T14:23:47.753-05:00"}
---

Date: 14-02-2025

PowerShell and PowerShell ISE

Interesting commands:
```
$script = Get-Content .\test.ps1
Invoke-Expression $script
```
```
$policy = "Set-ExecutionPolicy -ExecutionPolicy Undefined -Scope Process"
# Print the variable
Write-Output $policy
```
