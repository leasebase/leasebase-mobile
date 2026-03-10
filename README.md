# Leasebase Mobile (Standalone Repo)

This repository hosts the **mobile client** for the Leasebase platform (for example, a React Native or Expo application).

It is intentionally **mobile‑only**:
- No backend API code lives here.
- No web UI code lives here.

The mobile client talks to the local dev API running from a separate repo:
- Schema & Dev API: `leasebase-schema-dev`, `services/api` (NestJS + Prisma + PostgreSQL)

Until this repository is populated with full mobile application code, treat it as the home for all mobile‑specific assets (screens, navigation, native configuration) and as a client of the backend API.

---

## Working with Leasebase locally

To run the full stack locally (backend + mobile; optional web), you will work with **two (or three) repos** side by side:

- Backend: `../leasebase` (backend API + DB)
- Mobile: this repo (`leasebase-mobile`)
- (Optional) Web: `../leasebase-web`

Example workflow (once mobile code exists):

```bash path=null start=null
# 1) In ../leasebase (backend)
cd ../leasebase
npm install
docker-compose up -d db
npm run migrate
npm run seed
npm run dev:api    # API on http://localhost:4000

# 2) In ../leasebase-mobile (mobile client)
cd ../leasebase-mobile
npm install
npm start          # or expo start / react-native start, depending on stack
```

The mobile client code will live entirely in this repo and will connect to the backend API via HTTP.

---

## Local environment for this repo (future)

Once this repository is initialized with a mobile app, a typical setup will look like:

1. Install prerequisites
   - Node.js (LTS, e.g. 18 or 20)
   - npm or yarn
   - For React Native, the appropriate iOS/Android tooling (Xcode, Android Studio, simulators/emulators)
   - For Expo, the Expo CLI and app

2. Clone and install

   ```bash path=null start=null
   git clone <your-git-url>/leasebase-mobile.git
   cd leasebase-mobile
   npm install
   ```

3. Configure API endpoint
   - Point the mobile client at a Leasebase API endpoint, e.g.:
     - `http://localhost:4000` when the API is running locally
     - `https://api.dev.yourdomain.com` for the AWS dev environment

   This is typically done via an `.env` file, Expo config, or React Native config module.

4. Run the app (placeholder example)

   ```bash path=null start=null
   # Example for Expo
   npm start
   ```

Exact commands will depend on the chosen mobile stack and `package.json` scripts once the project is wired up.

---

## Deploying the backend to AWS

This repository **does not** contain backend code. All backend implementation and deployment steps live in the monorepo `../leasebase`.

To deploy or modify the backend on AWS, see:

- `../leasebase/README.md` – "Backend deployment to AWS" section
- `../leasebase/docs/architecture.md` – detailed system and AWS architecture

From the mobile client perspective, AWS deployment mainly affects **which API base URL** you configure for your app's environments (dev, staging, prod).

---

## Relevant information for contributors

- **What belongs here?**  All mobile concerns: screens, navigation, native modules/config, mobile-specific state and UX.
- **What does *not* belong here?**  Backend services, database schema/migrations, web UI code.
- **Where is the backend?**  In `../leasebase/services/api`.
- **Where is the web app?**  In the separate `../leasebase-web` repo.

Once this repository is initialized with a concrete mobile project, this README should be updated with precise dev, build, and release instructions appropriate to that stack.
