---
name: bootstrap-wiki
description: Create a new LLM Wiki topic with full scaffolding — git repos, Obsidian vault, CLAUDE.md, and wiki structure based on a chosen template.
---

# Bootstrap Wiki

You are helping the user create a new LLM Wiki topic. An LLM Wiki is a structured knowledge management system where the user stores raw learning materials (papers, code, docs) in one git repo, and an LLM-maintained wiki in a separate git repo (which doubles as an Obsidian vault).

**Use this skill when:**
- User wants to start a new learning topic/wiki
- User says "bootstrap wiki", "create wiki", "new topic", "set up a wiki for X"

## LLM Decision Logic

```
MAIN FLOW:

1. ASK user:
   - Topic name (e.g., "mlops", "data-systems", "interview-prep")
   - One-line description of what this topic is about
   - Which template to use (show options below)
   - GitHub base URLs for raw and wiki repos (can be set later)
   - Parent directory (default: ~/learning/)

2. BASED ON template choice:
   IF "interview-prep" → use Interview Prep template
   IF "cs-learning" → use CS Learning template
   IF "custom" → ask user to describe their wiki categories

3. CREATE directory structure
4. INITIALIZE git repos
5. WRITE CLAUDE.md at topic level
6. WRITE initial wiki files (index.md, log.md, refs/raw-index.md)
7. SET UP Obsidian vault config in wiki/
8. REPORT what was created
```

## Templates

### Template: `interview-prep`

For interview preparation. Wiki pages are patterns/concepts only — individual problems live in raw/code/ and are linked back via refs and git web URLs.

```
[topic]/
├── CLAUDE.md
├── raw/                          # Git repo
│   ├── code/                     # Solved problems, implementations
│   ├── papers/                   # PDFs (textbooks, research)
│   └── docs/                     # Design docs, notes, saved articles
└── wiki/                         # Git repo + Obsidian vault
    ├── .obsidian/
    ├── index.md
    ├── log.md
    ├── sources/
    │   ├── papers/
    │   ├── problems/             # Extracts from solved problems
    │   └── articles/
    ├── wiki/
    │   ├── theory/               # CS fundamentals (threads, SIMD, mutex, ACID, etc.)
    │   ├── system-design-concepts/  # Concepts distilled from design problems
    │   ├── tech/                 # Technologies (AWS, k8s, OTEL, OAuth, etc.)
    │   ├── algorithms/           # CS algorithms
    │   └── coding-patterns/      # Patterns extracted from coding problems
    └── refs/
        └── raw-index.md
```

### Template: `cs-learning`

For learning a computer science topic (MLOps, Data Systems, distributed systems, etc.). Wiki categories emerge organically as content is ingested.

```
[topic]/
├── CLAUDE.md
├── raw/                          # Git repo
│   ├── code/                     # Projects, exercises
│   ├── papers/                   # PDFs
│   └── docs/                     # Articles, course materials
└── wiki/                         # Git repo + Obsidian vault
    ├── .obsidian/
    ├── index.md
    ├── log.md
    ├── sources/
    │   ├── papers/
    │   ├── courses/
    │   └── articles/
    ├── wiki/                     # Pages created organically during Update Wiki
    └── refs/
        └── raw-index.md
```

### Template: `custom`

Ask the user to describe:
- What top-level categories should exist in wiki/wiki/
- What types of raw content they'll have
- What types of sources they'll produce

Then generate the structure accordingly.

## File Contents to Generate

### CLAUDE.md (at topic level)

