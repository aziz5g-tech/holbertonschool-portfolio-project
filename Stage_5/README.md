# Stage 5: Project Closure — Final Report

> **Project:** Rafeeq — Companion-on-demand platform for seniors and their families
> **Repository:** https://github.com/aziz5g-tech/Rafeeq
> **Production Environment:** https://rafeeqsa.me
> **Stack:** Node.js/Express · React/Vite · MySQL · Firebase (Firestore · FCM · Storage optional) · Local FS storage · Moyasar · Google Maps
> **Architecture:** Modular monolith (single Express API + single React SPA + single MySQL DB) deployed on Hostinger
> **Development window (code history):** 2026-04-21 → 2026-06-15
> **Team size:** 4 members
> **Report date:** 2026-06-15

---

## Purpose of this Report

This document is the Stage 5 **Final Report**. It fulfils the three required deliverable sections:

1. **Results Summary** — the MVP's core functionalities, a comparison of outcomes against the initial objectives, and key metrics.
2. **Lessons Learned** — what went well and why, the challenges faced and how they were addressed, and improvements for future projects.
3. **Team Retrospective Highlights** — the consolidated key points from the team's retrospectives.

> **Note on the comparison baseline:** The project's agreed scope and objectives are recorded in `Docs/product-requirements.md` (the client-flow requirements) and `Docs/companion-specs.md` (the companion-flow specification). This report treats those documents as the Project Charter equivalent for the "compare outcomes to initial objectives" requirement, since they are the authoritative statement of what the MVP committed to deliver.
>
> **Note on figures:** All quantitative figures in this report were measured directly from the repository and Git history on the report date (2026-06-15), not copied from earlier-stage documents. Where an earlier figure has since changed, the current measured value is used.

---

## 1. Results Summary

### 1.1 MVP Core Functionalities

Rafeeq is a working, deployed platform that delivers the full **client → companion → trip → payment → rating** loop, with administrative oversight. The MVP's core functionalities are:

**Client journey**
- Account creation with first/last name, unique mobile and email, and email **OTP verification**.
- Login by **email + password OR mobile + password**.
- Service request creation with a guided multi-step wizard: requester ("for me" / "for someone else"), beneficiary details, mandatory health information (multi-select + free-text "other"), companion service type, one-time vs. recurring, with-car vs. without-car.
- Location selection on an interactive **Google Map** (pickup + destination), with address parts auto-filled from reverse geocoding.
- Immediate and scheduled requests; the available-companions list is ranked **nearest-first** (Haversine distance from live companion locations) and shows name, rating, and distance.
- **Full payment before dispatch** through the **Moyasar** gateway (Visa / Mada / MasterCard), with payment confirmed by both an active verification call and a hardened webhook.
- Live **trip-tracking screen**: a map with the companion's moving position (streamed via Firestore), the driving route + distance + ETA (Google Directions API), vehicle details, and an in-app chat link.
- **Realtime in-app chat** between client and companion (Firestore-backed, backend-only writes).
- Post-trip **star rating + written comment**, and request cancellation (before the trip starts, with full refund on companion rejection).
- A **support-ticket** channel for complaints and suggestions.

**Companion portal**
- Profile, document upload (with admin review), per-service rate management, vehicle details, availability/online status, and live GPS location sharing.
- Incoming-request handling (accept/reject) with a **privacy gate** that masks the client's full identity until acceptance.
- Start-trip / end-trip flow with GPS capture, plus a wallet and withdrawal requests.

**Admin panel**
- Companion review and activation, document-type management, user CRUD/suspension/role changes, withdrawal approvals, support handling, and a runtime **switchable storage backend** (local filesystem ↔ Firebase Storage) toggled without a redeploy.

**Cross-cutting platform capabilities**
- JWT authentication (access + refresh tokens), role-based guards (client / companion / admin), realtime request-lifecycle events (Firestore), push-notification backend (FCM), reference-data API, environment validation, input sanitization, and a security baseline (Helmet, CORS allowlist, rate limiting, Joi validation, Firestore security rules).

### 1.2 Outcomes vs. Initial Objectives

The MVP implemented **100% of the functional requirements** defined in `Docs/product-requirements.md`. The table below maps each committed requirement to its delivery status.

