---
{"dg-publish":true,"permalink":"/dev/mulch/mulch-refactor-hell-aug-7/","noteIcon":"","created":"2025-08-07T14:07:16.670-05:00"}
---

Date: [[-Daily Activity Log-/2025 08-August 07\|2025 08-August 07]]

# CB 0
mulch init is expected to only be run once. If it is run again, there is a manager.lock file to protect against erroneous overrides. It also provides metadata and claritiy about the state of the mulch.toml scaffold when it was created. This can be extended in the future to help with updates and changes. The --stealth flag matters where the workspace manager exists relative to the workspaces in the /workspaces/ dir, which are each generated using the mulch workspace command. If this flag is used, the relative path needs to be clear. 

mulch workspace is expected to be run multiple times. the --here flag controls whether or not a workspace is placed into a /workspaces/ dir or at the root. when the command is run again, consistency matters, and so the automatic behavior unless forced should assume that the rules should be followed the same as the first instance.  There is a space.lock file generated for each workspace. If the workspace is changed or tried to override, if provides protection. It also provides metadata and claritiy about the state of the mulch.toml scaffold when it was created.

Also, the mulch.toml defined scaffold  is currently not interpretted or implemented correctly, i think the problem is with the check_and_create_workspace_dirs_from_scaffold() function in the WorkspaceInstanceFatory class.

make sense?