```markdown
# [Topic Name] Wiki

## Purpose
[User's one-line description]

## Template
[template name]

## Git URLs
- Raw repo: [URL or "not yet hosted"]
- Wiki repo: [URL or "not yet hosted"]

## Wiki categories
[List categories with brief description of what goes in each]

## Raw content overview
[Describe what kind of content lives in raw/ — based on template]

## Topic-specific guidance
[For interview-prep: "Pattern pages link back to specific problems in raw/ via git web URLs. Never duplicate problem solutions in wiki — extract the pattern only."
For cs-learning: "Categories emerge organically. Create new subdirectories in wiki/wiki/ when a cluster of related pages forms."]

## Conventions
- Wiki pages use [[wikilinks]] for internal cross-references (Obsidian compatible)
- External links to raw repo use [text](git-web-url) format
- Every wiki page must have a ## Connections section with at least one link
- Every wiki page must have a ## Sources section linking to its source extracts
- refs/raw-index.md is auto-maintained by the Extract skill — do not manually edit
```

### index.md

```markdown
# [Topic Name] — Index

## Categories

[List categories as links to subdirectories]

## Recent Pages

[Empty — filled as pages are created]

## Statistics
- Total wiki pages: 0
- Total sources: 0
- Last updated: [date]
```

### log.md

```markdown
# [Topic Name] — Log

Chronological record of ingests and wiki updates.

| Date | Action | Source | Wiki pages affected |
|---|---|---|---|
| [today] | Wiki bootstrapped | — | — |
```

### refs/raw-index.md

```markdown
# Raw Content Index

Links raw files to their source extracts and wiki pages.

## Papers (../raw/papers/)

| File | Git link | Extracted to | Wiki pages |
|---|---|---|---|
| (none yet) | | | |

## Code (../raw/code/)

| File/Project | Git link | Extracted to | Wiki pages |
|---|---|---|---|
| (none yet) | | | |

## Docs (../raw/docs/)

| File | Git link | Extracted to | Wiki pages |
|---|---|---|---|
| (none yet) | | | |
```

### raw/README.md

```markdown
# [Topic Name] — Raw Materials

[User's one-line description]

Source materials for learning. Processed into wiki pages via `/extract` → `/update-wiki`.

## Contents

### papers/
[Description based on template — e.g., "PDFs of research papers, textbooks, course slides"]

### code/
[Description based on template — e.g., "Solved problems, implementations, exercises"]

### docs/
[Description based on template — e.g., "Saved articles, design docs, notes"]

## Stats

- Total files: 0
- Last updated: [date]
```

### wiki/README.md

```markdown
# [Topic Name] — Wiki

[User's one-line description]

Synthesized knowledge pages maintained by LLM. Also an Obsidian vault.

## Categories

[List wiki categories from template with brief descriptions. Initially empty — filled as pages are created.]

## How this works

1. Raw materials go into the sibling `raw/` repo
2. `/extract` produces source extracts in `sources/`
3. `/update-wiki` synthesizes extracts into wiki pages in `wiki/`

## Stats

- Wiki pages: 0
- Sources processed: 0
- Last updated: [date]
```

**Keeping READMEs up to date:** Both READMEs should be refreshed when content is added. The `/extract` skill updates `raw/README.md` stats after processing new materials. The `/update-wiki` skill updates `wiki/README.md` categories and stats after creating pages.

### .obsidian/ setup

Create minimal Obsidian vault config so it opens cleanly:

**.obsidian/app.json:**
```json
{
  "useMarkdownLinks": false,
  "newLinkFormat": "relative"
}
```

**.obsidian/appearance.json:**
```json
{
  "baseFontSize": 16
}
```

## Execution

After creating everything:
1. Write raw/README.md and wiki/README.md (using templates above, populated with topic details)
2. Initialize git in raw/: `git init`, create .gitignore (ignore .DS_Store), initial commit (includes README.md)
3. Initialize git in wiki/: `git init`, create .gitignore (ignore .DS_Store, .obsidian/workspace.json), initial commit (includes README.md)
4. Print summary of what was created
5. Remind user to:
   - Open wiki/ folder in Obsidian as a vault
   - Set up GitHub remotes when ready (`git remote add origin [url]`)
   - Use `/extract` to start processing raw content
