# E-commerce Demo (Streamlit + Supabase)

A training project: a demo online shop built with **Streamlit** (UI) and **Supabase**
(database, sign-in and file storage). It also serves as an example of deploying to
**Streamlit Community Cloud**.

> Status: skeleton only. Folders and starter files are in place; no application code yet.

## Folder structure

```
ecommerce-demo/
├── app/                    # All application code
│   ├── pages/              # Streamlit pages (one file per page, e.g. Products, Cart)
│   ├── components/         # Reusable UI pieces (product card, nav bar, ...)
│   ├── services/           # Talks to Supabase: auth, database queries, storage
│   ├── models/             # Data shapes (Product, Order, User, ...)
│   ├── utils/              # Small helpers (formatting, validation)
│   └── config/             # Loads settings and secrets
├── assets/
│   ├── images/             # Static images (logo, placeholders)
│   └── styles/             # Custom CSS
├── docs/
│   ├── requirements/       # What the app must do
│   ├── architecture/       # How the parts fit together
│   ├── deployment/         # How to deploy to Streamlit Community Cloud
│   └── planning/           # Roadmap, tasks, notes
├── supabase/
│   ├── migrations/         # SQL that creates/changes tables
│   └── seed/               # Sample data for development
├── tests/
│   ├── unit/               # Fast tests for single functions
│   └── integration/        # Tests that touch Supabase
├── scripts/                # Helper scripts (setup, seeding, ...)
├── .streamlit/
│   └── secrets.toml.example  # Template for Streamlit secrets
├── .env.example            # Template for local environment variables
├── .gitignore
├── requirements.txt        # Python dependencies (used by Streamlit Cloud)
└── README.md
```

Empty folders contain a `.gitkeep` file so Git keeps them.

## Secrets: keep them out of Git

Real keys never go in the repository. Only the `.example` templates are committed.

| File | Committed? | Purpose |
|---|---|---|
| `.env.example` | Yes | Template with empty values |
| `.env` | **No** (git-ignored) | Your local values |
| `.streamlit/secrets.toml.example` | Yes | Template with empty values |
| `.streamlit/secrets.toml` | **No** (git-ignored) | Your local Streamlit secrets |

Local setup:

```bash
cp .env.example .env
cp .streamlit/secrets.toml.example .streamlit/secrets.toml
# then fill in the values from Supabase > Project Settings > API
```

On Streamlit Community Cloud, paste the secrets into **App settings → Secrets**. Do not upload a file.

## Note for later

Streamlit looks for the `pages/` folder **next to the main script**. When the entry point is
added, put it at `app/main.py` so `app/pages/` is picked up. Set that path as the main file
when deploying.
