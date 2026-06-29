# Catermink — Case Study

← [Back to profile](./README.md)

> How I designed and built **[Catermink](https://catermink.com)** — a multi-tenant SaaS that runs the back office for catering businesses. This is a high-level architecture write-up; the product source is private.

![Catermink dashboard — open inquiries, offers in progress and confirmed events at a glance](./screenshots/dashboard.png)

## The problem

Caterers juggle inquiries, quotes, events, clients and staff across spreadsheets, inboxes and paper. The work is bursty and deadline-driven, and a missed detail costs a booking. Catermink replaces that patchwork with one system built around how catering actually runs.

## What Catermink is

A multi-tenant web application where each catering business gets an isolated workspace covering:

- **Inquiries** — public inquiry forms feed straight into the pipeline.
- **Quotes** — branded PDF offers generated on the fly.
- **Clients & events** — a calendar-driven view of bookings.
- **Staff** — shift planning with role-based access.

## Architecture

- **Multi-tenancy from day one.** Every record is scoped to a tenant, so data never leaks across workspaces. Tenant resolution happens at the edge before any business logic runs.
- **Domain isolation.** The admin app, the public inquiry forms and the client-facing surface are served on separate subdomains, each with its own trust boundary.
- **Role-based access.** Three levels — owner, admin and staff — gate what each user can see and do inside a workspace.
- **Auth.** Stateless, token-based sessions verified on every request.
- **Documents.** Quotes are rendered to PDF server-side from structured data.

## Tech decisions

- **Next.js 16 (App Router) + React 19** — one framework for the marketing surface, the authenticated app and the API, with server components keeping the client bundle lean.
- **Turso / libSQL + Prisma** — SQLite semantics with a hosted, replicated edge database. Local development runs against a file; production runs against Turso. Prisma gives typed access and predictable migrations.
- **Sentry** — errors and performance traced across the whole app, so production issues surface with context instead of guesswork.
- **Vercel** — preview deploys per branch and a clean path from staging to production.
- **TypeScript + Zod end to end** — types at the boundaries, validation at the edges, no `any`.

## Why these tradeoffs

- **libSQL over a classic Postgres host** — edge-replicated reads and a frictionless local story mattered more than heavyweight relational features the product doesn't need yet.
- **Subdomain isolation over a single app** — clearer trust boundaries and independently cacheable surfaces, at the cost of a bit more routing plumbing.
- **Custom token sessions over a managed auth vendor** — full control over the multi-tenant session shape, with no per-seat vendor pricing as tenants grow.

## Screenshots

_All screenshots use demo data._

**Bookings calendar** — every event colour-coded by status (new, in progress, sent, confirmed, done).

![Bookings calendar with colour-coded event statuses](./screenshots/calendar.png)

**Staff scheduling** — drag people onto a confirmed event; track who's confirmed, who's a substitute, and who declined.

![Drag-and-drop staff scheduling with confirmations, substitutes and declines](./screenshots/shifts.png)

**Shareable inquiry forms** — copy a link or embed a form on the caterer's own site, with or without prices.

![Inquiry forms with copy-link and embed options](./screenshots/forms.png)

**Roles & permissions** — owner, admin and staff, each with a clearly scoped permission matrix.

![Access management with a per-role permission matrix](./screenshots/access.png)

**The public inquiry form** — guests pick a date (with live availability), time, location and headcount…

![Public inquiry form — event details step with availability calendar](./screenshots/inquiry-details.png)

…and finish with their contact details. The caterer receives a structured lead instead of a vague email.

![Public inquiry form — contact details step](./screenshots/inquiry-contact.png)

## Status

Running in production with a separate staging environment and a dedicated QA workflow.

---

Built by **Adam Kolek** · [catermink.com](https://catermink.com) · adam@catermink.com
