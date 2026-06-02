# Stage 4: MVP Development and Execution — Rafeeq

> **Project:** Rafeeq — Companion-on-demand platform for seniors and their families
> **Repository:** https://github.com/aziz5g-tech/Rafeeq
> **Production Environment:** https://rafeeqsa.me
> **Stack:** Node.js/Express · React/Vite · MySQL · Firebase (Firestore/FCM, Storage optional) · Local FS storage · Moyasar
> **Architecture:** Modular monolith (single Express app + single React SPA + single MySQL DB) deployed on Hostinger
> **Duration:** 2026-04-29 → 2026-06-02 (~5 weeks, including Stage 4 closeout day)
> **Team Size:** 4 members

---

## Team Roles

Roles cover both technical responsibilities (per `Docs/scm-and-qa.md`) and the non-technical roles required for Stage 4. To keep the 4-person team workable, every member carries a primary technical role plus a process role.

| Member | Primary Role | Process Role | Secondary Domain |
|--------|--------------|--------------|------------------|
| **Abdulaziz Alrashedi** | Backend Developer (auth, request lifecycle, payments) | **Project Manager (PM)** — sprint planning, deadlines, deviation handling | Repo owner; coordinates demos |
| **Norah** | Backend Developer + Database Admin | **Source Control Manager (SCM)** — branch protection, PR reviews, merge gating | DB schema and migrations |
| **Shatha** | Frontend Developer (UI components, design system) | **Quality Assurance Lead** — test planning, manual flow checklist, regression sign-off | Companion portal flows |
| **Seba** | Frontend Developer + Database Admin | **Quality Assurance** — exploratory testing, bug intake, UAT | Client journey + Admin panel |

Testing ownership is shared across the whole team as documented in `Docs/scm-and-qa.md`.

---

## Task 0 — Plan and Define Sprints

The Stage 3 user stories were broken down into four sprints, each 1–2 weeks long, scoped by MoSCoW. The MVP scope was defined as: **a working client → companion → trip → payment loop, with admin oversight**, leaving Jest unit tests, FCM web push, and a few non-critical UI flows for post-MVP iteration.

### Sprint backlog overview

| Sprint | Dates | Goal | Outcome |
|--------|-------|------|---------|
| Sprint 1 — Foundation | 2026-04-29 → 2026-05-09 | Repo structure, DB schema, authentication groundwork | ✅ Shipped |
| Sprint 2 — Core MVP | 2026-05-10 → 2026-05-20 | Full request lifecycle, payments, realtime chat, responsive UI | ✅ Shipped |
| Sprint 3 — UX Polish | 2026-05-21 → 2026-05-26 | Navigation overhaul, maps picker, homepage redesign, privacy gate | ✅ Shipped |
| Sprint 4 — Advanced Features | 2026-05-27 → 2026-06-01 | Request stepper, active-request lock, switchable storage, trip tracking | ✅ Shipped |

### Sprint 1 — Foundation (2026-04-29 → 2026-05-09)

**Goal:** Establish the technical foundation so the team can build features in parallel.

| Priority | Task | Owner | Deadline | Dependency |
|----------|------|-------|----------|-----------|
| Must | Design MySQL schema (users, OTP, consents) | Norah | 2026-04-30 | — |
| Must | Service requests & logistics tables | Norah | 2026-04-30 | users table |
| Must | Companion services schema | Norah | 2026-04-30 | users table |
| Must | Financial & feedback tables | Norah | 2026-04-30 | service requests |
| Must | Restructure project into backend/frontend monorepo | Abdulaziz | 2026-05-03 | — |
| Must | Initial login page (static HTML) | Shatha | 2026-05-05 | — |
| Should | Cities reference table | Norah | 2026-05-03 | — |
| Should | Initial project README + docs structure | Abdulaziz | 2026-05-03 | — |
| Could | Migration runner with checksum tracking | Abdulaziz | 2026-05-08 | DB schema |

**Dependencies map:** All schema work blocks API development; monorepo restructure blocks frontend/backend split.

### Sprint 2 — Core MVP (2026-05-10 → 2026-05-20)

**Goal:** Deliver a working end-to-end MVP: a client can create a request, a companion can accept/reject, payment flows, and chat works in real time.

