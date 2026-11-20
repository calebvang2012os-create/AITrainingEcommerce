# 01 – Product Features and User Stories

| | |
|---|---|
| **Product** | Demo e-commerce store for fictional AI and Data training courses |
| **Stack** | Streamlit (UI) + Supabase (database, sign-in, file storage) |
| **Status** | Draft |

## 1. Purpose

This store is a **training demo**. It shows how to build and deploy a small online shop
with Streamlit and Supabase.

Important rules for the whole app:

- **No real payments.** Checkout is simulated. No card details are ever asked for or stored.
- **No real emails.** Emails are saved to an **outbox** table in the database. Nothing is sent.
- **All courses are fictional.** Buying one does not give access to real training.
- **Safe for all ages.** Content, images and seed data must be suitable for young learners.

## 2. Roles

| Role | Who they are | What they can do |
|---|---|---|
| **Guest** | Anyone not signed in | Browse and search the catalog, view course details, use a cart |
| **Customer** | A signed-in user | Everything a guest can do, plus check out, see their orders, and earn XP and badges |
| **Staff** | A team member | Everything a customer can do, plus add and edit courses and view all orders |
| **Owner** | The store owner | Everything staff can do, plus archive courses and manage staff accounts |

Each role includes the permissions of the roles above it in this table.

## 3. How to read this document

- Each feature area is an **epic** (E1–E7).
- Each user story has an **ID**. The first digit matches the epic, so US-1xx is in E1 and US-2xx is in E2.
- **Priority:**
  - **Must:** needed for the first working demo.
  - **Should:** important, but the demo still works without it.
  - **Could:** nice to have if there is time.
- **Acceptance criteria** (AC1, AC2, …) say how we check that a story is done.

---

## E1 – Product search and catalog

### US-101 Browse the catalog — **Must**
As a **guest**, I want to see a list of all courses, so that I can find something to learn.

- AC1: The catalog page shows every course with status **Published**.
- AC2: Each course card shows the title, image, category, level, price and a short summary.
- AC3: Draft and archived courses are not shown.
- AC4: If there are no courses, the page shows "No courses available yet."

### US-102 Search by keyword — **Must**
As a **guest**, I want to search by keyword, so that I can quickly find a topic.

- AC1: A search box searches the course title, summary and tags.
- AC2: The search ignores upper and lower case.
- AC3: If nothing matches, the page shows "No courses match your search" and a way to clear it.

### US-103 Filter the catalog — **Should**
As a **guest**, I want to filter by category, level and price, so that I only see courses that suit me.

- AC1: Filters for category (for example AI, Data, Python) and level (Beginner, Intermediate, Advanced).
- AC2: A price range filter.
- AC3: Filters can be combined with search.
- AC4: A "Clear filters" button resets everything.

### US-104 Sort the catalog — **Could**
As a **guest**, I want to sort courses, so that I can compare them more easily.

- AC1: Sort options: Newest, Price low to high, Price high to low, Title A–Z.
- AC2: The default sort is Newest.

### US-105 Page through results — **Could**
As a **guest**, I want results split into pages, so that the list stays easy to read.

- AC1: At most 12 courses are shown on each page.
- AC2: Next and Previous controls appear only when there is more than one page.

---

## E2 – Product detail

### US-201 View course details — **Must**
As a **guest**, I want to open a course and read about it, so that I can decide whether to buy it.

- AC1: The detail page shows the title, image, full description, category, level, duration (hours), price and what you will learn.
- AC2: The page has an **Add to cart** button.
- AC3: If someone opens the page of a course that does not exist or is archived, they see "Course not found" and a link back to the catalog.

### US-202 See the course outline — **Should**
As a **guest**, I want to see the list of modules, so that I know what the course covers.

- AC1: Modules are listed in order with a title and an estimated time.

### US-203 See where a course fits in a learning path — **Could**
As a **guest**, I want to see which learning path a course belongs to, so that I know what to take before and after it.

- AC1: The page shows the learning path name and any **prerequisite** courses.
- AC2: Each prerequisite links to its own detail page.
- AC3: Prerequisites are advice only. A course can be added to the cart even if the user does not own its prerequisites.

### US-204 See related courses — **Could**
As a **guest**, I want to see similar courses, so that I can explore more.

- AC1: Up to 3 published courses from the same category are shown. The current course is not included.

---

## E3 – Add to cart

### US-301 Add a course to the cart — **Must**
As a **guest**, I want to add a course to my cart, so that I can buy it later.

- AC1: Clicking **Add to cart** adds the course and shows a message that names the course, for example "Data Cleaning Workshop was added to your cart."
- AC2: A course can only be in the cart once, because each course is a single digital item. If it is already there, the button reads "In cart".
- AC3: A cart icon or counter shows how many items are in the cart.

