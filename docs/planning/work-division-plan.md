# Work Division Plan

| | |
|---|---|
| **Product** | Demo store for fictional AI and Data training courses (Streamlit + Supabase) |
| **Based on** | [00 Consolidated](../requirements/00-consolidated-requirements.md) · [01 Stories](../requirements/01-product-features-user-stories.md) · [02 UI/UX](../requirements/02-ui-ux-requirements.md) · [03 NFRs](../requirements/03-non-functional-requirements.md) |
| **Team** | Tech Lead + User 1, User 2, User 3 (see section 2 for a team of 6) |
| **Repository** | One GitHub repository, `main` branch protected |

## 1. How the work flows

```
Phase 1  FOUNDATION   Tech Lead builds the base and pushes it to GitHub.
                      Users set up their machines, learn the stack and review the Lead's PRs.
                                   │
Phase 2  STORIES      Each user works through their own list, ONE story at a time:
                      pick → branch → build → test → pull request → review → merge → next
                                   │
Phase 3  RELEASE      Everything merged, full test pass, NFR checklist, deploy, tag v1.0.
```

The base is built first so that later work does not wait on other people. When Phase 2
starts, the database, security rules, sign-in, every page (as a stub), every service function
(as a stub) and the shared components already exist. Each user then fills in **their own
files**, and almost every dependency stays **inside their own list**.

## 2. Team and roles

| Role | Area | Stories | Main reviewer |
|---|---|---|---|
| **Tech Lead** | Foundation, security, releases | US-501 – 504 (inside the foundation) | Users take turns reviewing the Lead's PRs. This is how they learn the base. |
| **User 1** | **Discover:** catalog, course detail, learning paths, publishing | 12 stories | User 2 |
| **User 2** | **Buy and account:** cart, checkout, orders, outbox, profile | 13 stories (1 optional) | User 3 |
| **User 3** | **Learn and manage:** progress, XP, levels, badges, course editor, team | 10 stories | User 1 |

- Any PR labelled `schema-change` (a new migration, RLS policy or database function) also needs the **Tech Lead's** approval.
- **The reviewer is also the backup owner.** If a user is away, their reviewer can take over the next story.

**A team of 6:** the Tech Lead has a co-lead, and each User list is worked by **a pair**
(one person types, the other reviews as they go, and they swap each story). The rules do
not change: each list still has one story in progress at a time.

---

## 3. Phase 1 – Foundation (Tech Lead)

Milestone: **Foundation**. Label: `user:lead`. Build these in order. Each one is a PR.

