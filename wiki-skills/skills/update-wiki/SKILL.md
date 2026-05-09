---
name: update-wiki
description: Synthesize source extracts into wiki pages — create new pages, update existing ones, add cross-links, and maintain the wiki index. The second step after /extract.
---

# Update Wiki

You are helping the user synthesize their source extracts into structured, interlinked wiki pages. Source extracts (produced by /extract) capture what the user understood. Your job is to weave that understanding into the growing knowledge graph.

**Use this skill when:**
- User says "update wiki", "ingest into wiki", "synthesize this"
- User has just completed an /extract and wants to integrate it
- User wants to reorganize or cross-link existing wiki pages

## Prerequisites

- Must be run from a topic directory that has CLAUDE.md at the root
- Read CLAUDE.md to understand topic context, wiki categories, and template type
- Source extracts should exist in wiki/sources/ (produced by /extract)

## LLM Decision Logic

```
MAIN FLOW:

1. READ CLAUDE.md to understand:
   - Wiki categories and their purpose
   - Template type (determines structure expectations)
   - Topic-specific guidance

2. IDENTIFY what to ingest:
   IF user points to specific source: read that file
   IF user says "ingest recent": find sources not yet reflected in wiki
     (check refs/raw-index.md — rows with "(pending /update-wiki)" in Wiki pages column)

3. READ the source extract(s)

4. SCAN existing wiki pages:
   - List all pages in wiki/wiki/
   - Read pages that seem related (based on title, connections listed in source)
   - Identify where new content fits

5. PROPOSE changes to user:
   - New pages to create (with draft content)
   - Existing pages to update (show what sections you'd add/modify)
   - New cross-links to add between pages
   - Which category each new page belongs to

6. AFTER user approval:
   - Write new pages / update existing pages
   - Update refs/raw-index.md (fill in "Wiki pages" column)
   - Update index.md (add new pages to catalog)
   - Append to log.md (record what was ingested and changed)

7. REPORT summary of changes
```

## Wiki Page Format

```markdown
# [Concept Name]

[Core explanation — what this is and why it matters. Written clearly enough that
the user could explain it to a colleague from this page alone.]

## How it works
[Technical details at appropriate depth. Use diagrams (mermaid) if helpful.]

## Key points
- [Important details, gotchas, nuances]

## Connections
- [[related-concept]] — [one line explaining the relationship]
- [[another-concept]] — [how they interact]

## Sources
- [[sources/papers/paper-name]] — [what this source contributed to this page]
- [[sources/courses/chapter-name]] — [what was learned]
- [raw/code/project/file.py](https://github.com/user/repo/blob/main/code/project/file.py) — [implementation reference]
```

## Linking Rules

1. **Internal wiki links**: Use `[[wikilinks]]` — Obsidian-compatible
   - Same directory: `[[page-name]]`
   - Cross-directory: `[[category/page-name]]`
   - With display text: `[[category/page-name|Display Text]]`

2. **Links to sources**: Use `[[sources/type/name]]`

3. **Links to raw repo files**: Use markdown links with git web URL
   - `[filename](https://github.com/user/repo/blob/main/path/to/file)`
   - Construct from base URL in CLAUDE.md

4. **Every page MUST have**:
   - At least one `[[wikilink]]` to another wiki page in ## Connections
   - At least one link in ## Sources

5. **When updating existing pages**:
   - Add new bullet to ## Sources for the new source
   - Add cross-links if the new content reveals connections
   - Don't overwrite existing content — append, refine, or add sections

## Updating refs/raw-index.md

After creating/updating wiki pages, fill in the "Wiki pages" column for the relevant source:

Before:
```
| dnn-tabular.pdf | [link](url) | [[sources/papers/dnn-tabular]] | (pending /update-wiki) |
```

After:
```
| dnn-tabular.pdf | [link](url) | [[sources/papers/dnn-tabular]] | [[wiki/tabular-ml]], [[wiki/feature-engineering]] |
```

## Updating index.md

Add new pages to the appropriate category section:

```markdown
## Categories

### Theory
- [[wiki/theory/page-name]] — one-line description

### Coding Patterns
- [[wiki/coding-patterns/page-name]] — one-line description
```

Update the statistics at the bottom.

## Updating log.md

Append a row:

```markdown
| YYYY-MM-DD | Ingested source | [[sources/type/name]] | [[wiki/page1]], [[wiki/page2]] |
```

## Template-specific behavior

### For `interview-prep` topics:
- Each concept page MUST be placed in exactly one category (theory/, system-design-concepts/, tech/, algorithms/, or coding-patterns/)
- Pattern pages reference specific problems in raw/ via git web links
- Include a "## Interview angle" section: how would you explain this in 30 seconds?

### For `cs-learning` topics:
- Pages can live directly in wiki/ or in subcategories
- Create a new subcategory directory when 5+ pages cluster around a theme
- Focus on building mental models, not just definitions

## Handling conflicts

- **New content contradicts existing page**: Add a "## Nuances" or "## Debates" section rather than deleting. Note which source says what.
- **Content spans multiple categories**: Put the page where it PRIMARILY belongs, cross-link from others.
- **Page is getting too long** (>200 lines): Propose splitting into sub-pages with a hub page linking them.

## Batch mode

If user says "ingest all pending sources" or similar:
1. Find all rows in refs/raw-index.md with "(pending /update-wiki)"
2. Read each corresponding source
3. Propose all changes as a batch
4. Apply after approval
5. Update all tracking files in one pass
