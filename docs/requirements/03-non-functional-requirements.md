# 03 – Non-Functional Requirements

| | |
|---|---|
| **Product** | Demo store for fictional AI and Data training courses |
| **Stack** | Streamlit + Supabase, deployed to Streamlit Community Cloud |
| **Read with** | [01 – User stories](01-product-features-user-stories.md), [02 – UI/UX](02-ui-ux-requirements.md) |
| **Status** | Draft |

## What this document covers

Doc 01 describes **what** the app does. This document describes **how well** it must do it:
security, privacy, safety, performance, accessibility, reliability, code quality, testing and
deployment.

Each requirement has:
- an **ID**, such as NFR-SEC-01,
- a **priority**: Must / Should / Could (same meaning as in doc 01),
- a **check**: how we prove the requirement is met.

---

## SEC – Security

| ID | Requirement | Priority | Check |
|---|---|---|---|
| NFR-SEC-01 | **No secrets in the repository.** Keys live only in `.env` or `.streamlit/secrets.toml` (both git-ignored) locally, and in the Streamlit Cloud Secrets settings when deployed. Only `.example` files with empty values are committed. | Must | Search the repo for key patterns before every push. Run `git check-ignore` on `.env` and `secrets.toml`. |
| NFR-SEC-02 | **Row Level Security (RLS) is turned on for every table.** Access is denied unless a policy allows it. | Must | A SQL check shows RLS is on for all app tables. Integration tests prove a customer cannot read another customer's orders, cart, outbox or progress. |
| NFR-SEC-03 | **The Streamlit app uses only the anon key plus the signed-in user's session.** The service role key is never used inside `app/`. It is only for local scripts such as seeding. | Must | Search `app/` for the service role key name: no results. |
| NFR-SEC-04 | **Roles are stored in the database and enforced there.** Users cannot change their own role. Only the owner can change roles (US-708). | Must | Integration test: a customer who tries to update their own role is rejected. |
| NFR-SEC-05 | **Passwords are handled only by Supabase Auth.** Minimum 8 characters. No app table stores passwords (US-501). | Must | Schema review: no password column exists. |
| NFR-SEC-06 | **Prices are taken from the database at the moment of ordering**, never from the browser or session state. Order placement runs as one database function. | Must | Test: changing a price in the session does not change the order total. |
| NFR-SEC-07 | **File uploads are limited.** Course images: PNG, JPG or WEBP, at most 2 MB. Only staff and owner can upload. Anyone can view (US-703). | Must | Tests for a wrong file type, a file that is too big, and an upload by a customer. |
| NFR-SEC-08 | **User text is never shown as raw HTML.** Text entered by users, such as display names, is not passed to `unsafe_allow_html` or `st.html`. | Must | Code review and a search for `unsafe_allow_html`. |

## PRIV – Privacy

| ID | Requirement | Priority | Check |
|---|---|---|---|
| NFR-PRIV-01 | **Collect as little as possible:** display name and email only. No address, phone number, date of birth or payment details. | Must | Schema review. |
| NFR-PRIV-02 | **Learning progress is private.** XP, levels, badges and progress are only visible to their owner. Staff do not see them in v1. | Must | RLS test. |
| NFR-PRIV-03 | **No real personal data in seed or test data.** Use only `example.com` addresses and made-up names. | Must | Review of the seed files. |
| NFR-PRIV-04 | **No analytics, tracking, advertising or third-party cookies.** | Must | Review of `requirements.txt` and the page source. |
| NFR-PRIV-05 | **Learners may use made-up email addresses**, so no real email address is needed to take part (see open question Q3 in doc 00). | Should | Sign-up works with an `example.com` address. |

## SAFE – Safety and ethics

| ID | Requirement | Priority | Check |
|---|---|---|---|
| NFR-SAFE-01 | **All content is suitable for all ages,** including images, badge art, seed data and copy. | Must | Content review before each demo. |
| NFR-SAFE-02 | **No dark patterns:** no fake timers, fake scarcity, guilt messages, locks, or XP for spending (doc 02, section 4.2). | Must | Design review and UI review using the doc 02 checklist. |
| NFR-SAFE-03 | **No real payments and no real emails.** No payment provider or email provider is a dependency. App emails go to the `outbox` table only. | Must | Review of `requirements.txt`. Test: placing an order adds one outbox row. |
| NFR-SAFE-04 | **The demo banner is shown on every page** (doc 02, S16). | Must | Manual check of every page. |
| NFR-SAFE-05 | **Display names are checked against a blocklist** (US-506). | Should | Unit test with blocked and allowed names. |

## PERF – Performance and cost

| ID | Requirement | Priority | Check |
|---|---|---|---|
| NFR-PERF-01 | **The catalog page loads in 3 seconds or less** on Streamlit Community Cloud with seed data, once the app is awake. The first load after the app has been sleeping does not count. | Should | Timed manual test, 5 runs. |
| NFR-PERF-02 | **Common actions respond in about 1 second or less:** add to cart, remove from cart, mark module complete. | Should | Timed manual test. |
| NFR-PERF-03 | **Catalog reads are cached** with `st.cache_data` and a short time limit (for example 60 seconds). The cache is cleared after staff edit a course. | Should | Code review. An edit shows up in the catalog after saving. |
| NFR-PERF-04 | **The app runs on free plans:** Supabase Free and Streamlit Community Cloud. | Must | No paid service is needed to run the demo. |

