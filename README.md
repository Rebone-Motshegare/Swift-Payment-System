# RR Customer Portal (React + Vite frontend, Express + TypeScript backend)

[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/mynj0YAW)

<img width="1333" height="605" alt="image" src="https://github.com/user-attachments/assets/80d6706c-97ac-4bf2-b608-c32e954cbad1" />
<img width="1338" height="534" alt="image" src="https://github.com/user-attachments/assets/e9af515d-6011-4594-9008-11bb7a94ba5c" />
## Overview

This repository contains a full-stack sample application:

- frontend: a React application bootstrapped with Vite (TypeScript).
- backend: an Express API written in TypeScript.

The project demonstrates a payments/customer portal with authentication, payments endpoints, and utilities. It includes helpful scripts for development, SSL test/demo utilities, linting, and tests.

---

## Table of contents

- Requirements
- Quick start (local development)
  - Backend
  - Frontend
- Build & Production
- Environment variables
- Useful scripts
- Project structure
- Notes & troubleshooting

---

## Requirements

- Node.js 18+ (recommended)
- npm or yarn
- MongoDB (for backend database) or use a hosted MongoDB URI
- (Optional) OpenSSL or the provided ssl scripts for local HTTPS testing

---

## Quick start (local development)

The repository has two main folders: `backend/` and `frontend/`. Run them in separate terminals.

### Backend (Express + TypeScript)

1. Install dependencies

```powershell
cd backend
npm install
```

2. Create a `.env` file in `backend/` (you can copy `.env.example` if present) and set at minimum:

- `PORT` (defaults to 3001)
- `MONGODB_URI` (defaults to mongodb://localhost:27017/rr-customer-portal)
- `JWT_SECRET` and `JWT_REFRESH_SECRET` (change for production)
- `CORS_ORIGIN` (defaults to http://localhost:3000)

Example `.env` (development):

```text
NODE_ENV=development
PORT=3001
MONGODB_URI=mongodb://localhost:27017/rr-customer-portal
JWT_SECRET=dev-jwt-secret
JWT_REFRESH_SECRET=dev-refresh-secret
CORS_ORIGIN=http://localhost:5173
ENABLE_SSL=false
```

3. Run in development (uses `nodemon` + `ts-node`):

```powershell
npm run dev
```

4. Build and start (production-like):

```powershell
npm run build
npm start
```

Notes:
- The backend listens on `PORT` (default 3001).
- SSL: set `ENABLE_SSL=true` and provide `SSL_CERT_PATH` and `SSL_KEY_PATH` or use the included `backend/ssl` files for local testing. There are scripts under `backend/scripts` to generate/test self-signed certs.

### Frontend (Vite + React)

1. Install dependencies

```powershell
cd frontend
npm install
```

2. Run dev server

```powershell
npm run dev
```

By default Vite serves on port 5173. If you need the frontend to call the backend during development, ensure the backend `CORS_ORIGIN` includes the Vite origin (for example, `http://localhost:5173`).

3. Build for production

```powershell
npm run build
npm run preview    # optional: preview the built app
```

---

## Employee portal

This repository also includes a separate Employee portal (administration) implementation. It's organized under the `Employee portal/` folder and mirrors the customer portal layout with its own frontend and backend.

Folder layout:

- `Employee portal/backend-employee/` — Express + TypeScript backend for employee features (default PORT 3002)
- `Employee portal/frontend-employee/` — Vite + React frontend for employee UI (Vite default port 5173)

Quick start (employee portal):

Backend (Employee):

```powershell
cd "Employee portal/backend-employee"
npm install
npm run dev    # development using nodemon + ts-node

# or build + start
npm run build
npm start
```

Defaults and notes:
- Employee backend default port: `3002` (see `Employee portal/backend-employee/src/config/index.ts`)
- Provide `.env` values as needed (same keys as customer backend, but you may want different JWT secrets and MongoDB DB name)

Frontend (Employee):

```powershell
cd "Employee portal/frontend-employee"
npm install
npm run dev

# Build for production
npm run build
npm run preview
```

If you run both customer and employee frontends locally, they both default to Vite's 5173 port — run one at a time or change the port for one (for example: `PORT=5174 npm run dev` in the frontend project) or set `VITE_PORT` in the frontend's env.

---

## Build & Production

- Backend: `npm run build` (compiles TypeScript to `dist/`), then `npm start` to run the compiled Node server.
- Frontend: `npm run build` to generate a production bundle in `dist/` (Vite output). Serve with any static host.

If you plan to deploy both together behind a reverse proxy, map the frontend static assets to the appropriate host and point API calls to the backend base URL (e.g., `https://api.example.com/api/v1`).

---

## Environment variables (backend important ones)

The backend reads configuration from environment variables. Defaults are documented in `backend/src/config/index.ts`.

Key variables:

- NODE_ENV (development|production)
- PORT (default 3001)
- MONGODB_URI
- JWT_SECRET
- JWT_REFRESH_SECRET
- CORS_ORIGIN (default http://localhost:3000)
- ENABLE_SSL (set to `true` to enable HTTPS)
- SSL_CERT_PATH / SSL_KEY_PATH

Note: In production, ensure JWT secrets and database URIs are set and secret values are not the placeholder defaults.

---

## Useful scripts (extracted from package.json)

Backend (`backend/package.json`):

- `npm run dev` — start server with `nodemon` and `ts-node` for development
- `npm run build` — compile TypeScript
- `npm start` — run compiled server (`node dist/server.js`)
- `npm test` / `npm run test:watch` — run Jest tests
- `npm run lint` / `npm run lint:fix` — ESLint
- `npm run ssl:generate` / `npm run ssl:test` / `npm run ssl:demo` — tools for generating/testing self-signed SSL certs

Frontend (`frontend/package.json`):

- `npm run dev` — run Vite development server
- `npm run build` — build production bundle
- `npm run preview` — preview production build
- `npm run lint` — run ESLint

---

## Project structure (high level)

- backend/
  - src/ — TypeScript source
    - config/ — configuration and environment mapping
    - controllers/ — route handlers
    - middleware/ — security, validation, logging
    - models/ — Mongoose models
    - routes/ — Express routes
  - scripts/ — helper scripts (SSL, demos)

- frontend/
  - src/ — React app (components, pages, context, services)
  - public/ — static assets

---

## Notes & troubleshooting

- If the backend fails to start because of missing environment variables in production, the config loader will throw; populate required vars first.
- If you see CORS errors in the browser, check `CORS_ORIGIN` in backend env and ensure Vite origin is included.
- For HTTPS local testing, use the provided scripts in `backend/scripts` to generate self-signed certs; browsers will still show warnings for self-signed certs unless you trust them locally.

---

## Next steps and improvements (suggested)

- Add a `frontend/.env.example` showing any runtime API base URL variables (e.g., VITE_API_BASE_URL).
- Add Dockerfiles / docker-compose for easy local setup (Mongo + backend + frontend).
- Add API documentation (Swagger/OpenAPI endpoint) under `/api/docs`.

---

If you'd like, I can:

- add a `backend/.env.example` and `frontend/.env.example` to the repo,
- add a short section that explains how to run both services with `concurrently` or `docker-compose`, or
- include commands for Windows PowerShell to create self-signed certs using the repo scripts.

Tell me which additions you want and I'll implement them.
[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/mynj0YAW)
<img width="1333" height="605" alt="image" src="https://github.com/user-attachments/assets/80d6706c-97ac-4bf2-b608-c32e954cbad1" />
<img width="1323" height="433" alt="image" src="https://github.com/user-attachments/assets/e7a70227-d73b-4d83-a70d-344e34db1109" />
