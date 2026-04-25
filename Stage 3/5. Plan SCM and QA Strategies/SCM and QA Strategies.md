# SCM and QA Strategies (Team Practical Version)

## Purpose
To establish practical procedures for managing code, the development lifecycle, and ensuring quality for the Stage 3 project.

## Team Structure
- Abdulaziz and Norah: Backend APIs (`Node.js`/`Express`), auth, request lifecycle, and API integration.
- Shatha and Seba: UX/UI and Frontend (`React`) user flows.
- Norah and Seba: Databases (`MySQL`, `Firebase Firestore`, `Firebase Storage`).
- All team members: Testing ownership (unit, integration, and manual critical-flow tests).

## Define SCM Processes

### Version Control
- Use `Git` with one shared remote repository.
- Protect `main` and `development` branches.

### Branching Strategy
- `main`: production-ready code only.
- `development`: integration branch for approved work.
- `feature/<scope>-<short-name>`: one feature/user story per branch.
- `hotfix/<issue>`: urgent production fixes.

### Commit Plan
- Small, regular commits per logical change.
- Commit format:
  - `feat: ...`
  - `fix: ...`
  - `test: ...`
  - `docs: ...`

### Code Review and Pull Requests
- No direct push to `main` or `development`.
- Every merge goes through a PR.
- PR minimum requirements:
  1. Linked task/user story.
  2. Passing checks (lint + tests).
  3. One approval from a different developer.

### Team Workflow
1. Pick task from Stage 3 scope.
2. Create `feature/*` from `development`.
3. Implement + test locally.
4. Open PR to `development`.
5. Review, fix comments, merge.
6. Deploy `development` to staging.
7. Release from `development` to `main` after staging sign-off.

## Plan QA Processes

### Testing Strategy
- All team members are responsible for testing activities.
- Unit tests for backend services, validators, and utility functions.
- Integration tests for API + database interactions.
- Manual tests for critical end-to-end user flows.

### Testing Tools
- `Jest` for unit/integration tests.
- `Postman` collections for endpoint validation and regression.

### Project-Specific QA Coverage
- Auth flow: register/login/OTP verification.
- Request flow: create request, add beneficiary/health/location, assign companion.
- Trip flow: start/end/extend.
- Payment flow: checkout/webhook/refund (`Moyasar`).
- Admin flow: companion approval/rejection and operational endpoints.
- Realtime flow: chat/tracking/presence data integrity in `Firebase`.

### Manual Critical Flows
1. Service seeker creates request and pays.
2. Companion receives/accepts and updates status.
3. Trip is completed and rating submitted.
4. Admin handles companion verification and issues.

## Deployment Pipeline

### Staging Pipeline (on merge to `development`)
1. Install dependencies.
2. Run lint.
3. Run `Jest` test suite.
4. Run API regression subset via `Postman`.
5. Deploy to staging.
6. Execute manual critical-flow checklist.

### Production Pipeline (on merge to `main`)
1. Re-run required CI checks.
2. Deploy to production.
3. Run smoke tests for auth, request, trip, payment.
4. Monitor logs and key errors.

## Deliverable Section

### SCM strategy (branching, code reviews)
- `main` / `development` / `feature/*` / `hotfix/*` branch model.
- PR-based merges with mandatory review and passing checks.

### QA strategy (testing tools, types of tests)
- Tools: `Jest`, `Postman`.
- Tests: unit tests, integration tests, manual critical-flow tests.
- Deployment quality gates for staging and production.
