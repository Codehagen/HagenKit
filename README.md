<a href="https://www.prismstack.com/hagenkit">
  <h1 align="center">HagenKit: Production-Ready SaaS Boilerplate</h1>
</a>

<p align="center">
  Launch enterprise-grade SaaS experiences faster with a cohesive Next.js foundation for marketing surfaces, authenticated dashboards, and admin tooling.
</p>

<p align="center">
  <img width="1200" alt="HagenKit dashboard mockup" src="public/hero.png" />
</p>

<p align="center">
  <a href="https://github.com/codehagen/hagenkit/blob/main/LICENSE.md">
    <img src="https://img.shields.io/badge/license-AGPL--3.0-blue.svg" alt="License: AGPL-3.0" />
  </a>
</p>

<p align="center">
  <a href="#introduction"><strong>Introduction</strong></a> ·
  <a href="#installation"><strong>Installation</strong></a> ·
  <a href="#tech-stack--features"><strong>Tech Stack + Features</strong></a> ·
  <a href="#architecture"><strong>Architecture</strong></a> ·
  <a href="#directory-structure"><strong>Directory Structure</strong></a> ·
  <a href="#contributing"><strong>Contributing</strong></a>
</p>
<br/>

## Introduction

HagenKit is a batteries-included SaaS boilerplate that combines modern product design with production-ready infrastructure. Built on Next.js 16 and the App Router, it delivers authentication, multi-tenant workspaces, dashboards, and a marketing site so you can focus on customer value instead of scaffolding.

**Highlights**
- **Multi-tenant SaaS foundations** – Workspace model with owner/admin/member/viewer roles, invitations, and default workspace management.
- **Authentication that scales** – Better Auth with email/password, Google OAuth, session management, and client helpers for hydration-safe flows.
- **Responsive UI system** – Shadcn UI + Tailwind CSS components, marketing sections, and dashboard primitives tuned for accessibility.
- **Email-ready out of the box** – React Email templates and Resend integration for transactional messages.
- **Developer velocity** – TypeScript everywhere, server actions, data hooks, and deploy-ready configuration for Vercel.

## Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/your-org/hagenkit.git
cd hagenkit
pnpm install
```

Set up environment variables:

```bash
# -----------------------------------------------------------------------------
# Core app URLs & Better Auth configuration
# -----------------------------------------------------------------------------
NEXT_PUBLIC_APP_URL="http://localhost:3000"
BETTER_AUTH_URL="http://localhost:3000/api/auth"
# Generate a secure random string: `openssl rand -hex 32`
BETTER_AUTH_SECRET="replace-with-generated-secret"

# -----------------------------------------------------------------------------
# Database (PostgreSQL / Prisma)
# -----------------------------------------------------------------------------
# Example local connection string – update credentials/host/db as needed.
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/hagenkit"

# -----------------------------------------------------------------------------
# OAuth providers
# -----------------------------------------------------------------------------
GOOGLE_CLIENT_ID="your-google-oauth-client-id"
GOOGLE_CLIENT_SECRET="your-google-oauth-client-secret"

