# Requirements

Requirements for the demo store selling fictional AI and Data training courses
(Streamlit + Supabase). **Start with document 00.**

| # | Document | What it covers | Main readers |
|---|---|---|---|
| 00 | [Consolidated requirements](00-consolidated-requirements.md) | One-page summary: goals, roles, features, UI/UX, NFRs, data model, scope, **open questions**, build order, traceability | Everyone |
| 01 | [Product features and user stories](01-product-features-user-stories.md) | 7 epics, 39 user stories (US-101 …) with priorities and acceptance criteria, domain terms, seed data, out of scope | Developers, testers |
| 02 | [UI/UX requirements](02-ui-ux-requirements.md) | Vision, design principles, visual direction, gamification rules, sitemap, flows, screens S1–S16, Streamlit limits, designer deliverables | Designer, developers |
| 03 | [Non-functional requirements](03-non-functional-requirements.md) | 48 NFRs (NFR-SEC-01 …) for security, privacy, safety, performance, accessibility, reliability, code quality, testing, deployment | Developers, testers |

## How the IDs connect

```
Epic (E1–E7) → User story (US-xxx, doc 01) → Screen (S1–S16, doc 02) → NFR (NFR-xxx, doc 03)
```

The full map is in section 11 of document 00.

## Changing requirements

- Change the detailed document (01, 02 or 03) first, then update the summary in 00.
- When the class answers an open question in 00, section 9, record the decision there and update any document it affects.
