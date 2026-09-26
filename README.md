# Companion Care — Elder-Care & Verified Caregiver Marketplace

A working starter platform for Idea #1 from your business plan: a marketplace
that connects families needing elder-care support with background-checked,
personally vetted companions/caregivers.

This is real, runnable code — tested end-to-end before delivery (public
signup forms, admin login, the verification checklist, match creation, and
visit logging all work). It is **not** a mockup.

---

## Before you use this: an honest note

Your own business plan (and the roadmap document you already have) recommends
starting **manually** — WhatsApp and Google Sheets — before building any
software, so you can validate that real families and real caregivers will
actually use the service before investing time in a platform. That advice
still stands. This codebase is here because you asked for it, not because
building it first is the recommended sequence.

A sensible way to use this: keep running your first 10-20 matches manually
by phone/WhatsApp as planned, and bring this platform in once you have real
volume and are starting to feel the pain of tracking families, caregivers,
and visits in a spreadsheet. The verification checklist here mirrors exactly
the process from your business plan (reference checks, police verification
status, a trial visit) — it doesn't skip or soften any of it.

---

## What's included

- **Public website** — a landing page, a family sign-up form, and a
  caregiver application form.
- **Admin dashboard** (password-protected, single admin login) —
  - **Families** tab: every inquiry, with status tracking (new → contacted → matched).
  - **Caregivers** tab: every application, with the full verification
    checklist (reference 1 checked, reference 2 checked, police verification
    status, trial visit done + notes, overall verified/pending/rejected).
    **A caregiver cannot be matched with a family until marked "Verified"** —
    this is enforced by the server, not just the interface, so it can't be
    accidentally skipped.
  - **Matches** tab: pair a verified caregiver with a family, set the
    schedule and pricing, end a match when needed.
  - **Visit Log** tab: record each visit, whether the caregiver and family
    both confirmed it happened, and flag anything for follow-up.

## Technology used, and why

- **Node.js + Express** — a simple, widely-supported way to run a web
  server and API. No heavier framework needed for this scope.
- **SQLite** (via `better-sqlite3`) — a database that lives in a single
  file (`data.db`) on your own computer or server, with no separate
  database software to install or pay for. Fine for hundreds or a few
  thousand records; if you outgrow it later, migrating to a hosted
  database (e.g. PostgreSQL) is a well-understood next step.
- **Plain HTML/CSS/JavaScript** on the frontend — no build step, no
  framework to learn. Every page is a single file you can open and read
  top to bottom.

---

## How to run this yourself

You'll need [Node.js](https://nodejs.org) installed (version 18 or newer).

```bash
# 1. Install dependencies
npm install

# 2. Create your own environment file
cp .env.example .env
# then open .env and fill in a real SESSION_SECRET, ADMIN_EMAIL, and ADMIN_PASSWORD

# 3. Create your admin login (run this once, or again later to reset your password)
npm run seed-admin

# 4. Start the server
npm start
```

Then open **http://localhost:3000** in your browser for the public site,
and **http://localhost:3000/admin/login.html** to log in as admin.

## Putting this online for real families to use

Running it on your own laptop only works while your laptop is on and
connected. To make the public site reachable by anyone:

1. Deploy it to a low-cost host that runs Node.js (Render, Railway, and
   Hostinger VPS are common, affordable options for a solo founder — compare
   current pricing yourself, as this changes over time).
2. Set the same environment variables (`SESSION_SECRET`, `ADMIN_EMAIL`,
   `ADMIN_PASSWORD`) on that host instead of in a local `.env` file.
3. Point a domain name at it if you want a proper web address instead of a
   default one from the hosting provider.

This step is optional in the early phase — many operators run this only on
their own laptop or a shared office computer while volume is still small,
and only move to real hosting once they have paying customers relying on
the system daily.

---

## Important: data handling and safety

- **Caregiver ID numbers** are stored in the database as entered. Treat
  `data.db` as sensitive — back it up somewhere private, don't email it
  around, and don't commit it to a public code repository (a `.gitignore`
  entry for `data.db*` and `.env` is included).
- The admin password is the only thing protecting families' and
  caregivers' personal information. Choose a strong one, and don't share
  the admin login with anyone you haven't personally vetted the way you'd
  vet a caregiver.
- This platform does **not** perform police verification itself — it only
  tracks the status you report (not started / in progress / completed).
  The actual verification is a process you or the caregiver completes with
  local authorities, exactly as described in your business plan.
- The site explicitly states this is a **non-medical** companionship
  service on the landing page. Keep that boundary clear in all your
  marketing and conversations with families — this is also a safety and
  liability matter, not just wording.

---

## What this does not include (on purpose, to keep it simple to start)

- **Online payments** — the plan recommends collecting payment via UPI
  directly at this stage, not building a payment gateway before you have
  real volume. Adding one later (e.g. Razorpay) is a contained addition
  once it's worth the integration effort.
- **SMS/WhatsApp notifications** — for now, you'd still call or WhatsApp
  families and caregivers directly, using the dashboard as your system of
  record. Automating notifications is a sensible "Year 2" addition once
  the manual version is proven, consistent with the phased roadmap in your
  business plan.
- **Multiple admin accounts / roles** — built for a single founder running
  the business personally, matching your current setup. Adding more admin
  users later is a small, well-scoped change if you hire help.

## Project structure

```
elder-care-marketplace/
├── package.json
├── .env.example
├── server/
│   ├── index.js          # main server entry point
│   ├── db.js              # database connection + table schema
│   ├── seed-admin.js      # one-time script to create your admin login
│   ├── middleware/
│   │   └── auth.js        # protects admin-only API routes
│   └── routes/
│       ├── auth.js        # login/logout
│       ├── families.js    # family inquiries
│       ├── caregivers.js  # caregiver applications + verification
│       ├── matches.js     # pairing families with verified caregivers
│       └── visits.js      # visit logging
└── public/
    ├── index.html          # landing page
    ├── family-signup.html
    ├── caregiver-apply.html
    ├── css/style.css
    ├── js/main.js
    └── admin/
        ├── login.html
        ├── dashboard.html
        ├── css/admin.css
        └── js/admin.js
```
