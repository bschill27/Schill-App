# CLAUDE.md

> **Provisional.** This file is built from the decisions Brent stated in chat. The
> spec's "Tech stack and CLAUDE.md starter" section hasn't been received yet.
> When `docs/spec.md` arrives, rebuild this file from that section, and let the
> spec win wherever the two disagree.

## What we're building

An iPhone and Android app for clients of **Schill Insurance Brokers Ltd** (British
Columbia). The goal is the easiest, most beautiful insurance app our clients have
used: a plain-language snapshot of their coverage, policies, quotes and renewals,
with their broker one tap away.

## Repo layout (pnpm workspace)

| Path | What it is |
| --- | --- |
| `apps/mobile` | Expo + React Native + Expo Router, TypeScript strict |
| `apps/server` | Next.js (App Router): the API for the app + a small staff inbox web UI |
| `packages/shared` | Shared TypeScript types, Zod schemas, fictional BC sample data |
| `docs/` | Spec (`docs/spec.md`) and build plan (`docs/build-plan.md`) |

## Tech stack

- **Mobile:** Expo SDK 58 (latest stable at time of writing), React Native, Expo Router
  (file-based routes under `apps/mobile/app/`). Use `npx expo install` for every
  Expo/RN dependency so versions match the SDK.
- **Server:** Next.js 16, App Router, Route Handlers under `apps/server/app/api/`.
- **Shared:** Zod 4 is the single source of truth for every shape that crosses
  the network. Derive types with `z.infer`; never hand-write a duplicate type.
- **TypeScript:** `strict: true` everywhere, plus `noUncheckedIndexedAccess`. Don't use
  `any`, and don't use `@ts-ignore` without a comment explaining why.
- **Package manager:** pnpm workspaces. Node 22 LTS.
- Follow the **current** Expo and Next.js docs. Don't copy older patterns
  (e.g. `pages/` router, `expo-cli` global, Webpack-era Expo web config).

## Hard rules (don't break these)

1. **Applied Epic is READ-ONLY.** Only `apps/server` talks to Epic, and only through
   the one connector in `apps/server/lib/epic/`. The connector exposes read methods
   only. No create/update/delete calls, ever, even "temporarily".
2. **The mobile app never calls Epic** and never holds Epic credentials.
3. **Everything a client sends goes to the staff inbox**, never to Epic: messages,
   change requests, quote requests, documents, claims notices.
   Staff action it manually in Epic.
4. **The Epic connector has a mock** backed by fictional BC sample data in
   `packages/shared`. The mock is the default in dev and test. Never put real
   client data in the repo, fixtures, screenshots or logs.
5. **Demo sign-in is for development only.** It must be impossible to enable in
   a production build. Real auth (MFA + Face ID / biometrics) is required before launch.
6. **No secrets in the repo.** Use env vars, and keep `.env.example` up to date.

## Product and UX principles

- Plain language first. Say "what you're covered for", not "insuring agreement".
  Always offer a path to the actual policy wording.
- The broker is one tap away from every screen (call, message, email).
- Accessibility is required: Dynamic Type, screen readers, 4.5:1 contrast,
  44pt touch targets.
- Show money as CAD and dates in the `en-CA` locale.

## Commands

The workspace isn't scaffolded yet. Fill this in during prompt 1 of the build plan:
install, dev (mobile/server), typecheck, lint, test.

## Working conventions

- Run typecheck, lint and tests before every commit.
- Keep changes small, with one build-plan step per PR where practical.
- When you're unsure about insurance or compliance wording, flag it for Brent
  rather than guessing.
