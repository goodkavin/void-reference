# Reference architecture

Twelve components any business should install now, in the order they depend on each other. One folder per component: a one-page card for an engineer, and the same knowledge as an installable Agent Skill.

Written from things we have run for real clients, not from a vendor sheet. Tool-agnostic; every card names one example stack because "it depends" is not an architecture.

## The map

| # | Component | Depends on |
|---|---|---|
| 01 | Lightweight data warehouse | |
| 02 | Unstructured data ETL (Sheets, PDF, chat exports) | 01 |
| 03 | Automated parity checks | 01, 02 |
| 04 | Semantic layer | 01 |
| 05 | Context layer (OKF) | 04 |
| 06 | Chat agent with role-based access | 04, 05 |
| 07 | Agentic BI | 04, 05, 06 |
| 08 | LLM judge / tester | 06, 07 |
| 09 | Alert bot | 03, 06 |
| 10 | Signal dashboard | 04, 05, 09 |
| 11 | ERP agent (draft-first) | 05, 06 |
| 12 | Company OS | 05, 06, 10 |

Cards land one at a time. A row links to its folder once it exists.

## Each folder

```
NN-slug/
├── README.md   the card: what, depends on, outcome, pitfalls, pattern, example stack, setup
└── SKILL.md    the same, as an Agent Skill (agentskills.io) your coding agent can install
```

## Install as skills

Once the first `SKILL.md` lands, in Claude Code:

```
/plugin marketplace add goodkavin/void-reference
```

The Instagram series `#reference-architecture` at [@voidengineer.ing](https://www.instagram.com/voidengineer.ing/) is the cover for each card here.