| Committed requirement (Project Charter / `product-requirements.md`) | Status |
|---|---|
| Account: name, unique mobile + email, password hash, account status | ✅ Delivered |
| Login via email **or** mobile (+ password) | ✅ Delivered |
| Email OTP verification on signup | ✅ Delivered |
| "For me / for someone else" request branching | ✅ Delivered |
| Mandatory health data (multi-select list + "other") | ✅ Delivered |
| Beneficiary data (name, age, mobile, gender, relationship) | ✅ Delivered |
| Companion service types (medical, in-home, outdoor, shopping, official, social, digital, custom) | ✅ Delivered |
| One-time / recurring request nature | ✅ Delivered |
| With-car / without-car option | ✅ Delivered |
| Location from map (default) + manual entry; pickup + destination | ✅ Delivered |
| Immediate + scheduled timing | ✅ Delivered |
| Immediate: show available companions **nearest-first** + auto-filter (nearest + rating) | ✅ Delivered |
| Companion info shown: name + rating + distance | ✅ Delivered |
| Scheduled: pay full, then await companion approval | ✅ Delivered |
| Full payment before sending the request (both modes) | ✅ Delivered |
| Companion accept/reject; rejection → **full refund** | ✅ Delivered |
| In-app chat between client and companion | ✅ Delivered |
| Tracking screen: map + vehicle details + chat | ✅ Delivered |
| Pricing: companion sets per-service price; full price shown; price **snapshot** locked at booking | ✅ Delivered |
| Arrival/trip proof via GPS + companion action | ✅ Delivered |
| Service completion controlled by the companion | ✅ Delivered |
| Post-service: rate companion + return home | ✅ Delivered |
| Rating: stars + written comment | ✅ Delivered |
| Cancel after acceptance, before trip start, no cancellation fee | ✅ Delivered |
| Support contact for complaints / suggestions | ✅ Delivered |

**Delivered beyond the original scope.** The team also shipped capabilities that were not part of the original requirement set but strengthen the product: a full admin panel, a companion wallet + withdrawals module, a runtime-switchable storage backend (so the project ships without a paid Firebase plan), live route + ETA on the tracking map, companion availability/proximity with persistent live-location sharing, an FCM push-notification backend, a 94-endpoint Postman collection, a Jest test suite, and a GitHub Actions CI pipeline that runs ESLint + Jest on every pull request.

**Consciously deferred (post-MVP).** Two items were deferred by explicit decision; **neither is part of the documented requirements**:
- **Client-side FCM web push** — the backend is FCM-ready, but the browser client needs a VAPID key + service worker (deployment-side setup outside the academic scope).
- **Offline chat fallback** — a MySQL polling path for when Firebase is not configured; the Firestore-backed chat is the supported path.

### 1.3 Key Metrics and Performance Indicators

All values measured from the repository and Git history on 2026-06-15.

| Metric | Value |
|---|---|
| Functional requirements implemented | **100%** of `product-requirements.md` scope |
| Sprints completed | **4 / 4 (100%)** |
| Sprint velocity (completed/planned) | S1 100% · S2 88% · S3 100% · S4 100% |
| Most recent pull request | **#141** |
| Total commits | **297** (201 excluding merge commits) |
| Merge commits in history | 96 |
| Automated tests (Jest) | **56 / 56 passing** — 9 suites, ~0.4 s, fully mocked (no live DB) |
| API surface | **98** Express route handlers (94 documented in the Postman collection) |
| Documented defects | **13**, 100% resolved within the sprint they were found |
| Mean time to resolve (critical bugs) | **< 24 hours** |
| Manual critical-flow tests | **17 / 17 passing** |
| ESLint warnings on `development` | **0** (CI runs ESLint on every PR) |
| Lines of source code (excl. `node_modules` / build) | **~22,200** (backend `src` ~7,900 + frontend `src` ~14,300) |
| Database tables | **~30** across 28 schema files |
| Backend service domains | 16 |
| Frontend pages | 38 |
| Production uptime incidents in Stage 4 | 1 (503 from missing controller exports, resolved < 30 min) |
| CI/CD | GitHub Actions — ESLint + Jest run on every PR to `main` / `development` |
| Production environment | https://rafeeqsa.me (live) |

> **Branch protection:** the `main` / `development` protection rules (PR + passing CI required, no force-push) are configured as documented intentions. Platform-level enforcement requires a paid GitHub plan, so on the project's free private repository they are not merge-blocking; the CI workflow itself still runs on every pull request.

