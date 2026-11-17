# Codex instructions for this Obsidian vault

## Scope

- This directory is an Obsidian vault. Treat all existing notes, metadata, links, attachments, canvases, and Bases files as user-owned content.
- Communicate in Chinese by default. Keep filenames, paths, properties, tags, and code identifiers in their original form.
- Make the smallest change that satisfies the task. Begin with read-only inspection unless the user explicitly asks for edits.

## Vault role and knowledge ownership

- This vault is the comprehensive knowledge base and human operations surface for sources, evidence, general L0–L3 engineering knowledge, review queues, transfer records, decisions, questions, retrospectives, and history.
- `D:\666\AI_Global_Assistant` defines workspace-wide Codex governance, configuration, prompts, skills, and knowledge-routing rules. It does not own engineering knowledge bodies; governance views in this vault are read-only derived views of those rules.
- V500E and volant_XJ project facts, parameters, interfaces, model structures, logs, data, and project validation bodies remain in their project knowledge bases. This vault stores project entry cards, source pointers, transfer tickets, and newly abstracted knowledge objects, not copied project facts.
- General engineering methods stay in `30_知识维基` from L0/L1 candidacy through L2/L3 promotion. G1–G5 audit tickets stay in `40_审核队列/跨项目迁移`; promotion does not move the method body into `AI_Global_Assistant`.
- Before knowledge work, read `index.md` and the relevant part of `知识库迭代机制方案.md`. A default engineering conclusion must simultaneously have `record_role=canonical`, `maturity=L2/L3`, `review_status=approved`, matching `scope`, permitted `sensitivity`, no `stale/conflicted/superseded` flag, and an unexpired `review_due`.
- `D:\666\FlightControlKnowledgeableBase` is outside the current scope and must not be scanned, ingested, synchronized, or modified without a later explicit user instruction.

## Obsidian compatibility

- Preserve YAML frontmatter, property names, tags, heading hierarchy, block IDs, callouts, footnotes, code fences, comments, and Markdown formatting unless the task explicitly targets them.
- Preserve Obsidian wikilinks and embeds, including `[[Note]]`, `[[Note#Heading]]`, `[[Note|Alias]]`, `![[Attachment]]`, and block references.
- Preserve the existing link style. Do not convert wikilinks to standard Markdown links, or the reverse, unless explicitly requested.
- Write text files as UTF-8. Do not introduce a new metadata schema or folder hierarchy without first proposing it.
- Do not modify `.base` files unless the task explicitly concerns Obsidian Bases.

## Protected content

- Do not modify `.obsidian/`, `.codex/`, or this `AGENTS.md` unless the user explicitly asks for configuration changes.
- Do not modify binary files or attachments unless explicitly requested.
- Never expose vault content, credentials, private notes, or attachments to external services unless the user explicitly authorizes the exact transfer.

## Destructive and bulk operations

- Before any bulk rewrite, move, rename, merge, or deletion, present the exact affected paths and wait for confirmation.
- Never delete notes or attachments without explicit confirmation.
- When moving or renaming a note, identify affected wikilinks and embeds, update them consistently, and check for dangling links afterward.
- If the target folder or naming convention is unclear, propose a destination instead of inventing one.

## Verification and handoff

- After edits, report every created, modified, moved, renamed, or deleted path.
- Verify that YAML frontmatter remains valid, Obsidian links and embeds remain intact, and no protected paths changed unexpectedly.
- Clearly distinguish confirmed findings, reasonable inferences, unresolved questions, and recommendations when analyzing the vault.
