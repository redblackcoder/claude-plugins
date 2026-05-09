# wiki-skills

A Claude Code plugin for building and maintaining [LLM Wikis](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f#file-llm-wiki-md) — structured, interlinked knowledge bases where an LLM agent incrementally synthesizes your learning into a persistent wiki.

## Concept

Instead of bookmarking hundreds of articles and papers you'll never revisit, an LLM Wiki works in three layers:

1. **Raw sources** — PDFs, code, docs, annotations (immutable, git-tracked)
2. **Source extracts** — your processed understanding of raw content, produced iteratively with the LLM
3. **Wiki pages** — synthesized, cross-linked knowledge maintained by the LLM

Each topic is a pair of git repos (raw + wiki), with the wiki doubling as an Obsidian vault for local browsing and graph exploration.

## Skills

### `/wiki-skills:bootstrap-wiki`

Create a new LLM Wiki topic with full scaffolding — git repos, Obsidian vault, CLAUDE.md, and directory structure.

**Built-in templates:**
- `interview-prep` — organized by theory, system-design-concepts, tech, algorithms, and coding-patterns
- `cs-learning` — organic structure for learning any CS topic (MLOps, distributed systems, etc.)
- `custom` — describe your own categories interactively

**Creates:**
```
topic-name/
├── CLAUDE.md              # Topic context (categories, git URLs, guidance)
├── raw/                   # Git repo: papers, code, docs
└── wiki/                  # Git repo + Obsidian vault
    ├── sources/           # Processed extracts
    ├── wiki/              # Synthesized knowledge pages
    ├── refs/              # Links back to raw content
    ├── index.md           # Page catalog
    └── log.md             # Ingest history
```

### `/wiki-skills:extract`

Iteratively produce source extracts from raw content with human-in-the-loop understanding. This is where learning happens.

**Accepts:**
- PDFs directly from `raw/papers/`
- Code files or directories from `raw/code/`
- Pasted Zotero annotations
- Saved articles from `raw/docs/`

**The iteration loop:**
1. Claude reads the raw content and drafts an extract
2. Claude asks: "What was most surprising? How does this connect to what you know? What's unclear?"
3. You refine your understanding through dialogue
4. Final extract captures *your* mental model, not just an AI summary
5. Extract is saved to `wiki/sources/` with git web links back to raw content

### `/wiki-skills:update-wiki`

Synthesize source extracts into wiki pages — create new pages, update existing ones, add cross-links, and maintain the index.

**What it does:**
- Reads source extracts and existing wiki pages
- Proposes new pages or updates to existing ones
- Adds `[[wikilinks]]` for Obsidian graph view compatibility
- Maintains `refs/raw-index.md`, `index.md`, and `log.md`
- Links wiki pages back to source extracts and raw files via git web URLs

## Workflow

```
1. /wiki-skills:bootstrap-wiki     → set up a new topic
2. Drop raw content into raw/      → papers, code, articles
3. /wiki-skills:extract            → understand it deeply (iterate)
4. /wiki-skills:update-wiki        → weave into the knowledge graph
5. Open wiki/ in Obsidian          → browse, explore connections
```

## Integration with Zotero

Papers are stored in `raw/papers/` (git-tracked). Open them in Zotero via linked-file attachment mode for reading and annotation. Export annotations, then use `/wiki-skills:extract` to process them into source extracts.

## References

- [LLM Wiki concept by Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f#file-llm-wiki-md)