| Code | Foundation item | Includes | Size |
|---|---|---|---|
| **L-01** | Repository setup | Branch protection on `main`, PR template, issue template, `CODEOWNERS`, labels and milestones (section 7) | S |
| **L-02** | CI | GitHub Actions: Ruff linter + pytest unit tests on every PR | S |
| **L-03** | Config and Supabase client | `app/config/settings.py` (reads `st.secrets`, falls back to `.env`), `app/services/supabase_client.py`, pinned `requirements.txt` | S |
| **L-04** | Database schema | Migrations for all 16 tables in doc 00, section 6, with keys and unique rules (one course per user's cart and library, one XP award per action) | L |
| **L-05** | Security rules | RLS on every table, `course-images` storage bucket rules, and a rule that stops users changing their own `role` | L |
| **L-06** | Seed data and reset | `supabase/seed/`: 10 courses with modules, 2 learning paths, levels, badges, demo accounts, Ada's library and progress. `scripts/reset_demo.py` | M |
| **L-07** | App shell, **US-504** | `app/main.py` with **every page S1–S16 registered as a stub**, navigation grouped by role, access check, demo banner, theme in `.streamlit/config.toml` | M |
| **L-08** | Sign-in, **US-501, 502, 503** | Sign in / register page (S6), sign out, `auth_service`, the current user in session state, and an `on_sign_in` hook that calls `cart_service.merge_guest_cart` (a stub for now) | M |
| **L-09** | Service stubs | Every service file in section 6 with its function names, inputs, outputs and docstrings. Each stub returns sample data or raises `NotImplementedError` | M |
| **L-10** | Shared components and component stubs | Shared: `states.py` (S16), `demo_banner.py`, `nav.py`, `format.py` (money, dates), `gamification_visible()` helper. **Stub files for every area component** in section 6, already placed on their pages | M |
| **L-11** | Test setup | `pytest.ini`, fixtures, a **guard that refuses to run integration tests against the demo database**, one sample unit, integration and `AppTest` test | M |
| **L-12** | First deployment | Deploy to Streamlit Community Cloud with secrets, a draft `docs/deployment/` guide, and README setup steps | S |

**While the Lead builds the foundation, Users 1–3:**
1. Install the tools, clone the repo, and run each foundation PR locally.
2. **Review the Lead's PRs.** This is the fastest way to learn the codebase.
3. Read docs 01–03 for their stories. Turn each acceptance criterion into a test idea.
4. Sketch their screens (doc 02).

**Foundation is done when:** L-01 to L-12 are merged, the deployed app shows every page as a stub,
sign-in works, CI is green, and the issue for every story in section 4 exists. **Then Phase 2 starts for everyone on the same day.**

---

## 4. Phase 2 – Stories

### 4.1 The story loop (one story at a time)

1. **Pick** the next story in **your** list (the lowest pick code that is not done). Assign the issue to yourself and move it to *In progress*.
2. **Branch** from the latest `main`: `feat/U2-04-place-order`.
3. **Build** the story, mostly in **your own files** (section 6).
4. **Test:** add unit tests, and integration tests if you changed the database. Run `ruff` and `pytest` locally.
5. **Open a pull request** to `main` using the template. Write `Closes #<issue>` and add screenshots.
6. **Review:** your reviewer reviews within one working day. The Tech Lead also reviews `schema-change` PRs.
7. **Merge** (squash), delete the branch, check the change on the deployed app, and close the issue.
8. **Next story.** Go back to step 1.

If you are waiting for a review, start reading the **next** story and planning tests. Do not open a second branch.

**Size guide:** **S** = about one class session, **M** = 2–3 sessions, **L** = 4 or more sessions. Points: S = 1, M = 2, L = 3.

**"Depends on: –"** means the story depends only on the foundation, which is already merged before Phase 2.

### 4.2 User 1 – Discover

| Pick | Story | Pri | Size | Depends on | Files touched |
|---|---|---|---|---|---|
| **U1-01** | US-101 Browse the catalog | Must | M | – | `pages/catalog.py`, `pages/home.py`, `services/catalog_service.py`, `components/course_card.py`, `models/course.py`, `tests/unit/test_catalog_*.py` |
| **U1-02** | US-201 View course details | Must | M | U1-01 | `pages/course_detail.py`, `services/catalog_service.py`, `tests/unit/test_catalog_*.py` |
| **U1-03** | US-102 Search by keyword | Must | S | U1-01 | `services/catalog_service.py`, `components/catalog_filters.py`, `pages/catalog.py` |
| **U1-04** | US-704 Publish and unpublish | Must | S | U1-01 | `pages/manage_courses.py`, `services/course_status_service.py`, `components/course_status_controls.py`, `tests/integration/test_status_*.py` |
| **U1-05** | US-103 Filter the catalog | Should | M | U1-03 | `components/catalog_filters.py`, `services/catalog_service.py` |
| **U1-06** | US-202 See the course outline | Should | S | U1-02 | `pages/course_detail.py`, `services/catalog_service.py` |
| **U1-07** | US-705 Archive a course | Should | S | U1-04 | `services/course_status_service.py`, `components/course_status_controls.py`, `pages/manage_courses.py` |
| **U1-08** | US-203 Course in a learning path | Could | M | U1-02 | `pages/course_detail.py`, `services/learning_path_service.py`, `components/path_preview.py` |
| **U1-09** | US-604 Learning-path skill tree | Should | L | U1-08 | `pages/learning_paths.py`, `components/skill_tree.py`, `services/learning_path_service.py` |
| **U1-10** | US-204 See related courses | Could | S | U1-02 | `pages/course_detail.py`, `services/catalog_service.py` |
| **U1-11** | US-104 Sort the catalog | Could | S | U1-05 | `components/catalog_filters.py`, `services/catalog_service.py` |
| **U1-12** | US-105 Page through results | Could | S | U1-11 | `pages/catalog.py`, `components/pagination.py` |

**Total: 12 stories, 18 points.** Must stories come first (U1-01 to U1-04).
Note for U1-09: the skill tree reads progress straight from the seeded tables and uses the `gamification_visible()` helper from the foundation.

### 4.3 User 2 – Buy and account

| Pick | Story | Pri | Size | Depends on | Files touched |
|---|---|---|---|---|---|
| **U2-01** | US-301 Add a course to the cart | Must | M | – | `services/cart_service.py`, `components/add_to_cart_button.py`, `models/cart.py`, `tests/unit/test_cart_*.py` |
| **U2-02** | US-302 View and edit the cart | Must | M | U2-01 | `pages/cart.py`, `services/cart_service.py` |
| **U2-03** | US-401 Sign in before checkout | Must | S | U2-02 | `pages/checkout.py`, `pages/cart.py` |
| **U2-04** | US-402 Place a demo order `schema-change` | Must | L | U2-03 | `supabase/migrations/<timestamp>_place_order.sql`, `services/order_service.py`, `pages/checkout.py`, `models/order.py`, `tests/integration/test_order_*.py` |
| **U2-05** | US-403 See the order confirmation | Must | S | U2-04 | `pages/order_confirmation.py`, `services/order_service.py` |
| **U2-06** | US-404 Email saved to the outbox `schema-change` | Must | M | U2-04 | `supabase/migrations/<timestamp>_order_outbox.sql`, `services/order_service.py`, `tests/integration/test_order_*.py` |
| **U2-07** | US-304 Prevent buying a course twice | Should | S | U2-04 | `services/cart_service.py`, `components/add_to_cart_button.py` |
| **U2-08** | US-303 Keep the cart after signing in | Should | M | U2-02 | `services/cart_service.py` (`merge_guest_cart`, already called by the foundation) |
| **U2-09** | US-405 View my orders | Should | S | U2-04 | `pages/my_orders.py`, `services/order_service.py` |
| **U2-10** | US-707 View all orders (staff) | Should | S | U2-04 | `components/admin_orders.py`, `services/order_service.py` |
| **U2-11** | US-406 View my outbox emails | Could | S | U2-06 | `pages/my_orders.py`, `services/order_service.py` |
| **U2-12** | US-506 Edit my profile | Could | S | **U3-02** (cross-user) | `components/profile_settings.py`, `services/profile_service.py` |
| **U2-13** | US-505 Reset my password *(optional: only if the class changes Q4)* | Could | M | U2-06 | `pages/sign_in.py` (with the Lead's review), `services/auth_service.py` |

**Total: 13 stories, 20 points (18 without the optional U2-13).** Must stories come first (U2-01 to U2-06).

### 4.4 User 3 – Learn and manage

| Pick | Story | Pri | Size | Depends on | Files touched |
|---|---|---|---|---|---|
| **U3-01** | US-605 Mark a module as complete | Must | M | – (uses Ada's seeded library) | `pages/my_learning.py`, `pages/course_progress.py`, `services/progress_service.py`, `tests/unit/test_progress_*.py` |
| **U3-02** | US-601 Earn XP for learning actions | Must | M | U3-01 | `services/gamification_service.py` (`award_xp`), `models/gamification.py`, `components/xp_chip.py`, `tests/unit/test_gamification_*.py` |
| **U3-03** | US-602 Level up | Must | M | U3-02 | `models/gamification.py`, `components/level_message.py`, `components/progress_panel.py` |
| **U3-04** | US-701 Add a course | Must | M | – | `pages/course_editor.py`, `services/course_admin_service.py`, `tests/unit/test_admin_*.py` |
| **U3-05** | US-703 Upload a course image | Must | M | U3-04 | `services/storage_service.py`, `pages/course_editor.py`, `tests/integration/test_storage_*.py` |
| **U3-06** | US-702 Edit a course | Must | S | U3-04 | `pages/course_editor.py`, `services/course_admin_service.py` (calls `clear_catalog_cache()` from the foundation) |
| **U3-07** | US-603 Earn badges | Should | M | U3-02 | `services/gamification_service.py`, `components/badge_grid.py`, `components/progress_panel.py` |
| **U3-08** | US-706 Manage modules and learning paths | Should | L | U3-06 | `components/module_editor.py`, `services/course_admin_service.py`, `pages/course_editor.py` |
| **U3-09** | US-708 Manage staff accounts | Could | M | – | `components/team_admin.py`, `services/team_service.py`, `tests/integration/test_team_*.py` |
| **U3-10** | US-606 Hide gamification | Should | S | U3-03, U3-07 (**U1-09** for final testing only) | `components/gamification_settings.py`, `services/profile_service.py` (`show_gamification` field only) |

**Total: 10 stories, 19 points.** Must stories come first (U3-01 to U3-06).

### 4.5 Workload and start-day check

| User | Stories | Points | Must | First story | First story depends on another story? |
|---|---|---|---|---|---|
| User 1 | 12 | 18 | 4 | U1-01 (US-101) | **No.** Foundation only |
| User 2 | 13 (1 optional) | 20 (18) | 6 | U2-01 (US-301) | **No.** Foundation only |
| User 3 | 10 | 19 | 6 | U3-01 (US-605) | **No.** Foundation plus seed data |
| Tech Lead | 4 (in foundation) | – | 4 | L-01 | **No** |

All 39 stories are assigned: Lead 4 + User 1 12 + User 2 13 + User 3 10. All 20 Must stories come before the Should and Could stories in every list.

## 5. Cross-user dependencies

There are only **two** hard dependencies between users. Both are late in their lists, so neither one blocks anyone's start.

| Story | Waits for | Why | Workaround if it's not ready |
|---|---|---|---|
| **U2-12** US-506 Edit profile | **U3-02** US-601 (`award_xp`) | Completing the profile earns +20 XP | Build name editing and the blocklist first. Add the `award_xp` call in a small follow-up PR once U3-02 is merged. |
| **U3-10** US-606 Hide gamification | **U1-09** US-604 (skill tree) | AC1 hides the skill tree progress too | Not a build blocker: the skill tree already checks `gamification_visible()`. Only the final test of the skill tree part waits for U1-09. |

**Soft links** (no waiting needed, because the foundation already provides a stub):

| Where | Uses | Handled by |
|---|---|---|
| U1 course card and detail page | U2's `add_to_cart_button` | The Lead placed the stub component on the pages. It starts working when U2-01 is merged. |
| U3 course editor | U1's `course_status_controls` | Stub component in the editor. It starts working when U1-04 is merged. |
| U3-06 edits a course | U1's catalog cache | `clear_catalog_cache()` exists in the foundation. |
| U3-01 progress | Courses bought through U2-04 | Uses seeded library data until checkout works. |
| U2-08 cart merge | Sign-in | The Lead's `on_sign_in` hook already calls `merge_guest_cart`. |
| S11 profile and S15 orders/team pages | Components from two users | The Lead owns the page shell. Each user only edits their own component. |

## 6. File ownership (how we avoid merge conflicts)

**Main rule:** the Tech Lead builds every **page shell** and places a stub **component** or
**service function** for each area. After that, users mostly edit **their own** files. Two
users never need to edit the same file in the same week.

| Owner | Files |
|---|---|
| **Tech Lead** | `app/main.py`, `app/config/**`, `requirements.txt`, `.streamlit/config.toml`, `.github/**`, `supabase/migrations/**` (review), `supabase/seed/**`, `scripts/**`, `services/supabase_client.py`, `services/auth_service.py`, `pages/sign_in.py`, `pages/profile.py` (shell), `pages/orders_and_team.py` (shell), `components/states.py`, `demo_banner.py`, `nav.py`, `format.py`, `tests/conftest.py` |
| **User 1** | `pages/home.py`, `catalog.py`, `course_detail.py`, `learning_paths.py`, `manage_courses.py` · `services/catalog_service.py`, `learning_path_service.py`, `course_status_service.py` · `components/course_card.py`, `catalog_filters.py`, `course_status_controls.py`, `path_preview.py`, `skill_tree.py`, `pagination.py` · `models/course.py` |
| **User 2** | `pages/cart.py`, `checkout.py`, `order_confirmation.py`, `my_orders.py` · `services/cart_service.py`, `order_service.py`, `profile_service.py` · `components/add_to_cart_button.py`, `admin_orders.py`, `profile_settings.py` · `models/cart.py`, `order.py` |
| **User 3** | `pages/my_learning.py`, `course_progress.py`, `course_editor.py` · `services/progress_service.py`, `gamification_service.py`, `course_admin_service.py`, `storage_service.py`, `team_service.py` · `components/xp_chip.py`, `level_message.py`, `progress_panel.py`, `badge_grid.py`, `module_editor.py`, `team_admin.py`, `gamification_settings.py` · `models/gamification.py` |

All paths above are under `app/` unless they start with `supabase/`, `scripts/`, `tests/` or `.github/`.

**Rules**
1. **Don't edit another person's file.** If you need a change, ask the owner, or open a small separate PR and request the owner as reviewer.
2. **One shared exception:** `services/profile_service.py` is User 2's file. User 3 adds only the `show_gamification` field in U3-10, with User 2 as reviewer.
3. **Migrations:** add a new file named with a timestamp (`20261201093000_place_order.sql`). Never edit a merged migration. Label the PR `schema-change`.
4. **`requirements.txt`:** ask the Tech Lead. Add one line per PR.
5. **Session state and widget keys:** start with your area name (`u1_`, `u2_`, `u3_`, `lead_`).
6. **Database access only in `services/`.** Pages call services and never query tables directly (NFR-MAINT-01).
7. **Before opening a PR,** update your branch with the latest `main`.

**Tests:** name test files after your area (`test_catalog_*`, `test_cart_*`, `test_order_*`, `test_progress_*`, `test_gamification_*`, `test_admin_*`) so test files never clash.

---

## 7. GitHub setup

### 7.1 Labels

| Group | Labels | Used for |
|---|---|---|
| **User** | `user:1`, `user:2`, `user:3`, `user:lead` | Who owns the issue |
| **Epic** | `epic:E1` … `epic:E7` | Feature area (doc 01) |
| **Type** | `type:story`, `type:foundation`, `type:bug`, `type:chore`, `type:docs` | Kind of work |
| **Priority** | `priority:must`, `priority:should`, `priority:could` | From doc 01 |
| **Size** | `size:S`, `size:M`, `size:L` | From section 4 |
| **Flags** | `schema-change`, `blocked` | Needs the Lead's review / waiting on something |

### 7.2 Milestones

| Milestone | Contains |
|---|---|
| **Foundation** | L-01 to L-12 |
| **Stories** | Every U1, U2 and U3 story |
| **Release v1.0** | Release tasks in section 8, plus bugs found during the final test |

### 7.3 Issues

- **Title:** pick code, story ID and name, for example `U2-04 US-402 Place a demo order`. Foundation issues: `L-04 Database schema`.
- **Body:** the story text, the acceptance criteria as a checklist, *Depends on*, *Files touched*, and the reviewer.
- **Labels:** user + epic + type + priority + size (+ `schema-change` if needed).
- **Project board:** Backlog → Ready → In progress → In review → Done. Each user has **at most one** issue in *In progress*.
- If a story is waiting on something, add `blocked` and write what it is waiting for.

### 7.4 Branches and pull requests

- `main` is protected: changes only through PRs, CI must pass, at least 1 approval, no force pushes.
- **Branch names:**
  - `feat/U1-03-search`
  - `fix/U2-04-double-order`
  - `chore/L-02-ci`
  - `docs/deployment-guide`
- **PR:** title starts with the pick code. The body includes `Closes #<issue>`, how to test, screenshots (desktop and mobile), and any migration or new package.
- **Merge:** use **Squash and merge**, then delete the branch.

### 7.5 Definition of Done (for every story)

- [ ] Every acceptance criterion is met and ticked in the issue.
- [ ] Unit tests are added. Integration tests are added for any database or RLS change. Any manual test steps are written in the PR.
- [ ] `ruff` and `pytest` pass in CI.
- [ ] The reviewer approved the PR (and the Tech Lead too, for `schema-change`).
- [ ] Only your own files were changed, or the owner reviewed changes to theirs.
- [ ] No secrets, no real personal data, all content suitable for all ages, and no dark patterns.
- [ ] Empty, error and loading states work at 375 px and 1280 px.
- [ ] Merged, and checked on the deployed app.

---

## 8. Phase 3 – Release v1.0

Milestone: **Release v1.0**. Led by the Tech Lead, with everyone helping.

| Step | What | Who |
|---|---|---|
| R-1 | **Code freeze:** only bug fixes from now on. Every Stories issue is closed or moved to "after v1.0". | Lead |
| R-2 | Reset the demo database from migrations and seed data (NFR-REL-05). | Lead |
| R-3 | **Full test pass:** all automated tests, then flows F1–F7 (doc 02) by hand on the deployed app. | Everyone. Each person tests a different user's area. |
| R-4 | **NFR checklist:** each owner checks their NFRs. Lead: security, privacy, deployment. User 1: performance. User 2: reliability and safety. User 3: accessibility. | All |
| R-5 | Fix release bugs (label `type:bug`, milestone Release v1.0). | Area owner |
| R-6 | Secrets check: search the repo history for keys, and confirm `.env` and `secrets.toml` are not tracked. | Lead |
| R-7 | Deploy the final `main` to Streamlit Community Cloud. Tag `v1.0`. Write release notes listing the stories done. | Lead |
| R-8 | Final demo and retrospective. | All |

## 9. Meetings

| Meeting | When | Length | Purpose |
|---|---|---|---|
| **Kick-off** | Start of Phase 1 | 45 min | Walk through docs 00–03 and this plan. Answer the open questions. |
| **Stand-up** | Each class day | 10 min | Each person says what they did, what they will do next, and what is blocking them. Check for `blocked` issues. |
| **Foundation handover** | End of Phase 1 | 45 min | The Lead demonstrates the base: pages, services, tests, how to run everything. Phase 2 starts. |
| **Weekly demo** | Once a week in Phase 2 | 30 min | Show merged stories on the deployed app. |
| **Retrospective** | After each weekly demo | 15 min | One thing to keep, one to change. |
| **Release review** | End of Phase 3 | 45 min | Final demo, release notes, lessons learned. |

## 10. Risks

| # | Risk | Impact | What we do about it | Owner |
|---|---|---|---|---|
| R1 | The foundation takes longer than planned, so Users wait | High | Users have Phase 1 tasks (setup, reviews, test ideas, sketches). The Lead keeps foundation PRs small and merges them often. | Lead |
| R2 | Service stubs turn out to be wrong once real code is written | Medium | Users review L-09 before it merges. Changing a function's inputs or outputs needs an issue and the Lead's approval. | Lead |
| R3 | Merge conflicts | Medium | File ownership (section 6), one story at a time, update from `main` before each PR. | All |
| R4 | A secret key is committed | High | `.gitignore`, reviewer check, GitHub secret scanning, R-6. If a key leaks, **rotate it in Supabase immediately**. | Lead |
| R5 | Checkout (U2-04, size L) is late | High | U2's list puts it 4th. The Lead pairs on the `place_order` migration. | User 2 + Lead |
| R6 | Workload is uneven | Medium | At each weekly demo, someone who is ahead takes the next story at the **end** of another user's list (with the owner's agreement). | All |
| R7 | A user is absent | Medium | Their reviewer continues their list. | Reviewer |
| R8 | Real emails are sent by Supabase Auth | High | "Confirm email" is turned off in L-03/L-05 (Q2), and password reset is optional (Q4). | Lead |
| R9 | Inappropriate content | High | Content review of the seed data in L-06, display name blocklist in U2-12, sign-ups can be turned off. | Lead |
| R10 | Tests hit the demo database | High | The test guard from L-11 refuses to run against the demo URL. | Lead |
| R11 | Local setup problems (Python, PATH) | Medium | Setup is the first task in Phase 1. The README has setup steps. | Lead |
