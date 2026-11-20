# 00 – Consolidated Requirements

**Start here.** This page summarises docs 01–03 on one page. The detail lives in those documents:
[01 User stories](01-product-features-user-stories.md) · [02 UI/UX](02-ui-ux-requirements.md) · [03 Non-functional](03-non-functional-requirements.md)

| | |
|---|---|
| **Product** | Demo store for fictional AI and Data training courses |
| **Stack** | Streamlit (UI) + Supabase (database, sign-in, file storage), deployed to Streamlit Community Cloud |
| **Status** | Draft. Open questions (section 9) need a class decision. |

## 1. Goals

1. **Teach:** a complete, realistic example of building a web app with Streamlit and Supabase.
2. **Deploy:** a working example of deploying to Streamlit Community Cloud with secrets handled safely.
3. **Show good product practice:** clear user stories, a professional but gamified UI, and ethical design.
4. **Stay safe:** no real payments, no real emails, no real personal data, suitable for all ages.

**Vision:** *"Professional, yet gamified. It should create curiosity to buy the course."*

## 2. Users and roles

| Role | Can do | Inherits from |
|---|---|---|
| **Guest** | Browse, search, view courses and learning paths, use a cart | – |
| **Customer** | Check out, see orders and outbox, track learning, earn XP and badges | Guest |
| **Staff** | Add, edit, publish and unpublish courses, view all orders | Customer |
| **Owner** | Archive courses, give and remove the staff role | Staff |

## 3. Feature summary

| Epic | What it does | Stories | Must / Should / Could |
|---|---|---|---|
| **E1** Search and catalog | Browse, search, filter, sort and page through courses | US-101 – 105 | 2 / 1 / 2 |
| **E2** Product detail | Course details, outline, learning path, related courses | US-201 – 204 | 1 / 1 / 2 |
| **E3** Cart | Add and remove courses, keep the cart through sign-in, no buying a course twice | US-301 – 304 | 2 / 2 / 0 |
| **E4** Checkout | Demo order, confirmation, email saved to the outbox, order history | US-401 – 406 | 4 / 1 / 1 |
| **E5** Register / sign in | Accounts, sign in and out, role-based access | US-501 – 506 | 4 / 0 / 2 |
| **E6** Gamification | XP, levels, badges, skill tree, option to hide it all | US-601 – 606 | 3 / 3 / 0 |
| **E7** Product management | Course editor, images, modules, publishing, orders, team | US-701 – 708 | 4 / 3 / 1 |
| | | **39 stories** | **20 / 11 / 8** |

## 4. UI/UX summary

- **Principles:** clarity before delight · show the path, not the wall · reward learning, not spending · honest by default · calm celebration · user in control · built for Streamlit · accessible to everyone.
- **Look:** "a modern learning studio". Indigo primary colour. Gold (`color.xp`) is used **only** for XP, levels and badges. Inter font. Material Symbols icons.
- **Gamification:** an XP/level chip, toasts, one-time level-up messages, badges (never shown as "locked"), and a skill tree with text states. Everything can be hidden.
- **Never:** fake timers, fake scarcity, guilt messages, locks, XP for spending, leaderboards.
- **Screens:** 16 screens. S1–S8 are for shopping, S9–S12 for learning and account, S13–S15 for management, and S16 covers system states such as the demo banner, loading, empty, error, not found and access denied.
- **Streamlit limits:** no drag and drop, no sticky elements, no carousels, little animation. Use native components first.

## 5. Non-functional summary

48 NFRs (35 Must). The most important ones:

| Area | Key rules |
|---|---|
| **Security** | No secrets in the repo (SEC-01). RLS on every table (SEC-02). The app uses only the anon key (SEC-03). Roles enforced in the database (SEC-04). Prices taken from the database at order time (SEC-06). |
| **Privacy** | Collect only display name and email (PRIV-01). Progress is private (PRIV-02). No real data in seeds (PRIV-03). No tracking (PRIV-04). |
| **Safety** | Suitable for all ages (SAFE-01). No dark patterns (SAFE-02). No real payments or emails (SAFE-03). Demo banner on every page (SAFE-04). |
| **Performance** | Catalog in 3 seconds or less (PERF-01). Runs on free plans (PERF-04). |
| **Accessibility** | WCAG 2.2 AA (A11Y-01). Works at 375 px and 1280 px (A11Y-04). |
| **Reliability** | Orders are all-or-nothing (REL-01). No double orders (REL-02). XP awarded once only (REL-03). |
| **Code, testing and deployment** | Only `services/` talks to Supabase (MAINT-01). Database changes only through migrations (MAINT-02). Tests run against a separate test project (TEST-02). Deploy from `main` (DEP-01). |

## 6. Logical data model

All tables have RLS turned on. Money is stored as whole cents (`price_cents`) to avoid rounding errors. `auth.users` is managed by Supabase Auth.