### US-302 View and edit the cart — **Must**
As a **guest**, I want to see my cart and remove items, so that I stay in control of what I buy.

- AC1: The cart page lists each course with its title and price.
- AC2: Each item has a **Remove** button.
- AC3: The cart shows the total price.
- AC4: An empty cart shows "Your cart is empty" and a link to the catalog.

### US-303 Keep the cart after signing in — **Should**
As a **customer**, I want the cart I filled as a guest to stay after I sign in, so that I do not lose my choices.

- AC1: When a guest signs in, the items in their guest cart are merged into their saved cart.
- AC2: The merged cart has no duplicate courses.

### US-304 Prevent buying a course twice — **Should**
As a **customer**, I want to be warned if I already own a course, so that I do not buy it twice by mistake.

- AC1: If a signed-in customer already owns a course, the button reads "Owned" and the course cannot be added to the cart.

---

## E4 – Checkout and order confirmation

### US-401 Sign in before checkout — **Must**
As a **guest**, I want to be asked to sign in at checkout, so that my order is saved to my account.

- AC1: When a guest clicks **Checkout**, they are sent to sign in or register.
- AC2: After signing in, they are taken back to checkout with their cart kept.

### US-402 Place a demo order — **Must**
As a **customer**, I want to place an order, so that I can complete my purchase.

- AC1: The checkout page shows the items, the total and the notice: "Demo store: no real payment is taken."
- AC2: Checkout never asks for card or bank details.
- AC3: Clicking **Place order** creates an order with status **Paid (demo)** and a unique order number.
- AC4: The cart is emptied after the order is placed.
- AC5: The courses in the order are added to the customer's library.

### US-403 See the order confirmation — **Must**
As a **customer**, I want a confirmation page, so that I know my order worked.

- AC1: The page shows the order number, date, items and total.
- AC2: The page has a link to "My orders".

### US-404 Confirmation email saved to the outbox — **Must**
As the **owner**, I want order emails saved instead of sent, so that the demo never emails real people.

- AC1: Each order creates one row in the **outbox** with the recipient, subject, body and time created.
- AC2: The app has no email service set up. Placing an order makes no outgoing email request.
- AC3: The email body includes the order number, the items and the total.

### US-405 View my orders — **Should**
As a **customer**, I want to see my past orders, so that I can check what I bought.

- AC1: "My orders" lists my orders, newest first, with the order number, date and total.
- AC2: A customer cannot see another customer's order, even by opening that order's link directly.

### US-406 View my outbox emails — **Could**
As a **customer**, I want to read the emails "sent" to me, so that I can see what a real store would send.

- AC1: An "Outbox" page shows only my own outbox messages.

---

## E5 – Customer register and sign in

### US-501 Register an account — **Must**
As a **guest**, I want to create an account, so that I can buy courses and track my progress.

- AC1: The form asks for a display name, email and password.
- AC2: The password must be at least 8 characters long.
- AC3: Using an email that is already registered shows a clear error.
- AC4: New accounts get the **customer** role.
- AC5: Accounts are created with Supabase Auth. No app table has a password column.

### US-502 Sign in — **Must**
As a **customer**, I want to sign in, so that I can see my cart, orders and progress.

- AC1: Signing in with a correct email and password works.
- AC2: A wrong email or password shows "Email or password is incorrect". The message does not say which one was wrong.
- AC3: After signing in, the page shows the user's display name.

### US-503 Sign out — **Must**
As a **customer**, I want to sign out, so that others using the same computer cannot use my account.

- AC1: **Sign out** ends the session and returns the user to the catalog as a guest.

### US-504 Pages match the role — **Must**
As the **owner**, I want each role to see only what it is allowed to see, so that the store stays safe.

- AC1: Management pages do not appear in the menu for guests and customers. Opening one of their links directly shows "You do not have access to this page."
- AC2: The database enforces the same rules with Supabase Row Level Security. A customer who calls the database directly with their own session cannot add, change or archive a course.

### US-505 Reset my password — **Could**
As a **customer**, I want to reset a forgotten password, so that I can get back into my account.

- AC1: The reset link is written to the outbox, not sent by email.

### US-506 Edit my profile — **Could**
As a **customer**, I want to change my display name, so that it shows the name I prefer.

- AC1: The display name can be changed and is saved.
- AC2: A display name that contains a word from the app's blocklist is not saved, and the user is asked to choose a different name.

---

## E6 – Gamification

### Ethical design rules (apply to every story in E6)

