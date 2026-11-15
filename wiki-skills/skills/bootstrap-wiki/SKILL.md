---
name: bootstrap-wiki
description: Set up or extend a single-repository Obsidian wiki for interview preparation, CS learning, or a custom topic.
---

# Bootstrap a topic

1. Resolve the requested topic root and profile (interview-prep, cs-learning or custom). Read existing README and CLAUDE.md. Treat a checkout containing wiki/README.md and sources/ as an existing vault. Preserve its pages and conventions. If a legacy raw/wiki pair is present, explain the layout difference and propose migration before relocating any material.
2. Use one repository rooted at the topic. Initialize Git only when creating a new repo. Configure a remote or publish only when requested.
3. Create missing navigation with ordinary relative Markdown links. The root README opens wiki/README.md. The study home links wiki/concepts.md and wiki/reading-notes/README.md. For interview-prep, also link wiki/problems/README.md with categories coding, ai-assisted-coding and system-design. For CS learning or custom topics use the subject categories appropriate to the user.
4. Create sources/README.md explaining where artifacts go: code/ for projects, whiteboards/ for diagrams, notes/coding/ and notes/designs/ for sanitized exercise notes, references/ for publishable readings. Create category directories as pages or artifacts arrive. Open the topic root as the vault.
5. Record the topic profile, paths and any user-specific authoring conventions in CLAUDE.md, preserving existing instructions. On repeat runs add only missing scaffolding. Check each created link from the Markdown file that contains it.