| Table | Key fields | Notes |
|---|---|---|
| `profiles` | `id` (= auth user id), `display_name`, `role` (customer / staff / owner), `show_gamification`, `profile_completed_at` | One row per user. Only the owner can change `role`. |
| `categories` | `id`, `name`, `slug` | AI, Data, Python, Responsible AI |
| `learning_paths` | `id`, `name`, `slug`, `description`, `badge_id` | Data Explorer, AI Builder |
| `courses` | `id`, `slug`, `title`, `summary`, `description`, `category_id`, `level`, `price_cents`, `duration_hours`, `tags`, `image_path`, `status` (draft / published / archived), `learning_path_id`, `path_position`, `updated_at` | Guests see only `published` courses. |
| `course_prerequisites` | `course_id`, `prerequisite_id` | Advice only. No course can be its own prerequisite, and prerequisites cannot form a loop. |
| `modules` | `id`, `course_id`, `position`, `title`, `est_minutes`, `description` | 3–5 per course |
| `cart_items` | `user_id`, `course_id`, `added_at` | Unique per (user, course). Guest carts are kept in the session only (Q5). |
| `orders` | `id`, `order_number` (unique), `user_id`, `status` (paid_demo), `total_cents`, `created_at` | Created only by the `place_order` database function. |
| `order_items` | `order_id`, `course_id`, `title_at_purchase`, `price_cents_at_purchase` | Price copied at purchase time (US-702 AC2). |
| `library_items` | `user_id`, `course_id`, `order_id`, `acquired_at`, `completed_at` | Unique per (user, course), which prevents buying a course twice. |
| `module_progress` | `user_id`, `module_id`, `completed_at` | Unique per (user, module) |
| `xp_events` | `id`, `user_id`, `action` (module_complete / course_complete / profile_complete), `ref_id`, `xp`, `created_at` | Unique per (user, action, ref) so XP is only given once (REL-03) |
| `levels` | `level`, `name`, `min_xp` | 5 rows (Explorer → Mentor) |
| `badges` | `id`, `code`, `name`, `description`, `image_path` | 6 rows |
| `user_badges` | `user_id`, `badge_id`, `earned_at` | Unique per (user, badge) |
| `outbox` | `id`, `user_id`, `to_email`, `kind` (order_confirmation / …), `subject`, `body`, `order_id`, `created_at` | Never sent (SAFE-03) |

**Storage:** one `course-images` bucket. Anyone can view the images. Only staff and owner can upload (SEC-07).

**Main relationships:** a course has many modules and belongs to one category and at most one learning path. An order has many order items. A user has many library items, module progress rows, XP events and badges.

## 7. Scope

**In scope (v1):** every **Must** story (20) and **Must** NFR (35). **Should** items are built if time allows, in the order in section 10. **Could** items are stretch goals.

**Out of scope** (full list in doc 01, section 10): real payments, real emails, real course content, taxes, refunds, reviews, leaderboards, buying XP, social sign-in, two-factor authentication, multiple languages, mobile apps, analytics, production scaling.

## 8. Key terms

Course · Module · Learning path · Prerequisite (advice, not a lock) · Cart · Order · Library · Outbox (emails saved, never sent) · XP · Level · Badge · Skill tree · Role · RLS. Full definitions are in doc 01, section 8.

## 9. Open questions: decide as a class

Each question has a **default answer**. If the class agrees, write "Default" in the last column. If the class decides something different, write the new answer and update the linked document.

| # | Question | Default answer | Why the default | Class decision |
|---|---|---|---|---|
| Q1 | Which currency do prices use? | **USD**, display only | Simple, and every price is fictional anyway. | |
| Q2 | Supabase sends a real confirmation email on sign-up by default. Keep it? | **No. Turn off "Confirm email"** in Supabase Auth settings. | It keeps the demo rule of no real emails (SAFE-03). | |
| Q3 | Should learners sign up with real email addresses? | **No. Use made-up `@example.com` addresses.** | Protects learners' privacy, including younger ones (PRIV-05). Only works if Q2 = Default. | |
| Q4 | Password reset (US-505, Could) makes Supabase send a real email. Build it? | **Leave it out of v1.** The owner resets passwords in the Supabase dashboard. | It can't follow the outbox rule without extra work. | |
| Q5 | Where is a guest's cart stored? | **In the browser session only.** It is saved to the database after sign-in (US-303). | No database rows for anonymous users. Simpler RLS. | |
| Q6 | Do free ($0) courses go through checkout? | **Yes, the same flow** with a $0 total. | One flow to build and test. Every course gets an order record. | |
| Q7 | How are the first owner and staff accounts created? | **The seed script creates them,** using the service role key locally only. | The service role key never goes into the app (SEC-03). | |
| Q8 | Can anyone sign up on the public demo? | **Yes,** with the display name blocklist. The owner can turn off sign-ups in Supabase if it is misused. | Lets anyone try the demo, with a simple safety switch. | |
| Q9 | One Supabase project or two? | **Two:** "dev/test" and "demo" (DEP-03). | Tests never touch demo data. Fits the free plan, but check the current project limit. | |
| Q10 | Free projects pause or sleep when unused. Is that OK? | **Accept it.** Document how to wake them up, and show a friendly message (REL-06). | It's free, and fine for a demo. | |
| Q11 | How is the skill tree built? | **A static diagram (`st.graphviz_chart`) plus a clickable course list.** Vertical list on mobile. | Native, accessible, and quick to build. A custom component is a stretch goal. | |
| Q12 | Are the XP values and level thresholds in doc 01 right? | **Keep them** (+10 module, +50 course, +20 profile; levels at 0/100/300/600/1000 XP). | A full Data Explorer path reaches about Level 3, which feels like fair progress. | |
| Q13 | Dark mode in v1? | **Light theme only in v1.** Keep the dark tokens for later. | Halves the design and contrast checking. | |
| Q14 | Public or private GitHub repo? | **Public.** | Easy to share and deploy, and a good reason to practise SEC-01 and DEP-05. | |
| Q15 | Which Python version? | **3.12** (meets the 3.11+ requirement) | Current and well supported by Streamlit Community Cloud. | |

