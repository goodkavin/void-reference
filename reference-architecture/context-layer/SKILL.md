---
name: context-layer
description: Build or maintain a context layer, the curated pack of company knowledge (company, people, systems, rules, SOPs, data definitions) that agents, BI tools and automations read before they answer. Use when asked to give an agent business context, to stop a bot inventing facts about the company, or to consolidate knowledge scattered across docs, chats and prompts.
---

# Context layer

Read `README.md` in this folder first. The rules below are the procedure.

## Rules

1. One fact, one home. The pack is generated from a source of truth and never edited in place.
2. Every fact carries a status (`real` / `pending` / `blocked`) and an owner. A gap is answered as a gap, never filled from general knowledge.
3. Load the router first and one reference file on demand. Never paste the whole pack into a prompt.
4. Metric definitions belong to the semantic layer. Reference them; do not restate them here.

## Steps

1. Inventory what the company already keeps: catalog, SOPs, planning workbooks, decks, meeting notes, chat threads, ERP and CRM. Note which system is the source for which fact.
2. Name one owner per area. Six areas: company, people, systems, rules, SOPs, data definitions.
3. Write the six reference files as markdown, one concept per section, cross-linked. Anything unsourced is `pending`, not a guess.
4. Write the router (`SKILL.md`): what the pack is, which file answers which kind of question, the status legend, and the no-guessing rule.
5. Move the facts into a typed, reviewable source of truth and add a generator that renders the pack. Nothing downstream reads the source directly.
6. Add a CI job that re-renders the pack and fails on a diff, so a hand edit or a stale commit cannot ship.
7. Give it one ingestion path: a note or document goes in through a skill, the private full text stays private, the shareable subset updates the source of truth by pull request.
8. Write 6-10 golden questions that probe coverage. A non-refusing, correct answer passes.
9. Sync the pack to each agent host and load it as a skill. Measure the same question set with and without it before installing the next component.

## Checks

- Ask a question whose answer exists only in the pack. The agent answers and cites where it came from.
- Ask a question the pack does not cover. The agent says it is missing instead of inventing.
- Hand-edit a generated file and push. CI fails.
- Grep the semantics across the host. The metric appears once, not in three places that disagree.
- Run the golden set on the real host, not on a laptop.