| Priority | Task | Owner | Deadline | Dependency |
|----------|------|-------|----------|-----------|
| Must | JWT-based auth + OTP email verification | Abdulaziz | 2026-05-12 | DB schema |
| Must | Firebase Admin SDK integration (Firestore + Storage) | Abdulaziz | 2026-05-14 | env vars |
| Must | Service request lifecycle (create, assign, accept/reject, trip, cancel) | Abdulaziz + Norah | 2026-05-17 | auth |
| Must | Moyasar payment integration (live + demo mode) | Norah | 2026-05-17 | request lifecycle |
| Must | Realtime chat (Firestore-backed, backend-only writes) | Abdulaziz | 2026-05-18 | Firebase |
| Must | Realtime request lifecycle events | Abdulaziz | 2026-05-20 | Firebase rules |
| Must | Client journey UI (Signup → CreateRequest → Selection → Payment → Detail) | Seba | 2026-05-18 | API |
| Must | Companion portal UI (Dashboard, Requests, Profile, Documents, Rates, Wallet) | Shatha | 2026-05-18 | API |
| Must | Admin panel (Companions, Users, Withdrawals, Document Types, Support) | Shatha + Seba | 2026-05-19 | API |
| Must | Responsive design + hamburger nav + wider list cards | Shatha | 2026-05-20 | UI scaffolding |
| Should | Saved addresses + beneficiaries + health conditions for client | Norah + Seba | 2026-05-19 | client UI |
| Should | `/me` tabbed page (addresses · beneficiaries · health) | Seba | 2026-05-19 | saved data API |
| Should | Hostinger deployment guide + production hardening | Abdulaziz | 2026-05-20 | working backend |
| Could | Auto-greeting message from companion on accept | Abdulaziz | 2026-05-20 | chat |
| Could | Google Maps location picker (runtime-loaded key) | Seba | 2026-05-22 | Maps API key |
| Won't | Jest unit/integration tests (deferred per QA plan, post frontend integration) | — | — | — |
| Won't | FCM client-side web push (deferred; backend ready, client needs VAPID + service worker) | — | — | — |

### Sprint 3 — UX Polish (2026-05-21 → 2026-05-26)

**Goal:** Refine navigation, address UX feedback from internal testing, and ship a redesigned homepage.

| Priority | Task | Owner | Deadline | Dependency |
|----------|------|-------|----------|-----------|
| Must | Navigation overhaul — role-aware TopBar, token auto-refresh, longer session defaults | Shatha + Seba | 2026-05-22 | session API |
| Must | Route restructure for role symmetry (`/dashboard` → `/client/dashboard`, etc.) | Seba | 2026-05-22 | TopBar |
| Must | Breadcrumbs on every page (above form card) | Shatha | 2026-05-22 | route restructure |
| Must | Top loading bar on every route change (perceived performance) | Shatha | 2026-05-23 | router events |
| Must | Companion privacy gate (mask client mobile + last name until accept) | Abdulaziz | 2026-05-23 | request API |
| Must | Streamlined Google Maps picker — gates the city/district/street manual fields | Seba | 2026-05-23 | picker baseline |
| Must | Homepage redesign — Tajawal + IBM Plex, navy/cream/sky palette, prototype layout | Shatha | 2026-05-26 | new color tokens |
| Must | Floating transparent TopBar + unified cream background across pages | Shatha | 2026-05-26 | homepage redesign |
| Should | Card width unification (540px → 960px) | Shatha | 2026-05-22 | — |
| Should | Migration runner skips already-applied column ALTERs | Norah | 2026-05-26 | migrations |

### Sprint 4 — Advanced Features (2026-05-27 → 2026-06-01)

**Goal:** Close the request flow with a guided multi-step wizard, lock clients to one active request at a time, abstract storage so the project no longer depends on Firebase Storage (Blaze plan), and deliver the live trip-tracking experience.

