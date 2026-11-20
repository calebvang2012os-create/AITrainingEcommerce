# 02 – UI/UX Requirements

| | |
|---|---|
| **For** | UI/UX designer |
| **Product** | Demo store for fictional AI and Data training courses |
| **Built with** | Streamlit (UI) + Supabase (data, sign-in, files) |
| **Read with** | [01 – Product features and user stories](01-product-features-user-stories.md). Story IDs such as US-201 refer to that document. |
| **Status** | Draft |

## Before you start

- This is a **training demo**. There are no real payments and no real emails, and all courses are fictional. The UI must say this clearly (see S16).
- The audience includes **young learners**. All images, badge art, names and copy must be suitable for all ages.
- The app is built with **Streamlit**, which has real limits on layout and interaction. Read [section 10](#10-designing-for-streamlit) before you start on visual design. A design that cannot be built in Streamlit will be simplified by the developers, so it is better to design within the limits from the start.

---

## 1. The vision

> **"Professional, yet gamified. It should create curiosity to buy the course."**

| Part of the vision | What it means for the design | What it does NOT mean |
|---|---|---|
| **Professional** | Calm, clean layouts. Clear hierarchy. Prices, course details and the course outline are easy to find. The store looks trustworthy enough that a learner would spend their own money there. | Grey and corporate. Boring stock photos. Dense text. Jargon. |
| **yet** | Professional comes first. Gamification adds to a solid store and never takes it over. | Equal weight. The store is not a game with a shop attached. |
| **Gamified** | Learning progress is visible and rewarding: XP, levels, badges and a skill tree that shows where you are and where you can go next. Small, well-timed moments of recognition. | Casino style. Confetti everywhere. Loot boxes, spinning wheels, streaks you can lose, public leaderboards. Rewards for spending money. |
| **Create curiosity** | Show enough to make people want to know more: the course outline, what you will build, where a course leads in a learning path, free starter courses. The "what's next?" feeling. | Clickbait. Hidden prices. Teasers that mislead. Content held back to force a sign-up. |
| **to buy the course** | A clear and easy path from interest to purchase. **Add to cart** is always easy to find. Checkout has few steps. | Pressure. Fake timers, fake scarcity ("Only 2 left!"), guilt messages ("Don't fall behind!"), pre-ticked extras, pop-ups that interrupt browsing. |

**One-line test for every screen:** *Would a careful professional trust this page, and would a curious learner want to click one more thing?*

## 2. Design principles

1. **Clarity before delight.** Users must always know where they are, what something costs and what happens next. Delight comes after that.
2. **Show the path, not the wall.** Present learning as a map of possibilities. Never lock content or say "you can't".
3. **Reward learning, not spending.** XP and badges appear next to learning actions such as finishing modules and courses. They never appear next to prices or payments.
4. **Honest by default.** Real prices, honest copy, no invented urgency. Every claim in the UI must be true.
5. **Calm celebration.** Celebrate real milestones briefly, then get out of the way. One celebration at a time.
6. **User in control.** Users can remove anything from the cart, hide gamification, and find a way back from every screen.
7. **Built for Streamlit.** Use native components where possible. Custom styling is a finishing touch, not the base.
8. **Accessible to everyone.** WCAG 2.2 AA, works on a phone, and suitable for all ages.

## 3. Visual direction

### 3.1 Mood

**"A modern learning studio."** Think of a bright, well-organised workspace with a small trophy shelf in the corner.

- **Keywords:** confident, clear, curious, encouraging, modern.
- **Avoid:** neon gaming style, dark "hacker" style, cartoon overload, cold enterprise dashboards.
- **Illustration style (suggested):** simple flat or lightly shaded shapes that suggest data and AI, such as nodes, charts, grids and paths. No real brands or logos, and no photos of real people.

### 3.2 Suggested color tokens

These are **suggestions** for the designer to refine. Every text and background pair must be checked and must meet WCAG AA contrast: 4.5:1 for normal text and 3:1 for large text and UI elements.

| Token | Light | Dark | Use |
|---|---|---|---|
| `color.bg` | `#FFFFFF` | `#0F1420` | Page background |
| `color.surface` | `#F4F6FA` | `#1A2131` | Cards, sidebar, panels |
| `color.border` | `#DDE2EB` | `#2C3548` | Dividers, card borders |
| `color.text` | `#1A2233` | `#E8ECF4` | Main text |
| `color.text-muted` | `#5B6475` | `#A3ADC2` | Secondary text |
| `color.primary` | `#3949AB` | `#8C9EFF` | Buttons, links, focus (Streamlit `primaryColor`) |
| `color.xp` | `#F2A900` | `#FFC94D` | XP, levels and badges only. **Not for text on white.** Use it as a fill with dark text. |
| `color.success` | `#1E7F4F` | `#5CD394` | Completed states, success messages |
| `color.warning` | `#8A5A00` | `#FFB74D` | Warnings |
| `color.danger` | `#C62828` | `#FF8A80` | Errors, archive and delete actions |
| `color.info` | `#3949AB` | `#8C9EFF` | Info messages, the demo banner |

Rules:
- **`color.xp` belongs to gamification only.** Never use it on prices, sales or buy buttons. Keeping that rule teaches users that gold means learning progress, not shopping.
- Never show meaning with colour alone. Pair colour with an icon or a text label.

### 3.3 Typography

| Role | Suggestion | Notes |
|---|---|---|
| Headings and body | **Inter** (or another clear sans-serif from Google Fonts) | Must be available as a web font so it can be loaded in the Streamlit theme. |
| Numbers (XP, prices, order numbers) | Same font with tabular figures, or **JetBrains Mono** | Numbers should line up in tables. |
| Base size | 16 px body | Never smaller than 14 px. |

Suggested scale: 32 / 24 / 20 / 16 / 14 px. Keep to **two weights** (for example Regular and Semibold).
Streamlit controls a lot of the type styling itself, so treat this as the intent and expect small differences.

### 3.4 Icons

- Use **Google Material Symbols (Rounded)**. Streamlit supports these directly in buttons, page links, navigation and messages with the `:material/icon_name:` syntax, so no custom work is needed.
- In the handoff, list each icon by its **Material Symbols name** (for example `shopping_cart`, `school`, `military_tech`, `account_tree`).
- **Badges** are the one place for custom artwork (see section 11).
- Avoid emoji as the main icon system because they look different on each device. Emoji are fine in friendly copy.

## 4. How gamification appears in the UI

### 4.1 Where it appears

| Element | Where | How |
|---|---|---|
| **XP and level chip** | Navigation, for signed-in customers | Compact: level name + XP progress, e.g. "Lv 2 · Apprentice · 140/300 XP". Selecting it opens S11. |
| **XP toast** | After a learning action (S10) | A small, short message such as "+10 XP · Module complete". It closes by itself. |
| **Level-up message** | After a level-up (S10, S11) | One message with the new level name and a close button. Shown once only (US-602). |
| **Badges** | S11, plus a small row on S9 | Earned badges in full colour. Unearned badges greyed out with a "How to earn" hint. Never shown as "locked". |
| **Progress bars** | S9, S10, S11 | Course progress (modules done out of total) and level progress. |
| **Skill tree** | S4, plus a mini view on S3 | A learning path with each course's state as text: Not started, In progress, Completed (US-604). |
| **Course XP hint** (Could) | S3 | "Finish this course to earn up to 90 XP". Placed in the outline section, **never next to the price**. |

### 4.2 Ethical rules (required)

| Allowed | Not allowed |
|---|---|
| Showing honest progress | Countdown timers or "offer ends soon" |
| Celebrating real milestones once | Fake scarcity ("only 3 seats left") |
| Suggesting a next course in a learning path | Guilt or loss messages ("Your streak will break!") |
| Showing unearned badges with how to earn them | Locked content or padlock icons on courses |
| Letting users hide gamification (US-606) | XP for buying, or XP for sale |
| Keeping progress private | Leaderboards or comparing users |
| Short celebration messages | Full-screen animations, sounds, or repeated pop-ups |

**Hidden mode (US-606):** when gamification is hidden, remove the XP chip, toasts, level-up messages, badges and skill tree states. The rest of the store must still look complete with no empty gaps. Please design this state.

## 5. Sitemap and navigation

```
Store (everyone)
├── S1  Home
├── S2  Courses (catalog and search)
│   └── S3  Course detail
├── S4  Learning paths (skill tree)
├── S5  Cart
└── S6  Sign in / Register

Checkout (signed in)
├── S7  Checkout
└── S8  Order confirmation

My learning (customer)
├── S9  My learning (library)
│   └── S10 Course progress
├── S11 Profile and progress (XP, level, badges, settings)
└── S12 My orders and outbox

Manage (staff and owner)
├── S13 Manage courses
│   └── S14 Course editor
└── S15 Orders and team   (Team tab: owner only)

S16 System states (shown across all screens)
```

### Navigation rules

- **Primary navigation:** Home · Courses · Learning paths · Cart (with count) · Sign in or account menu.
- **Account menu (signed in):** My learning · Profile and progress · My orders · Sign out (US-503).
- **Manage section:** only visible to staff and owner (US-504). Team is only visible to the owner.
- **Demo banner:** a thin bar on every page that says "Demo store: no real payments or emails" (S16).
- **Back paths:** every detail page links back to its list, for example "← All courses".
- Each page has its own URL, so people can share and bookmark pages.
- See section 10 for how Streamlit navigation works (sidebar or top bar).

## 6. Key user flows

### F1 Guest discovers and buys (main flow)
S1 Home → S2 Courses (search or filter) → S3 Course detail → **Add to cart** → S5 Cart → **Checkout** → S6 Sign in / Register → S7 Checkout → **Place order** → S8 Confirmation → S9 My learning
*Stories: US-101, 102, 201, 301, 302, 401, 402, 403, 404*

Design notes: the price is visible at every step. The cart is kept through sign-in (US-303). After sign-in, the user goes straight back to checkout.

### F2 Curiosity through a learning path
S1 Home (learning path teaser) → S4 Learning paths → select a course in the tree → S3 Course detail → Add to cart
*Stories: US-203, 604, 301*

### F3 Learn and level up
S9 My learning → S10 Course progress → **Mark module complete** → XP toast → (sometimes) level-up message → S11 Profile shows the new level or badge
*Stories: US-601, 602, 603, 605*

### F4 Returning customer checks an order
Account menu → S12 My orders → order detail → Outbox tab to read the confirmation email
*Stories: US-405, 406*

### F5 Staff adds and publishes a course
S13 Manage courses → **New course** → S14 Editor (details, image, modules, learning path) → Save as draft → Preview → **Publish** → course appears in S2
*Stories: US-701, 703, 706, 704*

### F6 Owner archives a course and manages the team
S13 → Archive (with a confirmation dialog) · S15 Team tab → change a user's role
*Stories: US-705, 708*

### F7 Hide gamification
S11 → Settings → "Show XP, levels and badges" switched off → gamification disappears from every screen
*Story: US-606*

## 7. Screen-by-screen requirements

For each screen: purpose, linked stories, required content, states, and notes. "States" refers to the patterns in S16.

### S1 Home
**Purpose:** Make a strong first impression, explain the store in seconds, and spark curiosity.
**Stories:** US-101, US-102, US-203, US-604

Required content:
- **Hero:** a clear value line (e.g. "Learn AI and Data, one skill at a time"), a search box (US-102) and a main button "Browse courses".
- **Learning paths teaser:** 2 cards (Data Explorer, AI Builder), each with a mini path preview and "Explore path" → S4.
- **Featured courses:** 3–6 course cards. Include at least one free course as an easy start.
- **"How it works":** 3 steps: Pick a course → Learn → Earn XP and badges.
- For signed-in customers, also: "Continue learning" (the last course in progress) and a suggested next step.

States: loading, no published courses (US-101 AC4).

### S2 Courses (catalog and search)
**Purpose:** Find the right course quickly.
**Stories:** US-101, US-102, US-103, US-104, US-105, US-301

Required content:
- Search box, then filters (category, level, price range), sort, and "Clear filters".
- A results count, e.g. "8 courses".
- **Course card** (a reusable component): image, title, category, level, duration, price ("Free" when $0), short summary, and a button showing **Add to cart**, **In cart** or **Owned**.
- Pagination: 12 courses per page.

States: loading, no results (with "Clear search"), empty catalog, error.
Notes: on mobile the filters fold into an expandable section above the results. Cards go from 3 columns to 1.

### S3 Course detail
**Purpose:** Turn interest into a decision. This is the most important screen for "curiosity to buy".
**Stories:** US-201, US-202, US-203, US-204, US-301, US-304

Required content:
- **Header:** title, category, level, duration, price, and the main button (**Add to cart**, **In cart** or **Owned → Go to course**).
- **"What you'll learn":** 3–5 bullet points.
- **"What you'll build":** one sentence about a project, to spark curiosity.
- **Course outline:** modules with titles and times (US-202). Use expandable sections.
- **Learning path:** a mini skill tree showing this course, what comes before it (prerequisites, as advice only) and what it leads to (US-203).
- **Related courses:** up to 3 cards (US-204).
- Course XP hint (Could), in the outline section only.

States: loading, course not found or archived ("Course not found" + "← All courses", US-201 AC3).
Notes: the price and main button should stay visible without scrolling far on desktop. On mobile, repeat the main button after the outline.

### S4 Learning paths (skill tree)
**Purpose:** Show the big picture and invite exploration.
**Stories:** US-203, US-604

Required content:
- A path selector (Data Explorer / AI Builder).
- A skill tree: courses as nodes, with lines from each prerequisite to the next course. Each node shows the **title and state as text** (Not started / In progress / Completed) and links to S3.
- For guests: the same tree with no progress states, plus a gentle prompt: "Sign in to track your progress."
- A path summary: number of courses, total hours, and the badge earned for finishing the path.

Notes: **no locks**. Suggest an order, but never force it. See section 10 for the build options; please design a mobile version.

### S5 Cart
**Purpose:** Review the cart and continue.
**Stories:** US-302, US-303, US-304

Required content: a list of items (image, title, price, **Remove**), the total, **Checkout**, and "Continue browsing".
States: empty cart ("Your cart is empty" + link to S2), item already owned (removed automatically, with a short notice).
Notes: no cross-sell pop-ups. A quiet "You might also like" row is fine.

### S6 Sign in / Register
**Purpose:** Get in quickly, especially in the middle of checkout.
**Stories:** US-401, US-501, US-502, US-505, US-303

Required content:
- Two tabs: **Sign in** and **Create account**.
- Sign in: email, password, "Forgot password?" (US-505).
- Create account: display name, email, password (at least 8 characters, shown before the user starts typing).
- When reached from checkout, show "Sign in to complete your order. Your cart is saved."

States: wrong details ("Email or password is incorrect", US-502 AC2), email already registered, password too short, loading, reset link "sent" (explain it went to the demo outbox).

### S7 Checkout
**Purpose:** Confirm and place the demo order.
**Stories:** US-401, US-402

Required content: order summary (items and total), the notice **"Demo store: no real payment is taken"**, and **Place order**.
Must NOT include: any card, bank or address fields (US-402 AC2).
States: placing the order (loading, button disabled to stop double clicks), error with a retry option.

### S8 Order confirmation
**Purpose:** Reassure the user and point them to the next step.
**Stories:** US-403, US-404

Required content: a success message, order number, date, items, total, "A confirmation email was saved to your demo outbox", and the buttons **Start learning** (→ S9) and **View my orders** (→ S12).
Notes: a short, calm success moment is fine. No XP is given for buying (US-601 AC4).

### S9 My learning (library)
**Purpose:** The customer's home base.
**Stories:** US-402 (AC5), US-605, US-603

Required content: owned courses as cards, each with a progress bar (e.g. "2 of 5 modules") and **Continue**, filter tabs (In progress / Not started / Completed), and a small row of recent badges.
States: no courses yet ("Your library is empty. Find your first course" → S2, and suggest a free course).

### S10 Course progress
**Purpose:** Track learning inside one owned course.
**Stories:** US-605, US-601, US-602

Required content: course title, progress bar, a list of modules with a **Mark complete** action, and a "Course complete!" state with the next suggested course from the learning path.
Feedback: XP toast on completion, a level-up message when it happens, and a course-complete moment. Each appears once.
Note: course content is fictional, so each module shows a short placeholder description only.

### S11 Profile and progress
**Purpose:** See your progress and control your settings.
**Stories:** US-602, US-603, US-606, US-506, US-601, US-503

Required content:
- **Level card:** level number and name, XP total, progress to the next level.
- **Badges grid:** all 6 badges, earned and not yet earned, each with a "How to earn" hint.
- **XP history:** a simple list of action, XP and date (US-601 AC3).
- **Settings:** display name (US-506), the "Show XP, levels and badges" switch (US-606), and **Sign out**.

States: gamification hidden (show only profile and settings), name rejected by the blocklist (US-506 AC2).

### S12 My orders and outbox
**Purpose:** See past purchases and demo emails.
**Stories:** US-405, US-406, US-404

Required content: two tabs.
- **Orders:** a table of order number, date, item count, total and status "Paid (demo)". Selecting a row opens order details.
- **Outbox:** a list of messages (subject, date) that open to the full text. A banner explains: "These emails were not really sent. This is a demo."

States: no orders, no messages, access denied (another user's order, US-405 AC2).

### S13 Manage courses (staff and owner)
**Purpose:** See and manage every course.
**Stories:** US-704, US-705, US-707

Required content: a table of title, category, price, status (Draft / Published / Archived), last updated, and actions (Edit, Publish/Unpublish, Archive for the owner only). Also a status filter and **New course**.
Archiving opens a confirmation dialog that explains the course stays in past orders. There is no delete option for courses that appear in orders (US-705 AC3).

### S14 Course editor (staff and owner)
**Purpose:** Create and edit a course.
**Stories:** US-701, US-702, US-703, US-704, US-706

Required content in sections or tabs:
- **Details:** title*, summary, description, category*, level*, price*, duration, tags.
- **Image:** upload (PNG, JPG or WEBP, up to 2 MB) with a preview and the placeholder image as a fallback.
- **Modules:** a list with add, edit, remove and move up/down (drag and drop is not available, see section 10).
- **Learning path:** a path selector and prerequisites as a multi-select, with an error for loops (US-706 AC3).
- **Status and actions:** Save draft · Preview (opens S3 view) · Publish.

States: validation errors next to each field (US-701 AC3), uploading, saved message, unsaved changes warning.

### S15 Orders and team (staff and owner)
**Purpose:** Help customers, and let the owner manage staff.
**Stories:** US-707, US-708

Required content:
- **Orders tab:** a read-only table of all orders, with search by order number or customer.
- **Team tab (owner only):** a table of users and roles, with a role selector (Customer / Staff). The owner's own row cannot be changed (US-708 AC2).

### S16 System states (all screens)
**Purpose:** One consistent pattern for every state that is not the "happy path".
**Stories:** US-101 (AC4), US-201 (AC3), US-302 (AC4), US-504, plus all error criteria

| State | Pattern needed |
|---|---|
| **Demo banner** | Always visible, thin, info colour, not dismissable |
| **Loading** | Skeleton cards for lists, a spinner for actions |
| **Empty** | Small illustration + one line + one helpful action |
| **No results** | Same as empty, with "Clear search" |
| **Error** | What happened in plain English + "Try again" |
| **Not found** | "We couldn't find that page" + link home |
| **Access denied** | "You do not have access to this page" + link home (US-504 AC1) |
| **Signed out** (session ended) | Message + Sign in button |
| **Success** | Toast for small actions, a message box for big ones |
| **Confirm dialog** | Title, consequence in one sentence, Cancel + main action (danger colour for archiving) |
| **Form validation** | An error under the field, with the field name in the message |

## 8. Accessibility and mobile

### Accessibility (target: WCAG 2.2 AA)
- Contrast: 4.5:1 for text and 3:1 for large text, icons and focus indicators. Check both light and dark themes.
- Never show meaning only with colour. States such as Completed, In cart and Draft always have text.
- Every image needs alt text. Course images get descriptive alt text; decorative images get none.
- Keyboard: every action can be reached and used with the keyboard, and focus is always visible.
- Use real headings in order (H1 → H2 → H3). One H1 per page.
- Motion: no flashing. Avoid Streamlit's built-in balloons and snow effects, because they cannot respect the user's "reduce motion" setting.
- Toasts must not be the only place where important information appears.
- Plain language suitable for younger readers (see section 9).

### Mobile
- Design at **375 px** (phone) and **1280 px** (desktop). Show the tablet size only if the layout changes.
- Streamlit stacks columns on narrow screens. Design the stacked order on purpose: the most important content comes first.
- Touch targets should be at least 44 × 44 px where the design controls them.
- Tables (S12, S13, S15) need a mobile pattern: fewer columns, with details on selection.
- The skill tree must have a mobile version, such as a vertical list of steps.

## 9. Content and tone

**Voice:** a friendly, expert guide. Clear, warm and encouraging. Never pushy.

- Use simple, short sentences and everyday words. Explain technical terms the first time they appear.
- Address the user as "you".
- Celebrate effort and progress, not money spent.
- Every page must make sense to a 12-year-old *and* not talk down to an adult.

| Do | Don't |
|---|---|
| "Add to cart" | "Grab it before it's gone!" |
| "Free" | "FREE!!! Limited time" |
| "Great work, module complete. +10 XP" | "You're crushing it, legend!!!" |
| "Not started yet" | "Locked" |
| "Finish this course to earn the AI Builder badge" | "Don't miss out on the badge everyone has!" |
| "Email or password is incorrect" | "Invalid credentials (401)" |
| "Something went wrong. Please try again." | "Oops! Our bad 🙈" (as the only message) |

**Copy the designer must write or confirm:** hero line, empty states, error messages, button labels, demo notices, level names and badge descriptions (start from doc 01, section 9).

## 10. Designing for Streamlit

Streamlit builds pages from ready-made components. It is quick to build with, but layout and interaction are limited. Use this table to keep designs buildable. Feature availability depends on the Streamlit version, so the developers will confirm details during handoff.

### Works well (design freely)

| Need | Streamlit feature |
|---|---|
| Multi-page app with sections | `st.navigation` with grouped pages (shown in the sidebar, or as a top bar in recent versions) |
| Links between pages | `st.page_link` |
| Grids and side-by-side content | `st.columns` (fixed column ratios) |
| Cards and panels | `st.container(border=True)` |
| Tabs and collapsible sections | `st.tabs`, `st.expander` |
| Dialogs (confirm, quick view) | `st.dialog` |
| Small menus | `st.popover` |
| Toast messages | `st.toast` |
| Info, success, warning and error boxes | `st.info`, `st.success`, `st.warning`, `st.error` |
| Forms with a submit button | `st.form` |
| Progress bars and numbers | `st.progress`, `st.metric` |
| Icons in labels | Material Symbols (`:material/icon_name:`) |
| Tables | `st.dataframe` (sortable, can select rows) |
| Images and uploads | `st.image`, `st.file_uploader` |
| Light and dark themes, colours, fonts, corner radius | Theme settings in `.streamlit/config.toml` |
| Skill tree (simple) | `st.graphviz_chart`: a clear, static diagram. Nodes can't be clicked, so add links below it. |

### Possible with extra effort (use sparingly and mark clearly in the handoff)

| Need | How | Limit |
|---|---|---|
| Custom card styles, shadows, hover effects | Custom CSS, targeting containers by their key | Can break when Streamlit updates. Keep it small. |
| A custom hero section | HTML or Markdown block | Not interactive, so buttons must be normal Streamlit buttons |
| An interactive, clickable skill tree | A custom component | Much more developer time. A simple version (static diagram + course list) is preferred. |

### Not possible or not practical (please avoid)

- **Pixel-perfect layouts.** Spacing, widths and breakpoints are mostly controlled by Streamlit.
- **Drag and drop** (for example reordering modules). Use Move up / Move down buttons instead.
- **Sticky or floating elements**, such as a floating cart button or a sticky price bar.
- **Complex animations, page transitions** or hover-only interactions. Every interaction reloads part of the page.
- **Changing native widgets' internal styles in detail**, such as the inner parts of a slider or select box.
- **Carousels or sliders** for images. Use a grid instead.
- **Mega menus** or multi-level dropdown navigation.
- **Hover-only information.** Hover doesn't exist on touch screens, and Streamlit offers little control over it.

**Rule of thumb:** if a component is not in the "Works well" table, add a note in the design explaining why it is worth the extra effort, and offer a simpler fallback.

## 11. Designer deliverables

Hand over the following. Use one design file (Figma or similar) unless stated.

| # | Deliverable | Details |
|---|---|---|
| D1 | **Design tokens** | Final colour tokens (light and dark), font choices and type scale, spacing and corner radius. Provide the values ready for the Streamlit theme (`primaryColor`, background, secondary background, text colour, font). |
| D2 | **Component sheet** | Course card, XP/level chip, badge (earned / not earned), progress bar, path node (3 states), demo banner, empty state, error state, confirm dialog, toast. **Label each with the Streamlit feature it uses** (section 10). |
| D3 | **Screens S1–S16** | Desktop (1280 px) and mobile (375 px), including all listed states, signed-out and signed-in versions where they differ, and **gamification hidden** versions for S1, S3, S9, S10 and S11. |
| D4 | **Clickable prototype** | Flows F1 (guest buys) and F3 (learn and level up) at least. |
| D5 | **Flow diagrams** | F1–F7 as simple diagrams. |
| D6 | **Icon list** | Every icon with its Material Symbols name and where it is used. |
| D7 | **Badge artwork** | 6 badges (doc 01, section 9) in earned and not-earned versions. SVG plus PNG at 256 × 256. Suitable for all ages. |
| D8 | **Course images** | 10 images, one per seed course, 16:9 at 1200 × 675, PNG/JPG/WEBP under 2 MB each (US-703). One placeholder image too. Fictional only: no real logos, brands or photos of real people. |
| D9 | **Logo and favicon** | A simple, fictional store name and logo, with the favicon at 32 × 32 and 512 × 512 PNG. |
| D10 | **Copy deck** | All UI text from section 9, in a table: screen, element, text. |
| D11 | **Accessibility notes** | Contrast check results, focus order for key screens, and alt text for all images. |
| D12 | **Handoff notes** | Anything that needs custom CSS or a custom component, why it is needed, and its simpler fallback. |

### Definition of done for the design
- Every screen S1–S16 links to its user stories, and every story in doc 01 appears on at least one screen (see the appendix).
- No dark patterns from section 4.2 appear anywhere.
- All colour pairs pass WCAG AA in both themes.
- Developers have reviewed the designs against section 10.

---

## Appendix: User story to screen map

| Story | Screen(s) | Story | Screen(s) |
|---|---|---|---|
| US-101 | S1, S2, S16 | US-501 | S6 |
| US-102 | S1, S2 | US-502 | S6 |
| US-103 | S2 | US-503 | Navigation, S11 |
| US-104 | S2 | US-504 | Navigation, S16 |
| US-105 | S2 | US-505 | S6 |
| US-201 | S3, S16 | US-506 | S11 |
| US-202 | S3 | US-601 | S10, S11 |
| US-203 | S1, S3, S4 | US-602 | S10, S11 |
| US-204 | S3 | US-603 | S9, S11 |
| US-301 | S2, S3 | US-604 | S1, S4 |
| US-302 | S5, S16 | US-605 | S9, S10 |
| US-303 | S5, S6 | US-606 | S11 (+ hidden versions) |
| US-304 | S3, S5 | US-701 | S14 |
| US-401 | S6, S7 | US-702 | S14 |
| US-402 | S7, S9 | US-703 | S14 |
| US-403 | S8 | US-704 | S13, S14 |
| US-404 | S8, S12 | US-705 | S13 |
| US-405 | S12 | US-706 | S14 |
| US-406 | S12 | US-707 | S13, S15 |
| | | US-708 | S15 |