> If Q13 = Default, update doc 02 deliverable D1 and screen deliverables to say "light theme only for v1".

## 10. Suggested build order

Each phase ends with something you can demo. Deploy early and often.

| Phase | Build | Stories | Key NFRs |
|---|---|---|---|
| **0. Setup** | Supabase projects, Auth settings (Q2), secrets, a "Hello" page deployed to Streamlit Cloud | – | SEC-01, DEP-01, DEP-02, DEP-04 |
| **1. Data** | Migrations for all tables, RLS policies, storage bucket, seed script | – | SEC-02, MAINT-02, PRIV-03, REL-05 |
| **2. Browse** | Navigation, demo banner, Home, Catalog, Course detail, system states | US-101, 102, 201 | SAFE-04, PERF-01, PERF-03, A11Y-04 |
| **3. Accounts** | Register, sign in, sign out, role-based pages | US-501 – 504 | SEC-04, SEC-05, PRIV-01 |
| **4. Cart** | Add and remove courses, merge the cart on sign-in, owned check | US-301, 302, (303, 304) | REL-02 |
| **5. Checkout** | `place_order` function, checkout, confirmation, outbox, orders | US-401 – 404, (405) | SEC-06, REL-01, SAFE-03 |
| **6. Manage** | Course list, editor, image upload, publish | US-701 – 704, (705 – 707) | SEC-07, SEC-08 |
| **7. Learn and gamify** | Library, module progress, XP, levels, then badges, skill tree, hide option | US-605, 601, 602, (603, 604, 606) | REL-03, PRIV-02, SAFE-02 |
| **8. Polish** | Remaining Should and Could items, accessibility pass, tests, deployment guide | The rest | A11Y-01, TEST-03, MAINT-05 |

Stories in brackets are Should or Could items for that phase.

## 11. Traceability: features → stories → screens → NFRs

| Feature | Stories | Screens (doc 02) | NFRs (doc 03) |
|---|---|---|---|
| **E1** Search and catalog | US-101, 102, 103, 104, 105 | S1, S2, S16 | PERF-01, PERF-03, A11Y-02, A11Y-04, SAFE-01 |
| **E2** Product detail | US-201, 202, 203, 204 | S1, S3, S4, S16 | A11Y-03, PERF-01, SAFE-02 |
| **E3** Cart | US-301, 302, 303, 304 | S2, S3, S5, S6 | SEC-02, REL-02, PERF-02 |
| **E4** Checkout and confirmation | US-401, 402, 403, 404, 405, 406 | S6, S7, S8, S9, S12 | SEC-02, SEC-06, REL-01, REL-02, SAFE-03, PRIV-02 |
| **E5** Register and sign in | US-501, 502, 503, 504, 505, 506 | S6, S11, S16, navigation | SEC-02, SEC-04, SEC-05, SEC-08, PRIV-01, PRIV-05, SAFE-05 |
| **E6** Gamification | US-601, 602, 603, 604, 605, 606 | S1, S4, S9, S10, S11 | REL-03, PRIV-02, SAFE-02, A11Y-02 |
| **E7** Product management | US-701, 702, 703, 704, 705, 706, 707, 708 | S13, S14, S15 | SEC-04, SEC-07, SEC-08, PERF-03, REL-04 |
| **Across all features** | – | All, especially S16 | SEC-01, SEC-03, SAFE-01, SAFE-04, A11Y-01, A11Y-05, A11Y-06, REL-04, REL-05, REL-06, PERF-04, MAINT-01 – 05, TEST-01 – 04, DEP-01 – 05, PRIV-03, PRIV-04 |

The story-to-screen detail is in doc 02's appendix. The acceptance criteria are in doc 01.