- **No pressure tactics.** No countdown timers, fake scarcity ("only 2 left!"), guilt messages or nagging pop-ups.
- **No penalties.** You never lose XP, levels or badges, and missing a day has no penalty.
- **XP rewards learning, not spending.** XP comes from learning actions. Spending more money never earns more XP.
- **XP cannot be bought.**
- **Opt-out.** Users can hide all gamification.
- **Private by default.** Progress is not shown to other users.

### US-601 Earn XP for learning actions — **Must**
As a **customer**, I want to earn XP when I learn, so that I can see my effort add up.

- AC1: XP is given for: finishing a module (+10), finishing a course (+50) and completing my profile once (+20).
- AC2: Each action gives XP only once. Repeating it does not give XP again.
- AC3: Every XP gain is recorded in an XP history (action, amount, date).
- AC4: Buying a course gives 0 XP.

### US-602 Level up — **Must**
As a **customer**, I want to reach new levels, so that I can see my progress over time.

- AC1: Levels are based on total XP (see the table in section 9).
- AC2: My current level and the XP needed for the next level are shown on my profile.
- AC3: Reaching a new level shows a message naming the new level, for example "You reached Level 2: Apprentice." The message has a close button and does not appear again after it is closed.

### US-603 Earn badges — **Should**
As a **customer**, I want to earn badges for milestones, so that my achievements are recognised.

- AC1: Badges are given automatically when their rule is met (see section 9).
- AC2: My profile shows earned badges. Badges I have not earned yet are greyed out, with a hint on how to earn them.
- AC3: A badge can only be earned once.

### US-604 Learning-path skill tree — **Should**
As a **customer**, I want to see a skill tree for a learning path, so that I know what to learn next.

- AC1: The skill tree shows the courses in a path as nodes, with lines from each prerequisite to the course after it.
- AC2: Each node shows one of these states: **Not started**, **In progress**, **Completed**.
- AC3: Nodes are never locked. The tree suggests an order but does not force it.
- AC4: Each node shows its course title and its state as text, so the tree can be read without relying on colour.

### US-605 Mark a module as complete — **Must**
As a **customer**, I want to mark modules of my owned courses as complete, so that I can track my learning.

- AC1: Only modules in owned courses can be marked complete.
- AC2: When all modules in a course are complete, the course is marked complete.

### US-606 Hide gamification — **Should**
As a **customer**, I want to turn off XP, levels and badges, so that I can learn without them.

- AC1: A setting in my profile hides the XP total, level, badges, skill tree progress and level-up messages.
- AC2: XP is still recorded while gamification is hidden. When it is turned back on, the total includes XP earned while it was hidden.

---

## E7 – Owner and staff product management

### US-701 Add a course — **Must**
As **staff**, I want to add a new course, so that the catalog can grow.

- AC1: The form has fields for title, summary, description, category, level, price, duration, tags and image.
- AC2: Title, category, level and price are required. Price must be 0 or more.
- AC3: If a required field is empty or the price is below 0, an error names the field and the course is not saved.
- AC4: New courses start as **Draft**.

### US-702 Edit a course — **Must**
As **staff**, I want to edit a course, so that its details stay correct.

- AC1: All fields from US-701 can be edited.
- AC2: Changes to the price do not change the price on orders already placed.

### US-703 Upload a course image — **Must**
As **staff**, I want to upload an image for a course, so that the catalog looks good.

- AC1: Images are stored in Supabase Storage.
- AC2: Only PNG, JPG or WEBP files up to 2 MB are accepted.
- AC3: If a course has no image, a placeholder image is shown.

### US-704 Publish and unpublish — **Must**
As **staff**, I want to change a course between Draft and Published, so that I decide when customers can see it.

- AC1: Each course has a control to switch between Draft and Published.
- AC2: After a course is switched to Published it appears in the catalog. After it is switched back to Draft it no longer appears.

### US-705 Archive a course — **Should**
As the **owner**, I want to archive a course, so that it is no longer sold but past orders stay correct.

- AC1: Only the owner can archive a course.
- AC2: Archived courses are hidden from the catalog but still appear in past orders and customer libraries.
- AC3: A course that appears in any order has no delete option. It can only be archived.

### US-706 Manage modules and learning paths — **Should**
As **staff**, I want to edit a course's modules and its place in a learning path, so that the outline and skill tree are correct.

- AC1: Modules can be added, edited, reordered and removed.
- AC2: A course can be assigned to a learning path and given prerequisite courses.
- AC3: A course cannot be its own prerequisite, and prerequisites cannot form a loop.

### US-707 View all orders — **Should**
As **staff**, I want to see all orders, so that I can help customers.

- AC1: A list of all orders with the order number, customer, date, total and status.
- AC2: Orders are read-only for staff.

### US-708 Manage staff accounts — **Could**
As the **owner**, I want to give or remove the staff role, so that I control who can manage the store.

