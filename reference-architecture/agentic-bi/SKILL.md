---
name: agentic-bi
description: Set up or operate agentic BI, a chat agent that answers business questions from a data warehouse through read-only tools governed by a semantic layer. Use when asked to let people ask a data warehouse questions in chat, to add a metrics tool to an agent, or to review why a data bot answers wrong.
---

# Agentic BI

Read `README.md` in this folder first. The rules below are the procedure.

## Rules

1. The agent never defines a metric. Metric definitions, grain, timezone and taxonomies live in one semantic layer the tools read.
2. Every tool is read-only and returns provenance: period, filters applied, data-as-of.
3. Nothing ships without a golden question set passing on the real host.

## Steps

1. Confirm the warehouse is reachable from the bot host with a read-only role. Stop if it is not; agentic BI on spreadsheets is guessing.
2. Write the semantic layer as markdown: `data-dictionary.md` (tables, grain, keys, required filters), `metrics.md` (exact definitions), `taxonomies.md` (channel, product group mappings), `examples.md` (verified question → SQL). Put a short router at the top that states the invariants.
3. Build curated tools for the recurring questions. Generate SQL server-side from the definitions. Parameters: period (named or range), metric, comparison window.
4. Add a guarded raw SQL tool: single `SELECT`, AST-validated, `LIMIT` injected, timeout. Add `describe_schema` (progressive) and `resolve_period` (deterministic local-time arithmetic).
5. Append data-as-of to every tool result. Pass it through to the user.
6. Write a golden set: 6-10 questions with expected numbers reconciled by hand. Run it on the real host before cutover and on a timer.
7. Put the agent on the chat surface with role-based access. Keep the customer's dashboard tool; point it at the same warehouse.

## Checks

- Ask the three weekly questions. Numbers match a hand-written query.
- Ask "past week" today and tomorrow. Yesterday's answer does not change.
- Kill the pipeline. The next answer says the data-as-of date, not a fresh-looking number.
- Golden set passes on the host, not only on a laptop.
