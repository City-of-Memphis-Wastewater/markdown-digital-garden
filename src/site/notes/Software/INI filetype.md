---
{"dg-publish":true,"permalink":"/software/ini-filetype/","noteIcon":"","created":"2025-01-10T13:06:24.749-06:00"}
---

.ini
#software 
#filetype
Plant text configuration files that store software application settings in a structured format
Instances of INI files can be found on [[COM184025\|COM184025]] in C:\Users\george.bennett\ [^userdirectory] at 

A INI file is generated whenever I open [[Software/CADAssistant\|CADAssistant]], in multiple locations[^AppData-Local][^AppData-Roaming]

The INI file in AppData\Roaming\ was generated at. It's context are as follows:
The only heading is General, which defines a #lastRunnedVersion, a #backgroundMode value, and a value called #toLockOrbitZUp. These value keys are not used in the INI file in the AppData\Local directory, shown below.
![Pasted image 20250110142038.png](/img/user/Pasted%20image%2020250110142038.png)

For the INI file in AppData\Local\ has contents as follows:
The headings are:
- General'
- network
- filters
- recent
![Pasted image 20250110142259.png](/img/user/Pasted%20image%2020250110142259.png)

The Local config file was generated 30 minutes after the Roaming config file


[^AppData-Local]: [CADAssistant Local Cache ](C:\Users\george.bennett\AppData\Local\OpenCASCADE\CADAssistant\cache\qmlcache)
[^AppData-Roaming]: [CADAssistant Roaming Cache ](C:\Users\george.bennett\AppData\Roaming\OpenCASCADE)
[^userdirectory]: [User directory](C:\Users\george.bennett\\) 