| Priority | Task | Owner | Deadline | Dependency |
|----------|------|-------|----------|-----------|
| Must | 4-step visual stepper (تفاصيل → موقع → رفيق → دفع) | Shatha | 2026-05-28 | request flow |
| Must | CreateRequest split into 2-step wizard with per-step validation | Seba | 2026-05-28 | stepper |
| Must | Backend active-request lock (`requireNoActiveRequest` middleware, 409 with redirect id) | Abdulaziz | 2026-05-28 | request API |
| Must | TopBar active-request banner with auto-redirect on race | Seba | 2026-05-28 | active request hook |
| Must | Companion message toast on home for active client | Seba | 2026-05-28 | chat |
| Must | Switchable storage backend (Local FS default + Firebase Storage via admin toggle) | Abdulaziz + Norah | 2026-05-29 | admin settings |
| Must | Firebase Storage setup guide + actionable error mapping | Abdulaziz | 2026-05-29 | storage abstraction |
| Must | Client trip tracking screen (live map, companion car icon, pickup house icon) | Seba | 2026-06-01 | tracking service |
| Must | Companion trip tracking screen (`watchPosition` + 10s backend ping, car/house icons) | Shatha | 2026-06-01 | client tracking |
| Should | Fix admin storage-backend boot crash (controller export) | Abdulaziz | 2026-05-29 | storage backend |
| Should | Fix stale `/admin/select-companion` links to `/admin/companions` | Shatha | 2026-05-29 | route audit |
| Could | Update README with project links | Abdulaziz | 2026-05-31 | — |

### Post-Sprint Hardening — Stage 4 Closeout (2026-06-02)

**Goal:** Close the QA-tooling gap before academic submission. The original Sprint 2 plan marked Jest and Postman as `Won't Have` to protect the MVP delivery window; once Sprint 4 closed cleanly, the team allocated a single closeout day to bring the QA tooling up to the level promised in `Docs/scm-and-qa.md`.

| Priority | Task | Owner | Deadline | Dependency |
|----------|------|-------|----------|-----------|
| Must | Local docs cleanup + memory sync (STATUS, session-handoff) | Abdulaziz | 2026-06-02 | — |
| Must | Fix `POST /api/admin/users` validator gap (`gender` + `date_of_birth`) | Abdulaziz | 2026-06-02 | users table schema |
| Must | Unify JWT expiry defaults + drop hardcoded 7-day refresh TTL | Abdulaziz | 2026-06-02 | env-validator |
| Must | Postman collection covering all 94 API endpoints | Abdulaziz | 2026-06-02 | — |
| Must | Jest unit tests for 6 core services (33 scenarios) | Abdulaziz | 2026-06-02 | services stable |
| Must | GitHub Actions CI workflow (lint + tests on every PR) | Abdulaziz | 2026-06-02 | Jest baseline |
| Must | Branch protection rules on `main` and `development` | Abdulaziz | 2026-06-02 | CI workflow |
| Must | GitHub Project board sync (cards aligned with merged PRs) | Abdulaziz | 2026-06-02 | — |

---

## Task 1 — Execute Development Tasks

Development followed the workflow defined in `Docs/scm-and-qa.md`:

1. **Branch naming.** Every change went on a typed branch off `development`: `feature/<scope>` for new functionality, `fix/<scope>` for bug fixes, `docs/<scope>` for documentation, `chore/<scope>` for tooling.
2. **Commits.** Conventional-commit style (`feat:`, `fix:`, `docs:`, `chore:`) with imperative subjects and a short body when the *why* was non-obvious.
3. **Pull requests.** No direct pushes to `main` or `development`. Every PR linked to its sprint task and required one passing review.
4. **Linting gate.** ESLint must be clean (`npm run lint`) before merge.
5. **Manual test gate.** SCM and QA roles re-ran the critical flow (signup → request → accept → pay → chat → complete → rate) on every PR touching that path.
6. **Documentation gate.** `Docs/STATUS.md` updated as part of every feature branch (not a separate `docs/*` branch) so the project record stays current.

### Pull request volume

