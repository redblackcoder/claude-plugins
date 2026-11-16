# Wiki skills

Maintain a topic as one Git repository. Open the repository root as the Obsidian vault. Start at `wiki/README.md` to study and follow relative links into `sources/` to inspect the evidence.

| Input | Command | Result |
|---|---|---|
| A coding prompt and a Java project | `/wiki-skills:update-wiki` with the prompt and project path | A practice page with constraints, a trace and a link to the saved implementation |
| A design prompt, board and later feedback | `/wiki-skills:update-wiki` with the board and notes | A design page showing the recorded proposal, its stopping point and subsequent review |
| A blog URL and a question you want to explore | `/wiki-skills:extract` | A discussion or reading note with the citation, your conclusions and open questions |
| An article you have already studied | `/wiki-skills:update-wiki` with the URL and your takeaway | An update to the relevant concept or a standalone reading note |
| A new topic | `/wiki-skills:bootstrap-wiki` | One repository with a study home, appropriate catalogs and source storage |

Run commands from the topic checkout. Give a path, pasted material or URL directly to the relevant skill. Existing consolidated vaults can start with update-wiki immediately.