- AC1: The owner can change a user's role between customer and staff.
- AC2: The owner cannot change their own role, so the store always has an owner.

---

## 8. Key domain terms

| Term | Meaning |
|---|---|
| **Course** | The product being sold: a fictional training course. |
| **Module** | One part of a course. Customers mark modules complete. |
| **Category** | A topic group, such as AI, Data or Python. |
| **Level** | Course difficulty: Beginner, Intermediate or Advanced. |
| **Course status** | Draft (hidden), Published (for sale) or Archived (no longer sold). |
| **Cart** | Courses chosen but not yet ordered. |
| **Order** | A completed demo checkout. Stores the price paid at the time of the order. |
| **Order item** | One course inside an order. |
| **Library** | The courses a customer owns. |
| **Outbox** | A database table holding emails the app *would* send. Nothing is really sent. |
| **XP** | Experience points earned for learning actions. |
| **Level (gamification)** | A rank based on total XP. Not the same as course level. |
| **Badge** | A reward for reaching a milestone. |
| **Learning path** | An ordered group of courses on one theme. |
| **Prerequisite** | A course suggested before another one. It is advice, not a lock. |
| **Skill tree** | A diagram of a learning path showing progress. |
| **Role** | Guest, customer, staff or owner. Controls what a user can do. |
| **RLS** | Row Level Security: Supabase database rules that limit which rows each user can see or change. |

## 9. Seed data for demos

Seed data lives in `supabase/seed/`. All names and content are fictional.

### Demo accounts

| Role | Display name | Email |
|---|---|---|
| Owner | Demo Owner | owner@example.com |
| Staff | Demo Staff | staff@example.com |
| Customer | Ada Learner | ada@example.com |
| Customer | Sam Starter | sam@example.com |

Use only `example.com` addresses, which are reserved for examples. Demo passwords are set
by the seed script from local environment variables and are **never written in the repository**.

### Categories
AI, Data, Python, Responsible AI

### Courses (all prices in USD, fictional)

| # | Title | Category | Level | Hours | Price | Learning path | Prerequisite |
|---|---|---|---|---|---|---|---|
| 1 | Python Basics for Data | Python | Beginner | 6 | $0 | Data Explorer | – |
| 2 | Spreadsheets to DataFrames | Data | Beginner | 5 | $19 | Data Explorer | 1 |
| 3 | Data Cleaning Workshop | Data | Intermediate | 8 | $29 | Data Explorer | 2 |
| 4 | Charts That Tell Stories | Data | Intermediate | 6 | $29 | Data Explorer | 2 |
| 5 | Intro to Machine Learning | AI | Beginner | 10 | $39 | AI Builder | 1 |
| 6 | Building a Simple Classifier | AI | Intermediate | 8 | $49 | AI Builder | 5 |
| 7 | Prompting Large Language Models | AI | Beginner | 4 | $25 | AI Builder | – |
| 8 | AI Fairness and Ethics | Responsible AI | Beginner | 4 | $0 | AI Builder | – |
| 9 | Deploying Data Apps | Python | Advanced | 7 | $59 | – | 3 |
| 10 | SQL Fundamentals (Draft) | Data | Beginner | 6 | $19 | – | – |

Course 10 stays **Draft** so the demo shows that drafts are hidden. Each published course
has 3–5 modules.

### Gamification

| Level | Name | Total XP needed |
|---|---|---|
| 1 | Explorer | 0 |
| 2 | Apprentice | 100 |
| 3 | Practitioner | 300 |
| 4 | Specialist | 600 |
| 5 | Mentor | 1000 |

| Badge | How to earn it |
|---|---|
| First Step | Finish your first module |
| Course Finisher | Finish your first course |
| Data Explorer | Finish every course in the Data Explorer path |
| AI Builder | Finish every course in the AI Builder path |
| Responsible Learner | Finish AI Fairness and Ethics |
| All About Me | Complete your profile |

### Sample activity
- **Ada Learner:** owns courses 1, 2 and 8, has finished course 1, has 1 past order and 1 outbox email.
- **Sam Starter:** no orders, 2 courses in the cart. Use this account to demo checkout.

## 10. Out of scope

These are **not** part of this demo:

- Real payments, payment providers or storing card details
- Sending real emails, SMS or push notifications
- Real course content, video hosting or certificates
- Taxes, invoices, multiple currencies or discount codes
- Refunds and returns
- Product reviews and ratings
- Public leaderboards or comparing users
- Buying XP, badges or levels
- Social sign-in (Google, GitHub and others) or two-factor authentication
- Shipping or physical products
- Multiple languages
- Mobile apps (the Streamlit web app only needs to work in a browser)
- Analytics, tracking or advertising
- Production-level scaling, monitoring and backups
