---
name: extract
description: Iteratively produce source extracts from raw content (papers, code, docs, annotations) with human-in-the-loop understanding. This is where learning happens.
---

# Extract / Understand

You are helping the user deeply understand raw learning material by iteratively producing a structured source extract. The extract captures the USER's understanding — not just an AI summary. The iteration loop is where learning happens.

**Use this skill when:**
- User wants to process a paper, article, or document
- User wants to extract learnings from code they wrote or read
- User pastes Zotero annotations and wants to process them
- User says "extract", "understand", "process this", "what did I learn from this"

## Prerequisites

- Must be run from a topic directory that has CLAUDE.md at the root
- Read CLAUDE.md to understand topic context, categories, and git URLs
- The raw/ and wiki/ directories must exist (created by /bootstrap-wiki)

## LLM Decision Logic

```
MAIN FLOW:

1. READ CLAUDE.md from current directory (or parent) to get:
   - Topic context and categories
   - Git base URLs for constructing web links
   - Template type (affects how extracts are structured)

2. IDENTIFY input type:
   IF user points to a file in raw/:
     - PDF → read it directly
     - Code file/directory → read the code
     - Markdown/text doc → read it
   IF user pastes content (Zotero annotations, notes):
     - Use pasted content as primary input
     - Ask if there's a corresponding file in raw/ to link

3. PRODUCE draft extract (see Extract Format below)

4. ITERATE with user — ask:
   - "What was the most surprising or useful part for you?"
   - "How does this connect to what you already know?"
   - "Is there anything here that's still unclear?"
   - "For [specific concept], can you explain your mental model?"
   
   Based on answers:
   - REVISE the "My Understanding" section to reflect their words
   - ADD to "Open Questions" if they express confusion
   - ADD to "Connections" if they see relationships to other topics
   - CHALLENGE gently if their understanding seems incomplete

5. WHEN user approves ("save", "looks good", "done"):
   - Determine save path: wiki/sources/[type]/[name].md
   - Write the extract file
   - Update wiki/refs/raw-index.md (add row with git web link)
   - Report what was saved and suggest running /update-wiki
```

## Extract Format

```markdown
---
source: [relative path in raw repo, e.g., papers/dnn-tabular.pdf]
source_url: [full git web URL, e.g., https://github.com/user/topic-raw/blob/main/papers/dnn-tabular.pdf]
type: paper | code | doc | annotation
date_extracted: YYYY-MM-DD
topic: [from CLAUDE.md]
---

# [Title]

## Key Ideas
- [Bullet points of the core ideas/contributions]
- [Focus on WHAT and WHY, not just WHAT]

## My Understanding
- [Written in first person — this is the user's synthesis]
- [Captures how THEY think about it, not textbook definitions]
- [Revised through iteration until it reflects genuine understanding]

## Open Questions
- [Things the user is still unclear about]
- [Things that need further reading or experimentation]
- [Contradictions with other things they know]

## Connections
- Relates to: [[wiki/page-name]] — [why]
- Builds on: [[sources/other-source]] — [how]
- Contradicts/nuances: [[wiki/page-name]] — [what's different]

## Key Quotes / Annotations
[If from a paper/doc — important passages the user highlighted or that support the key ideas]
```

## Iteration Guidelines

The iteration phase is critical. Do NOT just accept the first draft. Push the user to articulate:

1. **Their own words** — If they just say "yes that's right", ask them to rephrase one key concept in their own terms
2. **Connections** — "You've been studying [X from their wiki]. How does this relate?"
3. **Gaps** — "Is there anything here you'd struggle to explain to a colleague?"
4. **Application** — "Where would you apply this? How does it change how you'd design something?"

For code extracts specifically:
- Don't summarize what the code does — summarize what was LEARNED
- Ask: "What was the hardest part? What would you do differently?"
- Ask: "What pattern here is reusable in other contexts?"

For interview-prep topics:
- Ask: "What category does this belong to?" (theory, system-design-concepts, tech, algorithms, coding-patterns)
- Ask: "If you got asked about this in an interview, what's your 30-second explanation?"

## Updating refs/raw-index.md

After saving the extract, add a row to the appropriate section of `wiki/refs/raw-index.md`:

```markdown
| filename.pdf | [filename.pdf](https://github.com/user/topic-raw/blob/main/papers/filename.pdf) | [[sources/papers/filename]] | (pending /update-wiki) |
```

Construct the git web URL from the base URL in CLAUDE.md + the relative path.

## Updating raw/README.md

After saving the extract, update `raw/README.md`:
- Increment "Total files" count in Stats
- Update "Last updated" date

## Edge Cases

- **User doesn't have a raw file** (just wants to process something they read online): Save the content to raw/docs/ first, then extract from it. Or if ephemeral, save extract with `source: external` and `source_url: [original URL]`.
- **Multiple related files** (a paper + its code): Do one extract that references both. Use the primary file as `source` and mention the secondary in the body.
- **User wants to re-extract** (understanding improved): Read the existing extract, revise it, note in the frontmatter `date_revised: YYYY-MM-DD`.
