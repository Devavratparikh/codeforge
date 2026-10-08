# PROJECT_CONTEXT.md — CodeForge

> Living state file. Read CLAUDE.md first, then this. Update at the end of every session (concise; no code dumps). If this conflicts with the real code, trust the code and fix this file.

## 1. Summary
CodeForge: an original LeetCode-inspired full-stack coding platform and a serious portfolio + learning project. Target features: register/login, problem browsing/search/filter, browser code editor, submissions, automated Docker-based judging against test cases, verdicts, submission history, solved tracking, profiles, statistics, leaderboards, contests, Redis queues + workers, CI/CD, AWS deployment, monitoring.

Developer level: basic HTML/CSS/JS; beginner in React, Node, Express, PostgreSQL, Docker, Redis, DevOps, AWS. Don't assume more.

## 2. Process
PLAN → IMPLEMENT → TEST → DEBUG → COMMIT → DOCUMENT. Build incrementally, one small task per DAY. The developer must understand what's built.

## 3. Stack (locked)
Frontend: React, Vite, JavaScript, Tailwind CSS, React Router · Backend: Node.js, Express, JavaScript · DB: PostgreSQL + Prisma · Auth: password hashing + HTTP-only cookies + server-side authN/authZ · Editor: Monaco · Judge: Docker (C++ only first) · Queue: Redis + BullMQ · DevOps: Git, GitHub, Docker, Docker Compose, GitHub Actions · Cloud: AWS (later) · Monitoring: Prometheus + Grafana (late).

## 4. Architecture
Now: Frontend → Backend → PostgreSQL. Later: + Redis. Eventually: Frontend → Backend → Redis queue → Judge worker → Docker sandbox → test cases → verdict → PostgreSQL. The judge is separated because submitted code is untrusted and needs isolation and resource limits.
Production target: Internet → HTTPS → Load balancer → Frontend/Backend → PostgreSQL, Redis, storage; Redis queue → multiple workers → Docker sandboxes → verdict → PostgreSQL. (Destination, not the start.)

## 5. Data model
Initial entities: User, Problem, TestCase, Submission. Later (only when needed): Tag, UserProblem, Contest, ContestProblem, ContestParticipant, Leaderboard.
Roles: USER, ADMIN (MODERATOR maybe later). Difficulty: EASY, MEDIUM, HARD.
Verdicts: QUEUED, RUNNING, ACCEPTED, WRONG_ANSWER, COMPILE_ERROR, RUNTIME_ERROR, TIME_LIMIT_EXCEEDED, MEMORY_LIMIT_EXCEEDED, SYSTEM_ERROR.
Language: C++ only until the judge is reliable (later maybe Python, Java, JavaScript).

## 6. Pages (planned)
`/` Home · `/problems` list · `/problems/:id` details + editor · `/login` · `/register` · `/profile/:username` · `/dashboard` · `/admin` · `/contests` · `/contests/:id`

## 7. API (planning reference, not all at once)
Auth: `POST /api/auth/register|login|logout`
Problems: `GET /api/problems`, `GET /api/problems/:id`; admin `POST /api/problems`, `PATCH /api/problems/:id`, `DELETE /api/problems/:id`
Submissions: `POST /api/submissions`, `GET /api/submissions`, `GET /api/submissions/:id`
Users: `GET /api/users/:username`
Error shape: `{ "success": false, "message": "..." }`

## 8. Roadmap (milestones)
1 Static frontend → 2 React frontend → 3 Backend API → 4 Database → 5 Authentication → 6 Problem management → 7 Monaco editor → 8 Basic judge → 9 Redis + workers → 10 Statistics → 11 Contests → 12 Dockerization → 13 CI → 14 CD → 15 Cloud deployment → 16 Monitoring → 17 Security hardening → 18 Scalability.

## 9. Success criteria
Users register/login, browse/open problems, write and submit code; the system safely executes it, evaluates test cases, returns and stores verdicts; progress is tracked; admins manage problems; app is Dockerized, CI/CD works, deployed, with logs/monitoring; and the developer can explain the whole architecture.

---
# LIVE STATE (update every session)

**Current milestone:** Milestone 1 — Static frontend (Phase 0 JS basics as needed)
**Current day:** DAY 0 (not started)
**Current blocker:** None

**Completed work:**
- (nothing yet)

**Current file structure:**
```
codeforge/
├── CLAUDE.md
└── PROJECT_CONTEXT.md
```

**Concepts learned** (only mark if truly understood, not just generated): none yet
**Commands learned:** none yet
**Important decisions:** (format: Decision / Reason / Date) — stack above is the initial decision set; record any change here.
Decision: After each major milestone, give Dev a small understanding-test challenge (no solution unless asked). / Reason: Dev wants to verify real understanding, not just working code. / Date: 2026-10-08
**Known issues:** none
**Bugs fixed:** none

**NEXT TASK (one only):**
DAY 1 — Create the project folder, `git init`, and build a static `index.html` landing page with a navbar (HTML/CSS only, mock content). Commit it.

---
# SESSION LOG (keep each day short)

## DAY 1 — YYYY-MM-DD
Completed: · Learned: · Problems: · Commit: · Next:
