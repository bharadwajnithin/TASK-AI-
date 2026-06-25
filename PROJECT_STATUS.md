# TaskFlow AI — Project Status Document

**Last updated:** June 4, 2026  
**Workspace:** `h:\TASK AI\`

---

## 1. Project Overview

**TaskFlow AI** is an AI-powered SaaS platform that converts client communications (email, WhatsApp, chats) into structured tasks with deadlines and priorities.

| Layer | Technology |
|-------|------------|
| Frontend | React (Vite), Tailwind CSS, React Router, Axios |
| Backend | Java 21, Spring Boot 3, Maven, Spring Security, JWT |
| Database | MongoDB (Atlas or local) |
| AI | Google Gemini (`gemini-2.5-flash`) |

---

## 2. Repository Structure

```
h:\TASK AI\
├── taskflow-backend\     # Spring Boot API (port 8080)
├── taskflow-frontend\    # React app (port 5173)
└── PROJECT_STATUS.md     # This document
```

---

## 3. Completed Phases (Built ✅)

### Phase 1 — Backend Auth + MongoDB ✅

**Backend only**

| Feature | Status |
|---------|--------|
| User registration & login | ✅ |
| BCrypt password hashing | ✅ |
| JWT generation & validation | ✅ |
| MongoDB integration + auditing | ✅ |
| Google OAuth login (optional) | ✅ |
| Global exception handling | ✅ |
| CORS for React frontend | ✅ |

**API Endpoints:**
- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET /api/users/me`
- `GET /api/health`
- `GET /oauth2/authorization/google` (when OAuth enabled)

**Key files:** `AuthController`, `AuthService`, `JwtService`, `SecurityConfig`, `User` model

---

### Phase 2 — React Frontend Auth ✅

**Frontend**

| Feature | Status |
|---------|--------|
| Login & Register pages | ✅ |
| JWT in localStorage + Axios interceptors | ✅ |
| Protected & Guest routes | ✅ |
| Google OAuth callback page | ✅ |
| App layout (Sidebar, Navbar) | ✅ |
| Dashboard & Profile pages | ✅ |

**Routes:**
- `/login`, `/register`, `/oauth/callback`
- `/dashboard`, `/profile` (protected)

**Key files:** `AuthContext`, `ProtectedRoute`, `GuestRoute`, `Login.jsx`, `Register.jsx`

---

### Phase 3 — Task CRUD ✅

**Backend + Frontend**

| Feature | Status |
|---------|--------|
| Create, read, update, delete tasks | ✅ |
| Search, filter, sort, pagination | ✅ |
| Task stats for dashboard | ✅ |
| Full tasks management UI | ✅ |

**API Endpoints:**
- `POST /api/tasks`
- `GET /api/tasks` (search, status, priority, page, size)
- `GET /api/tasks/stats`
- `GET /api/tasks/{id}`
- `PUT /api/tasks/{id}`
- `DELETE /api/tasks/{id}`

**Models:** `Task`, `TaskStatus`, `Priority`, `SourceType`  
**Frontend:** `/tasks` page with modals, filters, pagination

---

### Phase 4 — AI Task Extraction (Gemini) ✅

**Backend + Frontend**

| Feature | Status |
|---------|--------|
| Gemini AI integration | ✅ |
| Extract tasks from pasted text | ✅ |
| Optional auto-save to task list | ✅ |
| Client name & project detection | ✅ |
| Priority & deadline parsing | ✅ |

**API Endpoint:**
- `POST /api/ai/extract` — body: `{ content, saveTasks }`

**Frontend:** `/ai-extract` page

**Note:** Originally OpenAI; migrated to **Google Gemini** due to quota limits.

---

### Phase 5 — Gmail Integration ✅

**Backend + Frontend**

| Feature | Status |
|---------|--------|
| Gmail OAuth connect (readonly + offline refresh) | ✅ |
| Sync inbox emails to MongoDB | ✅ |
| List imported emails | ✅ |
| AI task extraction from emails | ✅ |
| Gmail disconnect | ✅ |

**API Endpoints:**
- `GET /api/gmail/status`
- `DELETE /api/gmail/disconnect`
- `GET /api/emails`
- `POST /api/emails/sync`
- `POST /api/emails/process`

**OAuth:** `/oauth2/authorization/google-gmail`

**Models:** `EmailMessage` (collection: `emails`)  
**Frontend:** `/gmail` page