# -----------------------------------------------------------------------------
# Email & notifications (Resend + branding overrides)
# -----------------------------------------------------------------------------
RESEND_API_KEY="re_your_resend_api_key"
EMAIL_FROM="noreply@hagenkit.dev"
EMAIL_FROM_NAME="HagenKit"
SUPPORT_EMAIL="support@hagenkit.dev"
```

Generate the Prisma client and sync the schema:

```bash
pnpm prisma:generate
pnpm prisma:push
```

Start the development server with Turbopack:

```bash
pnpm dev
```

- `pnpm email` – launch the React Email preview server.
- `pnpm lint` – run ESLint with the project config.

## Magic Link Setup

HagenKit supports Magic Link authentication out of the box.

### Development
For a seamless developer experience, if you do not provide a `RESEND_API_KEY` in your `.env.local` file while in development mode, **emails will be logged to your terminal console**.

1.  Go to the Sign In page.
2.  Enter your email and click "Sign in with Magic Link".
3.  Check your terminal where `pnpm dev` is running.
4.  Click the link printed in the console to sign in.

### Production
For production, you must set up Resend:

1.  Create an account at [Resend](https://resend.com).
2.  Get your API Key.
3.  Add `RESEND_API_KEY` to your environment variables.
4.  Verify your domain in Resend to ensure emails are delivered reliably.

## 🛡️ Security: First User Setup

**Automatic Admin Assignment:**
HagenKit automatically assigns the `admin` role to the **first user** who signs up. This ensures you have immediate access to the admin panel without any manual database manipulation.

1.  **First Sign-Up**: The first user created in the system will be granted the `admin` role.
2.  **Subsequent Users**: All users signing up after the first one will be assigned the default `user` role.

**No manual schema changes or migrations are required.** The system handles this logic securely via database hooks.

## Tech Stack + Features

### Frameworks & Platforms
- **Next.js 16** – App Router, Server Actions, and edge-ready rendering.
- **Prisma ORM v7 + PostgreSQL** – Type-safe ORM with `@prisma/adapter-pg` driver for direct TCP connections. Generated client in `app/generated/prisma`.
- **Better Auth** – Composable auth with cookie/session helpers and social providers.
- **Vercel** – First-class deployment target with optimized build output.

### UI & UX
- **Shadcn UI & Tailwind CSS** – Component library with design tokens and Radix primitives.
- **Framer Motion (via `motion`)** – Micro-interactions and animation choreography.
- **Lucide & Tabler Icons** – Consistent iconography across marketing and product surfaces.
- **Responsive marketing shell** – Polished landing page in `app/(marketing)` with reusable layout primitives.

### Application Capabilities
- **Dashboard modules** – Team, analytics, lifecycle, and settings routes ready for data wiring.
- **Workspace management** – Invitations, member role updates, and ownership safeguards.
- **Settings UI** – Account, profile, and workspace panels using configurable data tables (`@tanstack/react-table`).
- **Search & filtering utilities** – `nuqs` for deep-linkable filters and stateful navigation.
- **Productivity hooks** – Debounced callbacks, media queries, and mobile detection helpers.

### Communications
- **React Email** templates in `emails/` ready for transactional flows.
- **Resend** integration glue for real email delivery.

## Architecture

HagenKit separates concerns to keep features composable and scalable:

- **App Router segmentation** – Marketing `app/(marketing)`, auth flows `app/(auth)`, admin area `app/(admin)`, and the authenticated dashboard under `app/dashboard`.
- **Server Actions** – Business logic lives in `app/actions/*` with typed inputs and output helpers (`ActionResult`).
- **Data Layer** – Prisma schema models users, sessions, workspaces, invitations, and roles for robust multi-tenancy.
- **Configuration** – Centralized metadata in `lib/config.ts` powers SEO, social cards, and upgrade CTAs.
- **UI System** – Shared primitives in `components/ui`, marketing layout helpers, and specialized dashboard/admin components.

## Directory Structure

```
.
├── app
│   ├── (marketing)        # Public marketing landing experience
│   ├── (auth)             # Sign-in and sign-up flows
│   ├── (admin)            # Admin panel entry
│   ├── dashboard          # Authenticated product surface
│   ├── actions            # Server actions for auth, workspaces, admin tools
│   └── generated/prisma   # Generated Prisma client (keep in sync)
├── components
│   ├── ui                 # Shadcn-derived component library
│   ├── auth               # Auth-specific views and helpers
│   ├── dashboard          # Dashboard shell and empty states
│   ├── settings           # Settings navigation, forms, tables
│   └── marketing          # Landing page layout primitives
├── emails                 # React Email templates and preview entrypoint
├── hooks                  # Reusable client hooks (media queries, debounce, tables)
├── lib                    # Auth, config, utilities, and Prisma helpers
├── prisma                 # Database schema and migrations
└── public                 # Static assets (hero imagery, icons, og assets)
```

## Contributing

We welcome contributions! To get involved:

- Open an issue for bugs, feature requests, or questions.
- Submit a pull request with clear scope, tests when applicable, and a concise changelog entry.
- Share feedback on developer experience, documentation, or onboarding.

Let's build production-grade SaaS products faster—together.
