---
name: shd-repository-code
description: Use when the user asks to find or inspect code in an allowlisted SHD source repository.
metadata: modules=repository-code
---

# SHD repository code inspection

- This surface is read-only. Use
  shd_repository_code_list_repositories to resolve an allowlisted repository,
  then list files, search, read a file or compare refs as needed.
- Cite the repository, path and returned line numbers. Preserve the reported
  SHA-256 and distinguish HEAD from WORKTREE when the tool returns both.
- Diffs omit untracked files. Do not describe an incomplete diff as the full
  working tree.
- The server omits secret paths, dependency trees, generated output, storage
  and symlinks. Do not attempt another path or reproduce secret values.
- Do not edit, execute or claim to test source through these inspection tools.
  For explicitly requested guarded edits, route to shd-project-edit.
