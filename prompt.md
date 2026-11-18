# Task: Build "Larkepedia" — my LLM-maintained second brain (Project Prometheus)

You are setting up a personal knowledge base for me, **Lark**. It is a folder of markdown files that I will open in **Obsidian**. Future AI agents will read it to understand me deeply and to draft X (Twitter) posts that match my interests and voice. After setup, LLM agents maintain the wiki and I rarely edit it by hand.

Do everything below in order. When you finish, give me a short summary of what you created and a list of every `[todo]` I need to fill in.

---

## 0. Ground rules for this task

- **Do not invent facts about me.** Use only the facts in the "Seed facts" section. Wherever information is missing, write a `[todo]` line that says exactly what I should provide. Never write fake opinions, fake example posts, fake skills or fake handles.
- Use plain markdown with Obsidian-style `[[wikilinks]]`.
- Name files in `kebab-case.md`.
- Initialize git and make one commit at the end with the message `Initialize Larkepedia vault`.

---

## 1. Create this exact folder structure

```
larkepedia/
├── lark.md              ← the constitution: rules for any agent touching this vault
├── raw/                 ← source dump; agents NEVER edit anything here
│   └── .gitkeep
├── wiki/                ← LLM-owned compiled knowledge
│   ├── index.md         ← master map of every wiki page
│   ├── changelog.md     ← append-only log of agent changes
│   ├── me/
│   │   ├── index.md
│   │   ├── profile.md
│   │   ├── interests.md
│   │   ├── opinions.md
│   │   ├── voice-guide.md
│   │   └── now.md
│   ├── concepts/
│   │   └── index.md
│   ├── projects/
│   │   └── index.md
│   └── people/
│       └── index.md
├── outputs/             ← drafts, slides, charts, X posts
│   └── .gitkeep
└── tools/               ← scripts (search, lint, ingest) and saved prompts
    └── prompts/
        ├── ingest.md
        ├── x-post.md
        └── lint.md
```

Also add a `.gitignore` that ignores `.obsidian/workspace*.json` and `.DS_Store`.

---

## 3. Write `lark.md` (the most important file)

This file is the first thing every future agent reads. It must clearly cover all of the following.

**3.1 What the vault is.** Larkepedia is Lark's second brain. It exists so agents can (a) know Lark: identity, goals, interests, opinions, voice and current focus; (b) know what Lark knows: papers, articles, projects and concepts; and (c) act as Lark would, mainly by drafting X posts, threads, explainers, Marp slides and video scripts. The wiki belongs to the LLM. Lark adds raw material and reviews changes through git.

