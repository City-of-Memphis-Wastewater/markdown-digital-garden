---
{"dg-publish":true,"permalink":"/daily-activity-log/2025-09-september-18/","noteIcon":"","created":"2025-09-18T13:02:07.119-05:00"}
---

Date: [[-Daily Activity Log-/2025 09-September 18\|2025 09-September 18]]

[[Information Heap/Tioga Monthly Meeting - Sept 18 2025\|Tioga Monthly Meeting - Sept 18 2025]]

[[Software/Configuration Woes\|Configuration Woes]]

https://pypi.org/project/pipeline-eds
- Pipeline is in version 0.2.61, happy birthday Pipeline.
- Configuration accesible via the CLI is now handled via keyring for passwords and via a comparable flat single /.pipeline-eds/config.json file for things like plant names, ZD values, and URL.
- Key problem - Termux does not launch temp files on its own. I had to dial back to serving the HTML with a lite server to be control+C cancelled and for the user to over the URL manually. Lame. 0.2.53-0.2.60 was glorious battle in `src/pipeline/gui_plotly_static.py` to leverage `termux-open` or even `am start -a android.intent.action.VIEW -d file_uri -n com.android.chrome/com.google.android.apps.chrome.Main`. But alas, no bacon.
- [[Information Heap/pipeline 0.2.61 Video Analysis\|pipeline 0.2.61 Video Analysis]]
- [[Information Heap/pipeline 0.2.61 Video Analysis props\|pipeline 0.2.61 Video Analysis props]]
- [[Information Heap/Security analysis and suggestions\|Security analysis and suggestions]]
