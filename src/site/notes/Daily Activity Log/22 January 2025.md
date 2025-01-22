---
dg-publish: true
dg-home: false
permalink:	/daily-activity-log/22-january-2025/
noteIcon:	
created: 2025-01-22T07:56:09.490-06:00
---
Work from home today


- [x] Order #coveralls, to enhance my ability and willingness to go to #thefield
- [x] Set up Edge browser for remote access
- [x] Access and edit Obsidian notes remotely, via OneDrive
~~- [ ] Toggle Publish, to sync to *Maxson-Engineering-Notes*~~
- [x] Communicate with Matthew East, [[People/Peter Gabor\|Peter Gabor]], et al, about Teams meeting for (Th 23 Jan) or (F 24 Jan)
- [x] Try to edit [[-/~Welcome to Maxson~\|~Welcome to Maxson~]] by uploading this new file direcly to GitHub, via Edge browser login.
- [ ] Consider using a GitHub branch for manual browser editng, rather than commiting directly to Main.
- [x] Discover Digital Garden syntax for propertly editing raw MD to propagate links %%example syntax failure%%
- [x] Take down old [[Software/Vercel\|Vercel]] deployment at outdated URL
- [ ] Other tasks --> See local offshore notes: BioRem Logs Past v. Future, 8 am meeting with IT, Stantec, and Emerson 
- [ ] Tomorrow (Thursday 23 January), emigrate notes in offshore Obsidian Valut to immigrate to my office Valut on COM******.  

If i can put the app data for Obsidian on my work computer in a OneDrive location (my Files/Apps), then i should be able to sync my Obsidian settings across a remote instance.
Really, I just need to be able to run "Publish" on the Digital Garden plugin in my Obsidian instance at work

This should be doable by sycing the Obsidian instance settings, and then by adding the github fine grained token to the local instance digial garden settings. Even if the instances arent synced, i wonder if it would still work, with the same token, and choosing to only add and not delete the delta.

We're getting a weird phenomenon where this new 22 Jan daily note does upoad and propogate, but it reroutes to the home page rather than showing this text content.

Okay, we're all the way in. The answer is to edit the header/metadata/properties #properties in the handmade-markdown in github to include these #permalink and datetime values:
permalink:	/daily-activity-log/22-january-2025/ ~~*(example)*~~
noteIcon:	
created: 2025-01-22T07:56:09.490-06:00 ~~*(example)*~~
I think only the permalink is truly necessary. Otherwise the note will manifest as the homepage. 
Update: Actually, use brackets: {"dg-publish":true,"permalink":"/daily-activity-log/17-january-2025/","noteIcon":"","created":"2025-01-17T06:59:51.322-06:00"}
Low priority, but cool: Find some awesome note icons

I'm curious to see what happens back at base, especially when the github-edited file differs from the OneDrive file.