**3.2 Reading order for agents**, as a table:
1. `lark.md`
2. `wiki/me/now.md`
3. `wiki/me/profile.md`
4. `wiki/me/interests.md`
5. `wiki/me/opinions.md`
6. `wiki/me/voice-guide.md` (required before writing anything in Lark's voice)
7. `wiki/index.md`
8. `wiki/changelog.md`

**3.3 Vault layout.** Show the tree from section 1 with a one-line purpose for each folder.

**3.4 Write permissions (hard rules)**, as a table:
- `raw/`: read-only. **Agents NEVER create, edit, rename, move or delete anything in `raw/`.**
- `wiki/` and `outputs/`: the only places agents write.
- `lark.md`: agents don't edit it. They propose changes in `outputs/lark-md-proposals.md`.
- `tools/`: edit only when asked.
- Never delete a wiki page. Mark it `status: deprecated`, give the reason, and link to its replacement.
- **Never publish or post anything.** Agents draft and Lark posts.

**3.5 Required format for every wiki page.** Include this frontmatter template:
```yaml
---
title:
summary: One sentence: what this page covers and why it matters to Lark.
tags: [<type>, <topics...>]      # type = me | concept | project | person | meta
sources: ["[[raw/...]]"]
related: ["[[...]]"]
status: active                    # active | stub | needs-review | deprecated
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```
Rules:
- The summary line appears in the frontmatter and again in **bold** as the first line of the body.
- Reuse existing tags (check `wiki/index.md`) and don't create near-duplicates.
- Every factual claim links back to a source in `raw/`. Web-sourced claims added during linting go in a `## Sources` section with URL and access date.
- Use generous `[[backlinks]]`. Links go both ways: if A links to B, add A to B's `## Related` section.
- End every page with a `## Related` section.
- Keep one concept per page. Split a page if it goes over ~1,500 words.
- Images are stored locally and embedded with `![[...]]`. Never hotlink.

**3.6 Tagging: facts vs. opinions vs. inferences.** Every claim about Lark must carry one of these tags. Show this as a table with examples:

| Tag | Meaning |
|---|---|
| `[fact]` | External, verifiable, sourced |
| `[stated]` | Something Lark said about themself |
| `[opinion YYYY-MM-DD]` | Lark's own take, dated when expressed |
| `[inferred, low/medium/high]` | Agent's conclusion, with reasoning |
| `[todo]` | Missing info Lark should supply |

Rules:
- Only `[stated]` and `[opinion]` lines may be presented as Lark's views in public drafts.
- Never upgrade a tag yourself. When Lark confirms an inference, rewrite it as `[stated] … (confirmed YYYY-MM-DD)`.
- Never delete a changed opinion. Mark the old one `~~superseded YYYY-MM-DD~~` and link it to the new one.
- When unsure, write `[todo]` or ask. Don't guess.

**3.7 After EVERY change**, agents must:
1. Update `wiki/index.md` so every page is listed under its folder with its summary.
2. Update the folder's `index.md`.
3. Append to `wiki/changelog.md` using this format:
   ```
   ## YYYY-MM-DD — <task name>
   - created [[page]] from [[raw/...]]
   - updated [[page]]: <what changed>
   ```
4. Leave the vault reviewable in a single `git diff`.

**3.8 Standard workflows.** Summarize each one and point to its prompt file in `tools/prompts/`: Ingest, Q&A, Drafting X posts, Monthly lint.

**3.9 Privacy.** Never store passwords, API keys, ID numbers, bank details, or other people's private information. Pages in `wiki/people/` cover only public work and the person's relationship to Lark. If a raw item contains something sensitive, summarize around it and flag it in the changelog.

**3.10 When in doubt:** ask rather than guess, add rather than rewrite, write `[todo]` rather than invent, and make small changes rather than vault-wide rewrites.

---

## 4. Write the five `wiki/me/` files

Every file uses the frontmatter from 3.5, the tags from 3.6, and a `## Related` section that links to the other `me/` files.

**`profile.md`**: who Lark is and what Lark is aiming for.
Sections:
- Identity
- Career direction
- What Lark does (research / content / building, each linking to its project page)
- Skills & stack
- Background story
- Public presence (handles)

Fill these in from the seed facts. Everything else is `[todo]`.

**`interests.md`**: topics ranked by how much Lark cares.
- Use tiers: **Tier 1 core obsessions**, **Tier 2 strong interests**, **Tier 3 exploring**, and **Avoid**.
- Show each tier as a table with Topic | Tag | Evidence.
- Seed them as follows:
  - Tier 1: shared vision–audio embeddings; contrastive alignment models (CLIP/AudioCLIP/CLAP); hands-on ML engineering
  - Tier 2: image-conditioned audio for games; LLM-maintained personal knowledge bases; building in public
  - Avoid: LLM-agent/API-wrapper projects as content
- Add `[todo]` items for non-tech interests and favourite accounts.
- Add a note that only Lark changes tiers. Agents may suggest re-ranks, tagged `[inferred]`.

**`opinions.md`**: Lark's takes, each one dated.
- Start with a short explanation of the format: `[opinion YYYY-MM-DD] <take> — context/why`, how to supersede an old take, and that only these lines may be used as Lark's views in posts.
- Group them by topic: AI/ML engineering, Research, Content creation, Tools.
- You may record only one seed opinion: `[opinion 2026-11] Real ML engineering projects > LLM-agent/API-wrapper projects for showing skill`.
- Add 8–10 `[todo]` prompts asking for Lark's take on relevant questions, for example:
  - "Is CLIP-style contrastive learning the right default for audio–visual alignment?"
  - "Are review papers worth it early in a career?"
  - "Hot take on AI content creators?"

**`voice-guide.md`**: how Lark writes.
Sections:
- **Tone:** `[todo]`, with a checklist for Lark to tick (casual / technical / witty / blunt / teacherly…)
- **Sentence length & structure:** `[todo]`
- **Emoji & formatting** (emoji, hashtags, threads vs. single posts, line breaks): `[todo]`
- **Words & phrases Lark uses:** `[todo]`
- **Words & phrases Lark would never use:** `[todo]`. Pre-fill a *suggested* list for Lark to confirm, clearly labeled `[inferred, low] suggested — confirm or delete`, of generic AI-sounding filler such as "delve", "game-changer", "unlock", "in today's fast-paced world" and "🚀 thread 🧵".
- **Post formats Lark likes:** build logs, paper breakdowns, hot takes, "what I learned" threads (`[todo]` to confirm)
- **Example posts Lark is proud of:** 10–20 empty numbered slots formatted as `> post text` + `why it works:`. **Leave them empty and do not write sample posts.**
- **Lessons from Lark's edits:** an empty log where agents record how Lark changed their drafts

**`now.md`**: what Lark is focused on this month.
- Header: `Focus for November 2026` with `last updated` and a note: *"If this is more than 5 weeks old, agents should flag it as stale and ask Lark."*
- Seed it with:
  - finishing and submitting the review paper (Springer Nature template)
  - launching the Instagram/YouTube "project of the day" content
  - setting up Larkepedia
- Add sections: This month's priorities, Currently learning (`[todo]`), Currently building, Questions on my mind (`[todo]`), Not focusing on right now (`[todo]`).

---

## 5. Seed the other wiki folders

- **`wiki/projects/`**: create three pages from the seed facts, each with `status: active`:
  - `review-paper.md`: title, RQs, models compared, motivation, status, submission target, plus `[todo]` for co-authors and deadline
  - `tech-content.md`: format, workflow, constraints, plus `[todo]` for handles, launch date and first project ideas
  - `project-prometheus.md`: what it is, inspiration (an X post about LLM-compiled knowledge bases), workflow (raw → compile → wiki → outputs → file back), and future ideas (search CLI, MCP server so any agent can query the vault, fine-tuning a small model on Lark's voice)
- **`wiki/concepts/`**: create **stub** pages (`status: stub`, summary + `[todo]` to compile once papers are added to `raw/`) for:
  - `shared-embedding-spaces.md`
  - `contrastive-learning.md`
  - `clip.md`
  - `audioclip.md`
  - `clap.md`
  - `image-conditioned-audio-generation.md`
  
  Link them to each other and to `review-paper`. Do **not** write technical content from memory. These pages get compiled from sources later.
- **`wiki/people/`**: index only, with a `[todo]` asking for researchers/creators Lark follows.
- **All index files**: list every page with its summary. `wiki/index.md` should also include a "Start here" pointer to `lark.md` and a tag list.
- **`wiki/changelog.md`**: first entry dated today, listing everything created.

---

## 6. Write the saved prompts in `tools/prompts/`

Each prompt must be a complete standalone instruction that a future agent can run without any other context. Each one starts with "Read `lark.md` first."

- **`ingest.md`**
  1. Find files in `raw/` that are not yet referenced in any wiki page's `sources`.
  2. For each one, merge its knowledge into existing concept/project/people pages, or create new ones. Organize by concept, not one page per source.
  3. If a file is in Lark's own notes or posts, add `[stated]`/`[opinion]` lines to `wiki/me/`.
  4. Update the indexes and append to the changelog.

- **`x-post.md`**
  1. Read `voice-guide`, `now`, `interests`, `opinions` and the last 2 weeks of the changelog.
  2. Pick topics from real recent activity only. Never invent experiences, results, numbers or projects.
  3. Write 3–5 options (single post / thread / hot take) to `outputs/x-draft-YYYY-MM-DD-<topic>.md`. Each option includes a `based on:` line with links.
  4. Never post anything.
  5. When Lark's edited version is later added to `raw/`, update the "Lessons from Lark's edits" section of the voice guide.

- **`lint.md`** checks for:
  - contradictions
  - a stale `now.md`
  - orphan pages
  - missing sources or tags
  - duplicate tags
  - open `[todo]`s
  
  It then suggests 5 new article candidates and 5 questions to explore, writes the report to `outputs/lint-YYYY-MM.md`, and fixes only mechanical issues (links, indexes, tags).

---

## 7. Final checks before you finish

- [ ] Every wiki file has complete frontmatter and a `## Related` section.
- [ ] Every claim about Lark has a tag, and nothing beyond the seed facts is stated as fact.
- [ ] No example posts or opinions were invented.
- [ ] Every `[[wikilink]]` points to a file that exists, or to a deliberate stub.
- [ ] All index files list every page.
- [ ] `raw/` contains only `.gitkeep`.
- [ ] Git commit made.

Then reply with:
1. the folder tree you created
2. every `[todo]`, grouped by file, so I can fill them in