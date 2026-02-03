# Leasebase Mobile (Standalone Repo)

This repository is intended to host the **standalone mobile client** for the Leasebase platform (for example, a React Native or Expo application).

Currently, the primary implementation work and backend logic are defined in the **monorepo sibling** `../leasebase`, which contains:
- The backend API (`services/api` – NestJS + Prisma + PostgreSQL)
- The monorepo-level mobile workspace (`apps/mobile`, reserved for future use)
- Infrastructure and architecture documentation

Until this repository is populated with real mobile application code, treat it mainly as documentation and a pointer to the monorepo.

---

## Working with Leasebase locally

To run the full stack locally (backend + web; future mobile), follow the instructions in:

- `../leasebase/README.md`

In brief, from `../leasebase` you will:

```bash path=null start=null
cd ../leasebase
npm install
npm run dev:api     # start the API on localhost:4000
# (once web/mobile exist)
npm run dev:web
npm run dev:mobile
```

The mobile client code will eventually live under `../leasebase/apps/mobile` (or inside this repo once it is bootstrapped as a full project).

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

- **Where is the backend?**  In `../leasebase/services/api`.
- **Where is the current mobile code?**  The monorepo reserves `../leasebase/apps/mobile`, but this standalone repo has not yet been bootstrapped.
- **Where do I add new features now?**  Until this repo has real mobile code, prefer working in the monorepo and its `apps/mobile` workspace.

Once this repository is initialized with a concrete mobile project, this README should be updated with precise dev, build, and release instructions appropriate to that stack.