**Quality-assurance evidence:** ESLint clean as a merge gate on every PR; 56 Jest unit scenarios across 9 service suites; a 94-endpoint Postman collection ready for regression runs; and a 17-flow manual checklist executed against staging with a 100% pass rate (signup/OTP, request creation for self and others, active-request lock, companion accept + privacy unlock, live tracking, Moyasar payment + webhook, rejection refund, realtime chat, lifecycle events, trip end + rating, admin document approval, storage-backend switch, saved-address CRUD, responsive layout at 360/768/1024 px, and RTL rendering).

---

## 2. Lessons Learned

### 2.1 What Went Well — and Why

- **Clear role + domain split eliminated blocking handoffs.** Backend (Abdulaziz, Norah) and frontend (Shatha, Seba) worked in parallel against a shared API contract, so neither side waited on the other. *Why it worked:* responsibilities were assigned by domain up front, and the API shape was agreed before UI work began.
- **A monorepo + a living `STATUS.md` kept everyone synchronized.** A single source of truth, updated on every feature branch, meant any member could resume work in minutes. *Why:* the document was treated as part of the change, not an afterthought.
- **Architectural bets paid off.** Firestore-backed chat and lifecycle events removed the need for any polling implementation; the storage-provider pattern (local ↔ Firebase) let the team ship file uploads with **no paid cloud plan**; the price-snapshot model guaranteed price stability across a booking. *Why:* the team designed for the constraint (no billing account, realtime UX) instead of around it.
- **Process discipline produced a clean, deployable result.** Conventional commits, mandatory PRs, an ESLint merge gate, and (from the closeout onward) a CI pipeline yielded **zero unresolved bugs** and **100% sprint-goal completion** across all four delivery sprints.
- **Small, reviewable PRs.** Large features were split into paired PRs (e.g., the `/me` page, the loading bar, the migration-runner fix), keeping reviews fast and defects easy to localize.

### 2.2 Challenges Faced — and How They Were Addressed

