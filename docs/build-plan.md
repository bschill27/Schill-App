# Build plan (provisional)

> **Status:** draft. It's built from the decisions stated in chat, because the spec and
> Brent's prompt list (1–13) haven't been received yet. When they arrive, map each
> step below to the matching prompt, then reorder, merge or replace steps so this
> follows prompts 1–13 exactly.

Versions checked on npm on 2026-10-30: Expo SDK 58 (`expo@58.0.21`),
Next.js `16.4.3`, React `19.3.1`, Zod `4.6.7`. Recheck these on the day of scaffolding.

## Steps

| # | Step | Output | Done when |
| --- | --- | --- | --- |
| 1 | **Scaffold the workspace** | pnpm workspace, `apps/mobile` (create-expo-app, SDK 58, Expo Router), `apps/server` (create-next-app, App Router), `packages/shared`; shared tsconfig (strict), ESLint, Prettier, Vitest/Jest, CI (typecheck/lint/test) | Mobile boots in Expo Go / dev build, server serves `/api/health`, CI green |
| 2 | **Domain model + Zod schemas** | `packages/shared`: Client, Policy, Coverage, Vehicle/Property, Quote, Renewal, Document, Broker, InboxItem schemas | Schemas compile; types inferred, not duplicated |
| 3 | **Fictional BC sample data** | 3–5 sample households (home, auto incl. ICBC-style optional cover, tenant, small business), quotes, upcoming renewals | Data validates against the schemas in a test |
| 4 | **Epic connector interface + mock** | `apps/server/lib/epic/`: read-only `EpicConnector` interface, `MockEpicConnector` on sample data, factory picking the mock by default | Test fails if the interface gains any write method |
| 5 | **Server API** | Route handlers: `/api/me`, `/api/policies`, `/api/policies/[id]`, `/api/quotes`, `/api/renewals`, `/api/documents`; Zod-validated responses | Contract tests pass against the mock |
| 6 | **Staff inbox (backend)** | Inbox storage (Postgres via a hosted provider; SQLite locally), `/api/inbox` POST for client submissions, audit log | Submissions are stored; nothing is routed to Epic |
| 7 | **Demo sign-in** | Demo accounts mapped to sample clients, session token, production build guard | A production build refuses demo sign-in (tested) |
| 8 | **Mobile shell + design system** | Theme tokens, typography, light/dark, tab layout (Home, Policies, Documents, Contact), "broker one tap away" action | Accessible components; screenshot review |
| 9 | **Home snapshot** | Plain-language coverage summary, next renewal, open quotes, alerts | Reads only from the server API |
| 10 | **Policies & coverage detail** | Policy list, detail with plain-language cover/limits/deductibles, link to wording/documents | Copy reviewed for accuracy |
| 11 | **Quotes & renewals** | Quote list/detail, renewal timeline, "I have a question / request a change" → inbox | Client actions land in the staff inbox |
| 12 | **Staff inbox UI** | Next.js staff pages: list, detail, status (new/in progress/done), staff-only demo auth | Staff can triage submissions end to end |
| 13 | **Hardening & launch prep** | Real auth (MFA + Face ID / biometrics), secure storage, privacy policy, account deletion, error tracking, EAS Build/Submit, store listings | Launch checklist below is complete |

## Open questions and risks

1. **Spec and prompt list missing.** This whole plan is provisional until they arrive.
2. **Applied Epic access.** We need to confirm with Applied which API access Schill has
   (Epic SDK/API licensing, read-only scoped credentials, rate limits, sandbox).
   This is the longest lead-time item, so start the conversation now.
3. **Keeping Epic read-only.** It has to be enforced in code (interface with read methods only,
   plus a test) *and* in the credentials, if Applied supports read-only scopes.
4. **Demo sign-in reaching production.** Needs a build-time guard plus a test.
5. **Privacy (BC PIPA).** Decide data residency (Canadian hosting preferred), inbox
   retention, audit logging, breach response, and the privacy policy.
6. **Plain-language summaries are a liability risk.** Summaries can drift from policy
   wording. Every screen needs a "see your policy wording" path, and the copy needs
   compliance review (Insurance Council of BC).
7. **App store requirements.** Account deletion, privacy policy, Face ID usage string,
   data-safety forms, and a demo account for Apple review.
8. **TypeScript version.** TS 7 is current on npm. Use the TS version that Expo SDK 58
   and Next 16 support, not just the newest.
9. **Hosting** for the server and inbox DB hasn't been decided yet (e.g. Vercel + a Canadian-region Postgres).
10. **Branding.** We need Schill's logo, colours and fonts before step 8.

## Launch checklist (step 13)

- [ ] Real auth with MFA + biometrics; demo sign-in compiled out
- [ ] Live Epic connector behind the same interface, read-only creds, tested in sandbox
- [ ] Privacy policy, terms, in-app account deletion
- [ ] Compliance review of all customer-facing copy
- [ ] Accessibility audit (VoiceOver, TalkBack)
- [ ] Monitoring, error tracking, uptime alerts
- [ ] EAS production builds; TestFlight + Play internal testing