**Requires:** `GOOGLE_OAUTH_ENABLED=true` + Google Cloud credentials + Gmail API enabled

---

### Phase 6 — WhatsApp Chat Import ✅

**Backend + Frontend**

| Feature | Status |
|---------|--------|
| Paste or upload `.txt` WhatsApp export | ✅ |
| Parse bracketed & dashed WhatsApp formats | ✅ |
| Store chat imports in MongoDB | ✅ |
| AI task extraction from chats | ✅ |
| Import & save tasks in one step | ✅ |
| List, process, delete imported chats | ✅ |

**API Endpoints:**
- `POST /api/chats/import`
- `GET /api/chats`
- `POST /api/chats/process`
- `DELETE /api/chats/{id}`

**Models:** `ChatImport` (collection: `chat_imports`)  
**Frontend:** `/whatsapp` page

---

### Phase 7 — Analytics Dashboard ✅

**Backend + Frontend**

| Feature | Status |
|---------|--------|
| `GET /api/analytics` endpoint | ✅ |
| Tasks by status, priority, source metrics | ✅ |
| Overdue vs on-time tracking | ✅ |
| Completion rate calculation | ✅ |
| 7-day creation/completion trends | ✅ |
| `/analytics` page with charts | ✅ |
| Recharts integration | ✅ |
| Analytics link enabled in sidebar | ✅ |

**API Endpoint:**
- `GET /api/analytics` — returns comprehensive task analytics

**Frontend:** `/analytics` page with:
- Pie chart: Tasks by status
- Bar chart: Tasks by priority
- Bar chart: Tasks by source
- Line chart: 7-day creation/completion trends
- Stat cards: Total, Overdue, On Time, Completion Rate

---

## 4. All API Endpoints (Current)

| Method | Endpoint | Phase | Auth |
|--------|----------|-------|------|
| GET | `/api/health` | 1 | Public |
| POST | `/api/auth/register` | 1 | Public |
| POST | `/api/auth/login` | 1 | Public |
| GET | `/api/users/me` | 1 | JWT |
| POST | `/api/tasks` | 3 | JWT |
| GET | `/api/tasks` | 3 | JWT |
| GET | `/api/tasks/stats` | 3 | JWT |
| GET | `/api/tasks/{id}` | 3 | JWT |
| PUT | `/api/tasks/{id}` | 3 | JWT |
| DELETE | `/api/tasks/{id}` | 3 | JWT |
| POST | `/api/ai/extract` | 4 | JWT |
| GET | `/api/gmail/status` | 5 | JWT |
| DELETE | `/api/gmail/disconnect` | 5 | JWT |
| GET | `/api/emails` | 5 | JWT |
| POST | `/api/emails/sync` | 5 | JWT |
| POST | `/api/emails/process` | 5 | JWT |
| POST | `/api/chats/import` | 6 | JWT |
| GET | `/api/chats` | 6 | JWT |
| POST | `/api/chats/process` | 6 | JWT |
| DELETE | `/api/chats/{id}` | 6 | JWT |
| GET | `/api/analytics` | 7 | JWT |

---

## 5. Frontend Pages (Current)

| Route | Page | Status |
|-------|------|--------|
| `/login` | Login | ✅ Live |
| `/register` | Register | ✅ Live |
| `/oauth/callback` | OAuth token handler | ✅ Live |
| `/dashboard` | Dashboard + task stats | ✅ Live |
| `/tasks` | Task CRUD | ✅ Live |
| `/ai-extract` | Manual AI extraction | ✅ Live |
| `/gmail` | Gmail connect & sync | ✅ Live |
| `/whatsapp` | WhatsApp import | ✅ Live |
| `/analytics` | Analytics dashboard | ✅ Live |
| `/profile` | User profile | ✅ Live |
| `/inbox` | AI Inbox (unified) | ❌ Not built |

---

## 6. MongoDB Collections

| Collection | Model | Phase |
|------------|-------|-------|
| `users` | `User` | 1 |
| `tasks` | `Task` | 3 |
| `emails` | `EmailMessage` | 5 |
| `chat_imports` | `ChatImport` | 6 |

---

## 7. Environment Variables

Copy `taskflow-backend/.env.example` → `.env` (never commit `.env`).