| Challenge | Resolution |
|---|---|
| **Firebase Admin SDK on Hostinger** consumed ~2 unplanned days (the host double-escaped the private key; base64 credentials were unsupported). | Added base64 service-account support, normalized key escaping, and improved credential diagnostics (PRs #94–#96). Documented every Hostinger gotcha in `Docs/hostinger-deploy.md`. |
| **A feature commit left a critical service syntactically broken**, failing the backend on boot. | Rewrote `companion-request.service.js` cleanly from a known-good baseline and re-implemented the auto-greeting as a safe post-commit hook that cannot roll back the acceptance (PR #100). |
| **Built frontend bundles were missing on deploy** (blank page / wrong MIME on `/assets/*`). | Tracked the built bundle in Git, since the shared host cannot reliably run the frontend build. |
| **The Moyasar webhook verification was wrong** (HMAC-over-body vs. the actual `secret_token` field) and listened for the wrong event type — it would have rejected every real webhook. | Rewrote verification to a constant-time `secret_token` compare, accepted the correct `payment_paid` event, and added an idempotency log keyed on the event id. Discovered by reading Moyasar's official docs. |
| **Payments could stay `unpaid` after a successful charge** (the redirect outran the webhook). | Added an active `verifyAndConfirm` endpoint that asks Moyasar for the invoice status on return and flips the payment under a row lock (idempotent). The webhook remains as a backup. |
| **Firebase Storage requires the paid Blaze plan**, which the team had no billing account for. | Built a `local` / `firebase` storage-provider abstraction with an admin toggle, defaulting to local-disk storage. |
| **Shared-component and deploy regressions** (a TopBar nested in `overflow-x:hidden` broke `sticky`; a Hostinger restore silently rolled back the privacy gate; missing controller exports caused a 503). | Fixed each, then adopted preventive rules: sanity-check shared components on ≥3 routes, and add a CI check that `require()`s the app so missing exports cannot land. |
| **Messy first week of Git history** (force-pushed hotfixes lost the review trail). | Adopted a strict no-force-push + PR-only rule from Sprint 2 onward. |

### 2.3 Improvements for Future Projects

- **Adopt the full process discipline from day one.** Conventional commits, PR-only merges, and CI were introduced after a messy first week and a late closeout; starting with them would have prevented the early history churn and the QA-tooling backlog.
- **Write tests alongside features once contracts stabilize**, rather than deferring the whole suite. Jest and Postman were moved to "Won't-Have" during the MVP crunch and only completed in a dedicated closeout day; earlier coverage would have caught the webhook and controller-export bugs sooner.
- **Add a smoke test before merge for any change to a critical-path service** (auth, request lifecycle, payment, chat).
- **Budget explicit time for deployment/integration spikes.** The Firebase-on-Hostinger and Moyasar-webhook integrations both took longer than planned; treating third-party integrations as their own time-boxed spikes would improve estimates.
- **Allocate more time for testing and bug-fixing in the plan**, as a first-class activity rather than a closeout task.

---

## 3. Team Retrospective Highlights

This section consolidates the four sprint retrospectives conducted during development (documented in `Docs/stage4-deliverable.md`) into a project-level retrospective, structured around the standard guiding questions.

### Team roster and roles

| Member | Primary (technical) role | Process role |
|---|---|---|
| **Abdulaziz Alrashedi** | Backend (auth, request lifecycle, payments) | Project Manager |
| **Norah Mohammed Alskran** | Backend + Database Admin | Source Control Manager |
| **Shatha Alsawilem** | Frontend (UI, design system) | QA Lead |
| **Seba** | Frontend + Database Admin | QA (exploratory, UAT) |

### What worked well as a team?

- **Pair-designing the database schema** (Norah + Abdulaziz) caught foreign-key ambiguities before any code was written.
- **The monorepo decision** unblocked frontend and backend developers to work in the same repository without merge pain.
- **Domain-based ownership** meant zero blocking handoffs between the backend and frontend pairs.
- **Centralized design tokens** let a full homepage redesign and TopBar refresh land without leaking styles into other pages.
- **Reusable abstractions** (the storage-provider pattern, the `useActiveRequest` hook) let new features be added in a few lines of consumer code.

### What challenges did we face, and how were they resolved?

- **Third-party integration friction** (Firebase Admin on Hostinger, the Moyasar webhook) cost unplanned days; resolved by reading official docs carefully and documenting every gotcha for the team.
- **Regressions introduced by a deploy-hardening commit** (privacy gate, tracking fields) took ~1 hour of "git archaeology" to diagnose; resolved by restoring from a known-good baseline and adopting smoke-test-before-merge.
- **A production 503** from missing controller exports slipped past local development because it only fails at route-registration time; resolved with a CI check that loads the app on every PR.
- **Early Git-history churn** from force-pushed hotfixes; resolved by enforcing PR-only, no-force-push from Sprint 2.

### How can we improve collaboration in the future?

- Enforce the SCM/QA process (PRs, CI, branch protection) **from the first commit**, not after the first sprint.
- Pair on or smoke-test any change to a **critical shared service** before merge.
- Keep the daily async stand-up and the living `STATUS.md` — both were highly effective and should be standard practice on future projects.
- Treat **testing as a continuous, planned activity**, not an end-of-stage catch-up.

### Quantified retrospective outcomes

- **Sprint goals met:** 4 / 4 (velocities 100% / 88% / 100% / 100%).
- **Defects:** 13 found, **13 resolved within their own sprint** (critical MTTR < 24 h).
- **Plan deviations:** 4, each handled by an explicit, documented re-scope (Jest + FCM web push deferred; trip tracking promoted to Must-Have; switchable storage added mid-sprint when the Blaze-plan constraint surfaced).

---

## 4. Conclusion

A four-person team delivered a **complete, deployed MVP** of Rafeeq in roughly eight weeks of code history, implementing **100% of the committed functional requirements** plus several beyond-scope capabilities, with **zero unresolved defects**, a clean ESLint history, a 56-scenario automated test suite, a 94-endpoint API documented in Postman, and a CI pipeline running ESLint + Jest on every pull request. The product is live at https://rafeeqsa.me and exercises the full client → companion → trip → payment → rating loop with administrative oversight.

The project's main lesson is that **process discipline and architectural decisions made for the constraints** (realtime UX without polling, file storage without a paid plan, price stability across bookings) were the largest contributors to a clean delivery — and that the one recurring cost was **deferring testing and integration hardening**, which future projects should plan as first-class, continuous work from the outset.
