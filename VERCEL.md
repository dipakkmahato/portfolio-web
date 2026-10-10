# Deploying on Vercel

## Backend API

Create a Vercel project from this repository and set its **Root Directory** to `backend`. Vercel will use `backend/vercel.json` and expose the Express app as a serverless function.

Set these environment variables in the Vercel project settings for Production (and Preview if needed):

- `MONGODB_URI`: connection string for a MongoDB deployment reachable from Vercel.
- `MONGODB_DB`: database name (for example, `portfolio`).
- `JWT_SECRET`: a long, random secret used to sign admin session tokens.
- `ADMIN_USERNAME` and `ADMIN_PASSWORD`: production admin credentials.
- `FRONTEND_ORIGIN`: the exact deployed frontend origin, such as `https://your-site.vercel.app`.

The backend is an API deployment; it does not build or serve the frontend.

## Frontend

The frontend source in this checkout is incomplete for a clean deployment: it has no `package.json`, lockfile, Vite/TanStack configuration, or application entry files. The existing `frontend/.output` is a generated Cloudflare build and is ignored build output, so it cannot be used to reproduce a Vercel build. Restore the frontend's package manifest and build configuration before creating a Vercel frontend project.

When restored, configure the frontend build to produce the appropriate Vercel output and set `VITE_API_BASE_URL` to the deployed backend origin (for example, `https://your-api.vercel.app`). Set the backend's `FRONTEND_ORIGIN` to the frontend deployment origin so credentialed admin requests pass CORS checks.

Deploying frontend and backend as separate Vercel projects is supported by the current Express API design. A single-project deployment needs the frontend build setup restored and an explicit routing/build configuration.
