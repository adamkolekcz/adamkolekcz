<div align="center">

# Adam Kolek

**Founder of [Catermink](https://catermink.com)** — building the operating system for caterers

<p>
  <img src="https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white" alt="Next.js">
  <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React">
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript">
  <img src="https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white" alt="Tailwind CSS">
  <img src="https://img.shields.io/badge/Prisma-2D3748?style=for-the-badge&logo=prisma&logoColor=white" alt="Prisma">
  <img src="https://img.shields.io/badge/Turso-1EBD9A?style=for-the-badge&logoColor=white" alt="Turso">
  <img src="https://img.shields.io/badge/Sentry-362D59?style=for-the-badge&logo=sentry&logoColor=white" alt="Sentry">
  <img src="https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white" alt="Vercel">
</p>

</div>

---

I'm Adam — founder of **Catermink**, a multi-tenant SaaS that runs the back office for catering businesses. I work end to end: product, full-stack web, and the infrastructure behind it.

### 🍽 Currently building — [Catermink →](https://catermink.com)

Inquiries, PDF quotes, client & event management, staff scheduling — multi-tenant from day one. Built on Next.js 16, Turso/libSQL, Prisma, Sentry and Vercel.

### ✉ Get in touch

- **Email** — adam@catermink.com
- **LinkedIn** — [personal](https://www.linkedin.com/in/adamkolek/) · [Catermink](https://www.linkedin.com/company/112777367/)

---

## 📐 How Catermink is built

A high-level architecture write-up. The product source is private.

### The problem

Caterers juggle inquiries, quotes, events, clients and staff across spreadsheets, inboxes and paper. The work is bursty and deadline-driven, and a missed detail costs a booking. Catermink replaces that patchwork with one system built around how catering actually runs.

### What Catermink is

A multi-tenant web application where each catering business gets an isolated workspace covering:

- **Inquiries** — public inquiry forms feed straight into the pipeline.
- **Quotes** — branded PDF offers generated on the fly.
- **Clients & events** — a calendar-driven view of bookings.
- **Staff** — shift planning with role-based access.

### Architecture

- **Multi-tenancy from day one.** Every record is scoped to a tenant, so data never leaks across workspaces. Tenant resolution happens at the edge before any business logic runs.
- **Domain isolation.** The admin app, the public inquiry forms and the client-facing surface are served on separate subdomains, each with its own trust boundary.
- **Role-based access.** Three levels — owner, admin and staff — gate what each user can see and do inside a workspace.
- **Auth.** Stateless, token-based sessions verified on every request.
- **Documents.** Quotes are rendered to PDF server-side from structured data.

### Tech decisions

- **Next.js 16 (App Router) + React 19** — one framework for the marketing surface, the authenticated app and the API, with server components keeping the client bundle lean.
- **Turso / libSQL + Prisma** — SQLite semantics with a hosted, replicated edge database. Local development runs against a file; production runs against Turso. Prisma gives typed access and predictable migrations.
- **Sentry** — errors and performance traced across the whole app, so production issues surface with context instead of guesswork.
- **Vercel** — preview deploys per branch and a clean path from staging to production.
- **TypeScript + Zod end to end** — types at the boundaries, validation at the edges, no `any`.

### Why these tradeoffs

- **libSQL over a classic Postgres host** — edge-replicated reads and a frictionless local story mattered more than heavyweight relational features the product doesn't need yet.
- **Subdomain isolation over a single app** — clearer trust boundaries and independently cacheable surfaces, at the cost of a bit more routing plumbing.
- **Custom token sessions over a managed auth vendor** — full control over the multi-tenant session shape, with no per-seat vendor pricing as tenants grow.

### Screenshots

_Coming soon._

### Status

Running in production with a separate staging environment and a dedicated QA workflow.
