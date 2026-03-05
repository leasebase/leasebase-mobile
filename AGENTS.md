# AGENTS.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Repository purpose & related projects

- This repository is **mobile-only** for the Leasebase platform.
- No backend API code lives here; it resides in the sibling repo `../leasebase` (NestJS + Prisma + PostgreSQL under `services/api`).
- No web UI code lives here; that lives in the separate repo `../leasebase-web`.
- The mobile app is a **client of the backend API** and should communicate with it over HTTP using a configurable base URL (e.g. `http://localhost:4000` in local dev, or environment-specific HTTPS endpoints).

## Current state of this repo

- As of this version, the repo is a **skeleton for the mobile client**; there is no initialized mobile app code and no `package.json`.
- All concrete mobile setup (stack, navigation, native configuration, build pipeline, tests, etc.) is **TBD**.
- When mobile code and tooling are added (e.g. React Native or Expo app), always prefer the **actual scripts and instructions** documented in this repo (e.g. `README.md`, `package.json`, or `/docs`) over any assumptions.

## Local development – multi-repo setup

The intended workflow is to run the backend API from `../leasebase` and the mobile app from this repo side by side.

### 1. Start the backend API (in `../leasebase`)

In the sibling backend repo, a typical local API workflow (per the docs referenced in this README) looks like:

```bash
cd ../leasebase
npm install

# Start PostgreSQL via Docker (command name may vary by that repo's README)
docker-compose up -d db

# Database migrations and seed data
npm run migrate
npm run seed

# Run the API server (commonly on http://localhost:4000)
npm run dev:api
```

When working as an agent in this mobile repo and you need **backend behavior** (API shape, DB schema, etc.), open files in `../leasebase/services/api` and its documentation (e.g. `../leasebase/README.md`, `../leasebase/docs/architecture.md`).

### 2. Mobile client (this repo, once initialized)

Once a concrete mobile stack is wired up here (e.g. React Native or Expo), the high-level flow will be:

1. **Install prerequisites** (from the README):
   - Node.js LTS (e.g. 18 or 20).
   - `npm` or `yarn`.
   - For React Native: Xcode + iOS simulators and/or Android Studio + emulators.
   - For Expo: Expo CLI and the Expo Go app or native binaries.

2. **Clone and install dependencies**:

   ```bash
   git clone <git-url>/leasebase-mobile.git
   cd leasebase-mobile
   npm install
   ```

3. **Configure the API endpoint**:
   - Point the mobile client at the Leasebase API base URL, usually via an `.env` file or config module.
   - Common values (based on the README):
     - `http://localhost:4000` when the API is running locally in `../leasebase`.
     - Environment-specific URLs (e.g. dev/staging/prod) when connecting to AWS-hosted backends.

4. **Run the app (placeholder until scripts exist)**:

   ```bash
   # Example for an Expo-style setup – treat as illustrative only
   npm start
   ```

When `package.json` and scripts are added, **always infer the exact dev, build, lint, and test commands from that file** rather than reusing the placeholder above.

## Commands for agents (once tooling exists)

Because this repo currently has no tooling files, do **not assume** specific script names. Instead, when asked for commands:

- Read `package.json` (once present) to determine:
  - Development server command (e.g. `npm run start`, `npm run ios`, `npm run android`).
  - Test runner command (e.g. `npm test`, `npm run test:unit`, and options for running a single test file or pattern).
  - Lint/format commands (e.g. `npm run lint`, `npm run lint:fix`, `npm run format`).
  - Build commands for release binaries or Expo builds.
- Cross-check with `README.md` (and `/docs` if present) for any project-specific flags, environment variables, or required setup steps.

If the necessary scripts are **not** defined yet, clearly state that to the user instead of inventing commands.

## Architecture and separation of concerns

- **Mobile app (this repo)**
  - Owns all mobile-specific concerns: screens, navigation, mobile UX, local storage, and any native modules.
  - Should not contain backend business logic, database access, or infrastructure concerns.
  - Communicates exclusively with the backend via HTTP/HTTPS using a defined API contract.

- **Backend API (`../leasebase`)**
  - Implements core business logic, persistence (PostgreSQL via Prisma), and integrations.
  - Exposes HTTP endpoints consumed by this mobile client.
  - AWS deployment and system architecture are documented in `../leasebase/README.md` and `../leasebase/docs/architecture.md`.

- **Web app (`../leasebase-web`)**
  - Separately implemented web front-end that also consumes the backend API.
  - Not directly modified from this repo, but useful as a reference for API usage patterns, auth flows, and shared UX concepts.

When agents need a "big picture" view, consider all three repos together: backend (`../leasebase`), mobile (this repo), and web (`../leasebase-web`).

## Guidance for future agents

- Before proposing commands, **inspect** the actual tooling files in this repo (e.g. `package.json`, `app.json`, `eas.json`, `babel.config.*`, `tsconfig.*`) once they exist.
- Prefer patterns and conventions already established in the backend and web repos when introducing:
  - Environment variable names for API URLs and auth.
  - Error-handling patterns for API calls.
  - Directory layout for features/domains, if a shared structure emerges.
- When users ask architecture questions that clearly span mobile + backend, open the relevant files in `../leasebase` instead of trying to infer behavior solely from the mobile client.