Stage 4 produced **108 merged pull requests** across the 5-week window (PR #1 through #108 visible at https://github.com/aziz5g-tech/Rafeeq/pulls?q=is%3Apr+is%3Aclosed). Highlights:

- **PR #94–#96** — Firebase service-account loading hardening (3 PRs across the same week, evidence of incremental debugging on a tricky integration).
- **PR #97 + #99** — Saved client data + `/me` tabbed page, shipped as two related PRs to keep reviews small.
- **PR #100** — Hostinger deployment hardening (CORS auto-reflect, `postinstall` removal, migrate script, restored `companion-request.service.js`).
- **PR #101 + #102** — Top loading bar + companion-greeting rollout (paired PRs).
- **PR #103 + #104** — Migration runner skips column-adding migrations when the column already exists.
- **PR #105 + #106** — Map picker cleanup.
- **PR #107** — Request flow stepper + active-request lock + companion message toast (single large feature, 12 files).
- **PR #108** — Switchable storage backend + admin toggle.

### Coding standards enforced

- **Backend** is CommonJS by design (Hostinger Node compatibility); IDE suggestions to convert to ESM are intentionally ignored.
- **All API routes** live under `/api/*` to avoid collisions with the React Router SPA; only `/webhooks` lives at root for Moyasar.
- **API responses** follow `{ error: { code, message, details } }` for errors; success payloads are typed objects (no envelope).
- **Firestore writes** happen backend-only; the security rules deny client writes and gate reads on participant arrays.
- **Built frontend bundles** in `backend/public/assets/` are tracked in git because Hostinger Git-Deploy cannot reliably run `npm run build:frontend` on the shared Node host.

---

## Task 2 — Monitor Progress and Adjust

Progress was tracked through four lightweight loops:

1. **Daily stand-ups** (15 min, asynchronous in the team Discord channel) — what each member shipped yesterday, what's blocking today, dependencies on others.
2. **GitHub Project board** (https://github.com/users/aziz5g-tech/projects/2) — Kanban-style columns reflecting sprint state (`Backlog`, `In Progress`, `In Review`, `Done`). Cards are linked to Issues and PRs so transitions happen automatically when PRs merge. The PM uses the board for sprint reviews; the SCM uses it to verify every merged PR maps back to a planned task.
3. **`Docs/STATUS.md`** as a living single-source-of-truth, updated per feature branch. PM and SCM scan it before each sprint review.
4. **`Docs/session-handoff.md`** — captures the *active context* for the next conversation (next branch to pick up, recent decisions, gotchas) so a member returning from a few days off can resume in minutes instead of hours. (Kept local-only / `gitignored` because it changes daily and would generate constant merge noise.)

### Sprint velocity

Velocity is measured in shipped tasks per sprint (where a "task" = a feature/fix branch that made it into `development`). The team's velocity stabilized after Sprint 2:

| Sprint | Planned Tasks | Completed Tasks | Velocity | Notes |
|--------|---------------|------------------|----------|-------|
| Sprint 1 | 9 | 9 | 100% | Foundation work proceeded on schedule |
| Sprint 2 | 17 | 15 | 88% | 2 tasks (Jest tests, FCM client) explicitly moved to Won't-Have |
| Sprint 3 | 10 | 10 | 100% | Smaller scope, finished a day early |
| Sprint 4 | 12 | 12 | 100% | Trip tracking landed on the final day of the sprint |

### Bug count and resolution

Bugs are tracked through the `fix/<scope>` branch convention rather than a dedicated tracker. A search of merged PRs and direct commits with `fix:` prefix yields the following Stage 4 defects, all resolved before the sprint they were introduced in closed:

| # | Bug | Discovered In | Resolved In | Severity |
|---|-----|---------------|-------------|----------|
| 1 | Firebase private key double-escaping by Hostinger UI | Sprint 2 | PR #96 (`fix/firebase-private-key-double-escape`) | High |
| 2 | Firebase service account loading via base64 not supported | Sprint 2 | PR #94 (`fix/firebase-credentials-base64-support`) | High |
| 3 | Firebase credential errors gave no diagnostic info | Sprint 2 | PR #95 (`fix/firebase-credentials-diagnostics`) | Medium |
| 4 | Companion document page crashed on data mismatch | Sprint 2 | `156378f` (`fix/companion-document-data-mismatch`) | High |
| 5 | Hostinger Node.js boot was unreliable (postinstall, CORS, missing migrate script) | Sprint 2/3 | PR #100 (`fix/hostinger-deploy-robustness`) | Critical |
| 6 | `companion-request.service.js` left syntactically broken by auto-greeting commit | Sprint 2 | Restored in PR #100 from `feature/maps-picker` baseline | Critical |
| 7 | Mobile nav panel was visible by default in RTL on desktop | Sprint 2 | `c6506fd` | Medium |
| 8 | Built bundles missing on Hostinger deploys (404 on `/assets/*`) | Sprint 3 | `ce38fc2` (`fix/deploy-built-bundles`) | Critical |
| 9 | Migration runner crashed with `Duplicate column name` on fresh DB | Sprint 3 | PR #103/#104 (`fix/migration-skip-existing-columns`) | High |
| 10 | TopBar covered by `.home-v2 { overflow-x: hidden }` — sticky broke + login text color leaked | Sprint 4 | In `feature/homepage-redesign` follow-up | Medium |
| 11 | Admin storage-backend controller exports missing → 503 on every API hit | Sprint 4 | `619edb5` (`fix/admin-storage-exports`) | Critical |
| 12 | Stale `/admin/select-companion` links → blank admin page | Sprint 4 | PR `76023b8` (`fix/admin-select-companion-link`) | Medium |
| 13 | Companion privacy gate regressed during Hostinger restoration | Sprint 4 | Restored in `feature/trip-tracking-screen` | Medium |

**Bug resolution rate:** 13/13 = 100% within the sprint they were found. **Mean time to resolve (critical):** under 24 hours.

### Plan deviations

| Deviation | Sprint | Cause | Action Taken |
|-----------|--------|-------|--------------|
| Jest unit tests skipped | Sprint 2 | Tight MVP window + decision to integrate frontend before locking down test contracts | Re-scoped to Won't-Have for MVP; ESLint + manual testing fill the gap (see Task 4) |
| FCM web push client deferred | Sprint 2 | Requires VAPID key + service worker + extra deploy steps | Moved to post-MVP roadmap; backend `notification.service.js` is FCM-ready |
| Trip tracking screen carried into Sprint 4 | Sprint 4 | Originally a Sprint 3 "should-have" — promoted to Must-Have once admin/companion flows were polished | Allocated full Sprint 4 capacity from two members |
| Switchable storage backend added mid-sprint | Sprint 4 | Discovered Firebase Storage requires Blaze plan; team has no billing account yet | Architected `local`/`firebase` provider pattern with admin toggle so the project can ship without a credit card |

---

## Task 3 — Sprint Reviews and Retrospectives

### Sprint 1 Review (2026-05-09)

**Demo:** ERD walkthrough, monorepo structure (`backend/`, `frontend/`, `Docs/`), initial login page rendering with the design system's primary navy.

**Stakeholders saw:** Project skeleton ready for parallel backend and frontend work.

#### Sprint 1 Retrospective

| Question | Answer |
|----------|--------|
| **What worked well?** | Pair-design of the schema between Norah (DB) and Abdulaziz (API) caught FK ambiguities before any code was written. Monorepo decision unblocked frontend and backend developers to work in the same repo without merge pain. |
| **What didn't work?** | Early commits (`Feat: addming database info` × 7) showed messy iteration on the schema instead of clean migration deltas. Some hotfix commits were force-pushed in the first week, losing review trail — this was the lesson that drove the strict no-force-push rule adopted from Sprint 2 onward. |
| **Improvements for next sprint** | Adopt conventional commits strictly. Every change goes through a PR — no direct pushes to `development`. Track DB changes as numbered migration files, not as edits to the seed schema. |

### Sprint 2 Review (2026-05-20)

**Demo:** End-to-end happy path live on staging: signup with OTP → create a request as "for someone else" → see the available companions ranked by rating → assign one → trigger Moyasar checkout (demo mode) → companion accepts → realtime chat opens with the auto-greeting → trip starts and ends → client rates 5 stars. Admin panel demoed approving a companion document.

**Stakeholders saw:** The full client → companion → trip → payment loop. Responsive layout demoed on mobile.

#### Sprint 2 Retrospective

| Question | Answer |
|----------|--------|
| **What worked well?** | Splitting backend (Abdulaziz/Norah) and frontend (Shatha/Seba) by domain meant zero blocking handoffs. Firestore-backed chat avoided a polling implementation entirely. Demo mode for payments unblocked stakeholder demos without burning Moyasar API calls. |
| **What didn't work?** | Firebase Admin SDK setup on Hostinger consumed an unplanned 2 days (3 separate PRs on credentials). Auto-greeting feature corrupted `companion-request.service.js` and had to be reverted+rebuilt. |
| **Improvements for next sprint** | Document Hostinger gotchas as we discover them (resulted in `Docs/hostinger-deploy.md` + `Docs/chat-setup-guide.md`). Any new feature touching a critical service must include a smoke test before merge. |

### Sprint 3 Review (2026-05-26)

**Demo:** New homepage with the prototype-based design, floating pill nav, unified cream background, role-aware CTAs. Top loading bar visible on route changes. Improved companion privacy on incoming-request page (no mobile number until accept). Streamlined Maps picker — no manual fields, just click-to-place.

**Stakeholders saw:** The product looks production-ready. The homepage now reflects the brand identity established in design.

#### Sprint 3 Retrospective

| Question | Answer |
|----------|--------|
| **What worked well?** | Design tokens (`--cream`, `--navy`, `--sky`, `--f-display`, `--f-body`) centralized in `global.css` made the homepage redesign + TopBar refresh land cleanly without leaking into other pages. The migration runner column-existence check unblocked fresh-DB setup permanently. |
| **What didn't work?** | TopBar component was nested inside `.home-v2 { overflow-x: hidden }`, which silently broke `position: sticky` and `color: inherit` ate the white login text. Required 2 fix commits to diagnose and resolve. Three rebuilds of the production bundle landed in the same day. |
| **Improvements for next sprint** | When refactoring shared components, sanity-check them on at least 3 routes (home + form page + dashboard) before merge. Add a 404 catch-all route so the next stale link issue surfaces with a clear error instead of a blank screen. |

### Sprint 4 Review (2026-06-01)

**Demo:** Full client journey through the 4-step stepper (تفاصيل → الموقع → الرفيق → الدفع). After payment, the active-request banner anchors itself under the floating header on every page. After the companion accepts, the client opens the live trip-tracking screen — Google Map with a green house icon at pickup and a navy car icon that moves as the companion drives. The companion's mirror view ships their location every 10 seconds via `watchPosition`. Admin demoed switching the storage backend from Local to Firebase (and back) from the settings page without restarting the server.

**Stakeholders saw:** MVP-complete platform. Client cannot create a second request while one is active. Companion sees client's full identity only after accepting. File uploads work without any Firebase Storage configuration.

#### Sprint 4 Retrospective

| Question | Answer |
|----------|--------|
| **What worked well?** | The provider pattern for storage paid off immediately: the team built the whole feature, shipped to staging, and verified file uploads worked **before** any Firebase Storage decision. Splitting CreateRequest into a 2-step wizard with per-step validation made the form feel half as long without removing any field. The `useActiveRequest` hook centralized banner + redirect + guard logic so each consumer is 5 lines of code. |
| **What didn't work?** | Sprint 4 inherited two regressions from the Hostinger fix commit in Sprint 3: the companion privacy gate and the trip-tracking-related backend fields were silently rolled back. Diagnosing the regression took ~1 hour of git archeology. A 503 on the admin panel slipped past local development because the missing controller exports only fail at route registration time, not at module load. |
| **Improvements for next sprint** | Adopt a "smoke test before merge" rule for any commit that changes a service used in critical paths (auth, request lifecycle, payment, chat). Add a CI workflow that runs `node -e "require('./src/app')"` on every PR so missing exports cannot land. Begin writing Jest tests now that the MVP scope is stable. |

---

## Task 4 — Final Integration and QA Testing

### Integration approach

Because the MVP is a monolith deployed as a single Node.js application serving its own SPA bundle from `/public`, integration testing was performed continuously rather than as an end-of-stage activity:

- The Vite dev server proxies `/api/*` to the local backend, so the frontend always tests against the **real** backend during development.
- Production deploys ship the bundled SPA alongside the same Express app, so what we test locally is what production runs.
- Firestore rules are deployed manually after rule changes and verified against the staged data.

### QA tooling actually used

| Tool | Coverage | Status |
|------|----------|--------|
| **ESLint** (`npm run lint` → `eslint .`) | Both `backend/` and `frontend/` | ✅ Clean as merge gate on every PR; also enforced by CI |
| **Jest** (`npm test`) | 6 core services: `auth`, `payment`, `companion-request`, `request`, `admin.user`, `wallet` | ✅ **33 unit tests** passing in ~9 s (added in closeout — see Post-Sprint Hardening) |
| **Postman collection** ([Rafeeq.postman_collection.json](./Rafeeq.postman_collection.json)) | All 94 API endpoints in 25 folders, with body examples and environment variables | ✅ Importable artifact, ready for regression runs (added in closeout) |
| **GitHub Actions CI/CD** (`.github/workflows/ci.yml`) | Backend ESLint + Jest on every PR to `development` and `main` | ✅ Required check before merge (added in closeout) |
| **Manual critical-flow checklist** (from `Docs/scm-and-qa.md`) | Auth, request, trip, payment, admin, realtime | ✅ Executed before every release-to-staging |
| **Browser DevTools** | Frontend errors, network failures, layout regressions | ✅ Used by all team members during development |
| **Hostinger Application Logs** | Production runtime errors, boot failures | ✅ Used after every deploy |
| **Firebase Console** | Firestore writes and Storage uploads in real time | ✅ Used to verify chat + tracking and document uploads |
| **Manual API calls (cURL + browser DevTools)** | Backend endpoints during development | ✅ Used by API authors |
| Supertest (HTTP integration tests) | End-to-end API flow tests | ⏸ Post-MVP — Jest baseline covers service-layer logic; HTTP-layer tests deferred to Stage 5 |

### Manual critical-flow test results

The checklist below was executed against staging at the end of Sprint 4 (2026-06-01). All flows passed.

| # | Flow | Steps | Result |
|---|------|-------|--------|
| 1 | Client signup + OTP | Email + password → OTP email → verify | ✅ Pass |
| 2 | Client creates request for self | Login → CreateRequest wizard step 1 (service, health) → step 2 (pickup map) → submit | ✅ Pass |
| 3 | Client creates request for other | Same as #2 but with beneficiary fields | ✅ Pass |
| 4 | Client cannot create a second active request | Has pending request → tries `/client/requests/new` → 409 + auto-redirect to active | ✅ Pass |
| 5 | Companion accepts and starts trip | View pending request → see masked client info → accept → mobile and last name unlock → start trip → arrives on tracking screen | ✅ Pass |
| 6 | Live trip tracking visible to client | Companion's car icon moves on map as they drive; client sees "موقع الرفيق مباشر" | ✅ Pass |
| 7 | Payment in demo mode | Admin toggles demo → request → payment auto-marks paid | ✅ Pass |
| 8 | Payment via Moyasar (live) | Admin toggles live → request → Moyasar checkout → webhook with HMAC verification | ✅ Pass |
| 9 | Companion rejects → refund | Companion rejects → backend triggers `paymentService.refundForRequest` → request reset to pending with payment_status=refunded | ✅ Pass |
| 10 | Realtime chat | Client and companion send messages → Firestore writes via backend → other side sees message instantly | ✅ Pass |
| 11 | Realtime lifecycle events | Companion accepts → client banner updates without refresh | ✅ Pass |
| 12 | Trip end + rate | Companion ends trip → client sees rating form → submits 5 stars + comment | ✅ Pass |
| 13 | Admin approves companion documents | Admin views uploaded document via signed URL → approves → companion onboarding status recalculates | ✅ Pass |
| 14 | Admin switches storage backend | `/admin/settings` → Local → Firebase → upload new doc → switch back to Local → old Firebase doc still loads | ✅ Pass |
| 15 | Saved address CRUD | Save during request → see pill → delete pill → record removed | ✅ Pass |
| 16 | Responsive layout | Test #1–#15 at 360px, 768px, 1024px viewports | ✅ Pass |
| 17 | RTL rendering | Verify Arabic text direction across all pages | ✅ Pass |

### Outstanding bugs at end of Sprint 4

**None blocking MVP.** All items that were deferred-by-design in Sprint 2 (Forgot Password UI, Support Tickets UI, Postman, Jest) shipped either in late Sprint 4 (PRs #110–#113) or during the Stage 4 Closeout day (PRs #114–#119). The only remaining item from the original `Won't Have` list is FCM client-side web push, which is genuinely post-MVP because it requires deployment-side setup (VAPID key + service worker) the academic stage does not require.

---

## Task 5 — Deliverables

| Deliverable | Link / Evidence |
|-------------|-----------------|
| **Source repository** | https://github.com/aziz5g-tech/Rafeeq |
| **Production environment** | https://rafeeqsa.me |
| **Sprint planning** | This document (Task 0) — 4 sprints + Stage 4 Closeout × ~15 tasks each with MoSCoW priorities, owners, deadlines, dependencies |
| **Sprint reviews** | This document (Task 3) — 4 reviews with demo content for stakeholders |
| **Sprint retrospectives** | This document (Task 3) — 4 retros following the standard "what worked / what didn't / improvements" template |
| **Project management board** | GitHub Projects: https://github.com/users/aziz5g-tech/projects/2 — cards linked to Issues and PRs; columns reflect sprint state |
| **Bug tracking** | `fix/<scope>` branch naming convention + `Docs/STATUS.md` fix log — see Task 2 bug table. Full history: `git log --all --oneline \| grep "^[a-f0-9]* fix:"` |
| **Automated test suite** | `backend/tests/unit/*.test.js` — Jest, 33 scenarios across 6 services. Run with `cd backend && npm test`. See Task 4 QA tooling table |
| **API documentation** | [Rafeeq.postman_collection.json](./Rafeeq.postman_collection.json) — Postman v2.1 collection, 94 endpoints in 25 folders, importable as-is |
| **CI/CD pipeline** | `.github/workflows/ci.yml` — runs ESLint + Jest on every PR. Status visible at https://github.com/aziz5g-tech/Rafeeq/actions |
| **Branch protection** | Configured on `main` (PR + 1 review + CI required) and `development` (PR + CI required). Visible at https://github.com/aziz5g-tech/Rafeeq/settings/branches |
| **Testing evidence and results** | Task 4 manual critical-flow table (17 flows, 100% pass) + Jest 33/33 passing + ESLint-clean on all merged PRs (enforced by CI) |
| **Process documentation** | `Docs/scm-and-qa.md` (SCM and QA strategy) · `Docs/architecture.md` · `Docs/companion-specs.md` · `Docs/product-requirements.md` |

### Key Pull Requests (chronological)

**Core MVP delivery (Sprints 1–4):**
- **PR #94, #95, #96** — Firebase service account credential loading (3 iterations to handle Hostinger's escape behavior)
- **PR #97** — Saved client data + optional request fields
- **PR #99** — `/me` tabbed management page
- **PR #100** — Hostinger deployment hardening (CORS, postinstall, migrate script, restored companion service)
- **PR #101, #102** — Top loading bar
- **PR #103, #104** — Migration runner skips existing columns
- **PR #105, #106** — Map picker cleanup
- **PR #107** — Request flow stepper + active-request lock + companion message toast
- **PR #108** — Switchable storage backend with admin toggle
- **PR #109** — Client + Companion trip-tracking screens (live map, car/house icons)
- **PR #110** — Login by mobile (email-XOR-mobile toggle in UI)
- **PR #111** — Support tickets user-facing UI (Support.jsx + SupportTicketDetail.jsx)
- **PR #112** — Forgot password + Reset password flow
- **PR #113** — Admin reject-document modal (replaces `window.prompt`)

**Stage 4 Closeout (2026-06-02):**
- **PR #114** — Docs cleanup: STATUS and session-handoff sync; untrack local-only docs
- **PR #115** — Fix `POST /api/admin/users` (`gender` + `date_of_birth` required by schema)
- **PR #116** — JWT expiry consistency: unify defaults + replace hardcoded 7-day refresh TTL
- **PR #117** — Postman collection covering all 94 API endpoints
- **PR #118** — Jest unit tests for 6 core services (33 scenarios)
- **PR #119** — GitHub Actions CI workflow (lint + tests on every PR)

### Project metrics at Stage 4 close

| Metric | Value |
|--------|-------|
| Sprints planned | 4 (+ Stage 4 Closeout day) |
| Sprints completed | 4 / 4 (100%) |
| Total merged pull requests | 119 |
| Total commits (excluding merges) | ~130 |
| Total bugs resolved | 13 (all within the sprint they were found) |
| Critical-flow tests passing | 17 / 17 |
| **Automated Jest tests passing** | **33 / 33** (~9 s runtime) |
| **API endpoints documented in Postman** | **94 across 25 folders** |
| **CI runs required before merge** | Backend lint + Jest (on every PR to `main` or `development`) |
| ESLint warnings on `development` | 0 |
| Production downtime in Stage 4 | < 30 minutes (single 503 incident from missing controller exports, resolved < 30 min) |
| Lines of code (frontend + backend, no `node_modules`/build) | ~18,000 |

---

## Reflection

The combination of a small team (4 people), well-defined roles, conventional commits, mandatory PRs, and a continuously-updated `STATUS.md` produced a deployable MVP in 5 weeks with zero unresolved bugs and 100% sprint goal completion across all four delivery sprints.

The two QA-tooling deviations from the original plan (Jest + Postman, both moved to `Won't Have` during Sprint 2) were conscious trade-offs: shipping a working MVP that all stakeholders can interact with was prioritized over test scaffolding that would have added value primarily after the API contracts stabilized. Once Sprint 4 closed cleanly, a dedicated **Stage 4 Closeout day** (2026-06-02) brought the QA tooling up to the level promised in `Docs/scm-and-qa.md`: a 94-endpoint Postman collection, a 33-test Jest baseline, a GitHub Actions CI workflow that runs both on every PR, and branch protection rules that enforce the SCM strategy at the platform level. Combined with the existing critical-flow checklist and clean ESLint history, this closes Task 4 (Final Integration and QA Testing) with both manual and automated coverage in place.
