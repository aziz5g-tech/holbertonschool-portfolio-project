# SCM and QA Strategies

## Purpose
To establish procedures for managing code, the development lifecycle, and ensuring quality.

## Define SCM Processes
- Use **Git** for version control.
- Establish branching strategy:
  - `main`
  - `development`
  - `feature/*`
- Plan for regular commits, code reviews, and pull requests.

**SCM Example:** Feature branches for each task, with code reviewed before merging into the development branch.

## Plan QA Processes
- Define testing strategy:
  - Unit tests
  - Integration tests
- Specify testing tools:
  - **Jest**
  - **Postman**
- Plan deployment pipeline:
  - **Staging** environment
  - **Production** environment

**QA Example:** Automated unit tests for API endpoints and manual testing for critical user flows.

## Deliverable Section

### SCM strategy (branching, code reviews)
- Branching with `main`, `development`, and `feature/*`.
- Code reviews required through pull requests.

### QA strategy (testing tools, types of tests)
- Tools: **Jest**, **Postman**.
- Tests: Unit tests, integration tests, and manual testing for critical flows.