## A11Y – Accessibility and usability

| ID | Requirement | Priority | Check |
|---|---|---|---|
| NFR-A11Y-01 | **Target WCAG 2.2 AA** (doc 02, section 8): contrast, keyboard use, visible focus, headings in order. | Must | Automated check (for example a browser accessibility checker) plus a keyboard-only walkthrough of flow F1. |
| NFR-A11Y-02 | **Never show meaning with colour alone.** Every state has a text label. | Must | Design review. |
| NFR-A11Y-03 | **Every image has alt text,** or is marked as decorative. | Must | Code review. |
| NFR-A11Y-04 | **Works at phone width (375 px) and desktop width (1280 px).** | Must | Manual check of S1–S16 at both widths. |
| NFR-A11Y-05 | **Plain English** that a 12-year-old can read (doc 02, section 9). | Should | Copy review. |
| NFR-A11Y-06 | **Supported browsers:** current versions of Chrome, Edge, Firefox and Safari. | Should | Smoke test of flow F1 in each browser. |

## REL – Reliability and data integrity

| ID | Requirement | Priority | Check |
|---|---|---|---|
| NFR-REL-01 | **Placing an order is all-or-nothing.** The order, order items, library items, outbox email and cart clearing all succeed together or none of them happen. | Must | Integration test that forces a failure part-way and checks nothing was saved. |
| NFR-REL-02 | **No duplicate orders from double clicks.** The button is disabled while the order is placed, and the database prevents buying an owned course again. | Must | Test: two quick submits create one order. |
| NFR-REL-03 | **XP is awarded only once per action,** enforced by a unique rule in the database (US-601 AC2). | Must | Integration test. |
| NFR-REL-04 | **Errors are friendly.** Users never see stack traces or raw database errors. Technical details go to the app log. | Must | Force an error and check the screen. |
| NFR-REL-05 | **Demo data can be reset.** The seed script can be run again to restore a clean demo. | Should | Run the seed script twice. The result is the same both times. |
| NFR-REL-06 | **Handle a paused database.** Free Supabase projects can pause when unused for a while. The app shows a clear message instead of crashing. | Should | Test with a wrong or unavailable URL. |

## MAINT – Maintainability

| ID | Requirement | Priority | Check |
|---|---|---|---|
| NFR-MAINT-01 | **Follow the folder structure in the README.** Pages and components handle the UI, and only `app/services/` talks to Supabase. | Must | Code review. Search for `supabase` imports outside `services/`. |
| NFR-MAINT-02 | **Database changes are made only through SQL files** in `supabase/migrations/`, never by hand in the dashboard. | Must | A fresh project can be rebuilt from the migrations alone. |
| NFR-MAINT-03 | **Python 3.11 or newer. Versions in `requirements.txt` are pinned before the first deployment.** | Must | Check `requirements.txt`. |
| NFR-MAINT-04 | **Use a formatter and linter** (for example Ruff), and keep the code free of warnings. | Should | Run the linter. |
| NFR-MAINT-05 | **A new learner can set up and run the app locally within 30 minutes** using the README. | Should | A classmate follows the README on a fresh machine. |

## TEST – Testing

| ID | Requirement | Priority | Check |
|---|---|---|---|
| NFR-TEST-01 | **Unit tests for pure logic:** XP and level calculation, badge rules, cart rules, validation. Tests use pytest and go in `tests/unit/`. | Must | Tests pass. |
| NFR-TEST-02 | **Integration tests for RLS rules and order placement** run against a **test** Supabase project, never the demo project. | Must | Tests pass. The test configuration points to the test project. |
| NFR-TEST-03 | **Every acceptance criterion of a Must story** has an automated test or a written manual test step. | Must | Traceability check. |
| NFR-TEST-04 | **Page tests** for key pages use Streamlit's built-in testing tool (`AppTest`). | Could | Tests pass. |

## DEP – Deployment

| ID | Requirement | Priority | Check |
|---|---|---|---|
| NFR-DEP-01 | **Deploy to Streamlit Community Cloud from the GitHub `main` branch,** with `app/main.py` as the entry point. | Must | The app URL loads. |
| NFR-DEP-02 | **Settings come from `st.secrets` when deployed and from `.env` locally,** handled in one place in `app/config/`. | Must | The app runs both locally and on Cloud without code changes. |
| NFR-DEP-03 | **Separate Supabase projects** for development/testing and for the public demo. | Should | Two project URLs are in use. |
| NFR-DEP-04 | **A step-by-step deployment guide** in `docs/deployment/`. | Must | A classmate deploys their own copy using only the guide. |
| NFR-DEP-05 | **The repository is safe to make public:** no secrets, no real personal data. | Must | NFR-SEC-01 and NFR-PRIV-03 checks pass. |

---

## Summary

| Category | Must | Should | Could | Total |
|---|---|---|---|---|
| SEC | 8 | 0 | 0 | 8 |
| PRIV | 4 | 1 | 0 | 5 |
| SAFE | 4 | 1 | 0 | 5 |
| PERF | 1 | 3 | 0 | 4 |
| A11Y | 4 | 2 | 0 | 6 |
| REL | 4 | 2 | 0 | 6 |
| MAINT | 3 | 2 | 0 | 5 |
| TEST | 3 | 0 | 1 | 4 |
| DEP | 4 | 1 | 0 | 5 |
| **Total** | **35** | **12** | **1** | **48** |
