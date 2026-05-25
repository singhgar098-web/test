# WorkLog AI — AI-Powered Automatic Worklog Platform

> Your AI Work Memory Assistant. Automatically tracks work activity, generates professional worklogs, and lets you review/edit/submit them.

---

## Project Structure

```
Worklog/
├── backend/          # Node.js + Express + MongoDB API
├── frontend/         # React.js Dashboard
├── electron-app/     # Electron Desktop Tracker
├── docker-compose.yml
└── ecosystem.config.js
```

---

## Quick Start

### Prerequisites
- Node.js 18+
- MongoDB (local or Atlas)
- Redis (local or cloud)
- OpenAI API Key

---

### 1. Backend Setup

```bash
cd backend
npm install
cp .env .env.local   # Edit with your real values
npm run dev
```

Backend runs on: http://localhost:5000

---

### 2. Frontend Setup

```bash
cd frontend
npm install
npm start
```

Dashboard runs on: http://localhost:3000

---

### 3. Electron App Setup

```bash
cd electron-app
npm install
npm start
```

---

### 4. Docker (Full Stack)

```bash
# Copy and fill in your env values first
cp backend/.env backend/.env

docker-compose up --build
```

- Frontend: http://localhost:3000
- Backend API: http://localhost:5000
- MongoDB: localhost:27017
- Redis: localhost:6379

---

## API Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /api/v1/auth/register | Register |
| POST | /api/v1/auth/login | Login |
| POST | /api/v1/auth/refresh | Refresh token |
| GET | /api/v1/users/me | Get profile |
| POST | /api/v1/activities/batch | Save activities (Electron) |
| GET | /api/v1/timelines?date= | Get timeline |
| POST | /api/v1/worklogs/generate | Generate worklog |
| GET | /api/v1/worklogs | List worklogs |
| PATCH | /api/v1/worklogs/:id | Update draft |
| POST | /api/v1/worklogs/:id/submit | Submit worklog |
| POST | /api/v1/worklogs/:id/regenerate | Regenerate with AI |
| GET | /api/v1/analytics/weekly | Weekly analytics |
| GET | /api/v1/notifications | Notifications |
| PUT | /api/v1/schedules | Save auto-schedule |

---

## Worklog Lifecycle

```
Electron Tracks Activity
        ↓
Backend Processes → Timeline Built
        ↓
AI Generates Draft (BullMQ Worker)
        ↓
Status = DRAFT → User Notified
        ↓
User Reviews / Edits / Adds Entries
        ↓
User Clicks SUBMIT
        ↓
Status = SUBMITTED (Locked)
```

---

## Key Features

- ✅ Privacy-first — no keylogging, no passwords tracked
- ✅ Offline-first Electron with local SQLite cache
- ✅ AI worklog generation (GPT-4o-mini)
- ✅ 4 modes: Developer, Manager, Concise, Detailed
- ✅ Editable drafts — never auto-submit
- ✅ Version history on every edit
- ✅ Real-time updates via Socket.IO
- ✅ BullMQ job queues with retry logic
- ✅ Cron-based auto-generation scheduler
- ✅ Google OAuth
- ✅ JWT + refresh token rotation
- ✅ Docker + PM2 production ready

---

## Environment Variables (Backend)

See `backend/.env` for all required variables including:
- `MONGODB_URI`
- `REDIS_URL`
- `JWT_SECRET`
- `OPENAI_API_KEY`
- `GOOGLE_CLIENT_ID` / `GOOGLE_CLIENT_SECRET`

---

## Production Deployment

```bash
# Using PM2
cd backend
npm install
NODE_ENV=production pm2 start ecosystem.config.js --env production

# Using Docker
docker-compose -f docker-compose.yml up -d
```
