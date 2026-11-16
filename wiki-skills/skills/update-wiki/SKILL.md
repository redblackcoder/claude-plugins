---
name: update-wiki
description: Add or revise practice problems, reading notes and concepts from prompts, implementations, boards, articles or prior discussion.
---

# Update the wiki

Read CLAUDE.md, wiki/README.md and the affected catalog. Accept inputs directly. Search existing pages by topic, source and aliases before choosing a stable path. Add later attempts and new evidence to the same page when they address the same problem.

## Evidence

Keep a saved project, its build files and tests together under sources/code/. Put anonymized boards in sources/whiteboards/, coding notes in sources/notes/coding/ and design notes in sources/notes/designs/. Reuse matching artifacts. Before importing private material, remove employer and client details, names, private paths, identifiers and credentials from text, figures, attachments and metadata. Keep useful architecture and geometry. Do not import private history. Cite external publications directly or keep publishable reference material under sources/references/.

## Choose the deliverable

- **Coding exercise:** write wiki/problems/coding/<slug>.md, or use ai-assisted-coding for an explicitly assisted exercise. Give the complete prompt, inputs, outputs and constraints. Walk through an example before describing the recorded algorithm, state transitions, edge cases and complexity where supported. Link the implementation, tests or session notes. A prompt alone can be saved with “Solution pending”; preserve partial work and known bugs. Never fill a missing implementation with generated code and present it as the user's.
- **System design:** write wiki/problems/system-design/<slug>.md. Explain the workload, actors, requirements and scope. Trace one request or failure. Describe the user's architecture, key decision and unresolved point. Embed a local board when available and link the original notes. Label interviewer feedback and later study separately.
- **Reading or deep dive:** cite the article and place a narrow insight on the relevant concept page. Use wiki/reading-notes/ for a substantial standalone synthesis. Link it from relevant problems or concepts and credit the user's own conclusions only when supplied.

For cs-learning and custom profiles, read the topic's categories and use their equivalents. For batches, inventory the supplied inputs, group duplicates and reconcile each with its existing page or new destination. Do not require an intermediate extract or an entry in a raw index.

## Close the update

Use relative Markdown links from each page to local artifacts, diagrams and related pages. Update the affected problems, concepts or reading-notes catalog once per new page. Respect an existing ingest log if the topic uses it. Verify all changed links, embeds and catalog entries from the topic root. On repeat ingestion preserve existing writing and add only newly supported material. Report what changed and any missing evidence.