| Variable | Required | Purpose |
|----------|----------|---------|
| `MONGODB_URI` | Yes | MongoDB connection |
| `JWT_SECRET` | Yes | JWT signing key |
| `GEMINI_API_KEY` | For AI | Gemini API key |
| `GEMINI_MODEL` | Optional | Default: `gemini-2.5-flash` |
| `GOOGLE_OAUTH_ENABLED` | For Gmail/OAuth | `true` / `false` |
| `GOOGLE_CLIENT_ID` | For Gmail/OAuth | Google Cloud OAuth |
| `GOOGLE_CLIENT_SECRET` | For Gmail/OAuth | Google Cloud OAuth |
| `FRONTEND_URL` | Optional | OAuth redirect (default: `http://localhost:5173`) |
| `CORS_ALLOWED_ORIGINS` | Optional | Frontend origin |

---

## 8. How to Run

### Backend (Windows — use JDK 21)

```powershell
cd taskflow-backend
.\run.ps1
```

> Do **not** use plain `mvn spring-boot:run` — it won't load `.env` and may use wrong JDK.

Server: **http://localhost:8080**

### Frontend

```powershell
cd taskflow-frontend
npm install
npm run dev
```

App: **http://localhost:5173**

### Tests

```powershell
cd taskflow-backend
$env:JAVA_HOME = "C:\Program Files\Java\jdk-21"
mvn test
```

Requires MongoDB on `localhost:27017` for integration tests.

---

## 9. Remaining Phases (Not Built ❌)

### Phase 8 (Optional) — AI Inbox (Unified)

**Planned scope:**

| Item | Description |
|------|-------------|
| Frontend | `/inbox` — single view with tabs: Emails, WhatsApp, Manual Notes |
| UX | Unified "process all" workflow for unprocessed items |

Currently Gmail and WhatsApp are separate pages; Inbox would combine them.

---

### Phase 9 (Optional) — Slack Integration

**From original spec, not started:**

- Slack workspace connect
- Import messages/channels
- AI task extraction from Slack threads

`SourceType.SLACK` enum exists but no implementation yet.

---

### Phase 10 (Optional) — Production Hardening

| Item | Status |
|------|--------|
| Deploy backend to Render/Railway | Not done |
| Deploy frontend to Vercel/Netlify | Not done |
| Remove hardcoded defaults from `application.yml` | Partial |
| Email notifications for due tasks | Not planned yet |
| Webhook / scheduled Gmail sync | Not planned yet |

---

## 10. Phase Summary Table

| Phase | Feature | Backend | Frontend | Status |
|-------|---------|---------|----------|--------|
| 1 | Auth + MongoDB | ✅ | — | **Done** |
| 2 | React Auth UI | — | ✅ | **Done** |
| 3 | Task CRUD | ✅ | ✅ | **Done** |
| 4 | Gemini AI Extract | ✅ | ✅ | **Done** |
| 5 | Gmail Integration | ✅ | ✅ | **Done** |
| 6 | WhatsApp Import | ✅ | ✅ | **Done** |
| 7 | Analytics Dashboard | ✅ | ✅ | **Done** |
| 8 | AI Inbox (unified) | ❌ | ❌ | Optional |
| 9 | Slack Integration | ❌ | ❌ | Optional |
| 10 | Production deploy | ❌ | ❌ | Optional |

---

## 11. Known Issues & Tips

1. **Always use `.\run.ps1`** for backend — loads `.env` and sets Java 21.
2. **JDK 21 required** — Maven may default to JDK 17 and fail compile.
3. **Gmail needs Google OAuth** — set `GOOGLE_OAUTH_ENABLED=true` and create OAuth credentials with redirect:
   - Login: `http://localhost:8080/login/oauth2/code/google`
   - Gmail: `http://localhost:8080/login/oauth2/code/google-gmail`
4. **Register with same email** as Gmail account before connecting Gmail.
5. **Gemini API key** required for AI features (`GEMINI_API_KEY` in `.env`).
6. **Secrets** should only live in `.env`, not in `application.yml` or git.

---

## 12. Quick Reference — What's Working End-to-End

```
Register/Login → Dashboard
              → Create & manage Tasks manually
              → Paste text → AI Extract → Save tasks
              → Connect Gmail → Sync → Generate tasks from emails
              → Upload WhatsApp .txt → Import → Generate tasks from chat
              → View Analytics dashboard with charts
```

**Not yet working:**
```
Unified AI Inbox
Slack import
Production deployment
```

---

*Document maintained as part of TaskFlow AI build. Update this file when a new phase is completed.*
