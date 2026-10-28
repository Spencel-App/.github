# Spencel

**The money app for people who share life with someone.**

Spencel catches the subscriptions you forgot about, tells you what's actually safe to
spend, and settles up with your partner or roommate — with honest flat pricing and no
success fees.

---

## Why Spencel

Most money apps are built the way developers think: by data domain. You open them and
get a database to browse. But you don't open a money app to browse tables — you open it
with a worry:

- *"Am I okay this month, or about to overdraft?"*
- *"What am I being charged for that I forgot about?"*
- *"Did Alex pay me back yet?"*
- *"Can I afford this right now?"*

Spencel is built to answer those questions in a glance and a tap.

### What it does

- **Catch the leaks.** Find recurring charges — especially the forgotten ones, price
  hikes, and free trials about to convert.
- **Tell you if you're okay.** One honest "safe-to-spend" number after bills and
  commitments. Not a dashboard you have to decode.
- **Settle up, effortlessly.** Shared bills and one-off splits, a clear "who owes who,"
  and a one-tap reminder — for the person you actually share money with.
- **No busywork.** Auto-sync where it helps; dead-simple manual and photo entry where it
  doesn't. Manual entry stays free.
- **No tricks.** Pricing shown before you link anything. We never hold your money, and we
  never take a cut of what we save you.

### Honest pricing

One plan. One price, shown up front.

- **Free, forever** — manual and photo entry, imports, budgets, subscription detection,
  safe-to-spend, and a household of two.
- **Plus — $6.99/mo or $59.99/yr** — unlimited linked accounts, real-time sync, and
  unlimited receipt scanning. Covers the whole household.

No fees for "bill negotiation." No percentage of your savings. Ever.

---

## This organization

Spencel is built across a few focused repositories.

| Repo | What it is |
|---|---|
| [`backend`](https://github.com/Spencel-App/backend) | The edge-native API — Cloudflare Workers running FastAPI (Python), with D1 for data and R2 for storage. |
| [`frontend`](https://github.com/Spencel-App/frontend) | The mobile app — React Native on Expo, for iOS and Android. |
| [`landing`](https://github.com/Spencel-App/landing) | The marketing site and waitlist — built with Astro and Tailwind. |
| [`Spencel_designs`](https://github.com/Spencel-App/Spencel_designs) | Brand and product design sources — the canonical design files the app and site are built from. |
| [`agents`](https://github.com/Spencel-App/agents) | Self-contained implementation briefs and automation used to build Spencel. |
| [`.github`](https://github.com/Spencel-App/.github) | This repo — the org profile and shared community health files (contributing, security, code of conduct, issue and PR templates). |

---

## Our bar

A few principles we hold ourselves to:

- **Answer questions, don't list data.** Every screen earns its place by helping someone,
  not by showing off a data model.
- **Every number comes with a plain sentence.** "Dining is your biggest category, up 12%"
  — not a chart you have to interpret.
- **Never fake success.** If a sync or a save fails, we say so. We don't quietly show you
  numbers that aren't real.
- **You're in control.** See everything that's connected, export it all, and delete
  everything in one tap.

---

Spencel is early and built in the open where we can. If something here is unclear or
could be better, open an issue — we read them.
