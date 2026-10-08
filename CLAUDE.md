# CLAUDE.md — CodeForge Instructions

## 1. Project
CodeForge is an original, LeetCode-inspired full-stack coding platform (not a copy). Users register/login, browse and filter problems, write code in the browser, submit, get judged against test cases, see verdicts, history, profiles, stats; later contests, leaderboards, multi-language, and a secure scalable judge.

It is ALSO a learning project. Every step must deliver BOTH a working production-quality feature AND the understanding behind it. The developer must be able to explain the architecture in an interview, not just say "AI wrote it".

## 2. Developer level
Knows basic HTML, CSS, JS. Beginner in React, Node, Express, PostgreSQL, Prisma, auth, Docker, Redis, queues, Git/CI/CD, cloud, system design.
- Never assume advanced knowledge. Briefly explain any new concept before using it.
- Teach only what the current task needs. No theory dumps, no "watch 10 hours of tutorials".
- Don't dump big code blocks. Learn by building.

## 3. Session protocol
Work is organized as DAY 1, DAY 2, ...
**Start of session:** read this file, then PROJECT_CONTEXT.md; inspect the real code if needed; find the current milestone; pick the smallest logical task; don't redo finished work or add tech not yet needed. If context and code disagree, trust the code and fix the context file.
**End of session:** update PROJECT_CONTEXT.md (completed work, files changed, concepts learned, commands learned, bugs found/fixed, decisions, known issues, current milestone/day, ONE next task, session-log entry). Keep it concise; never paste whole code files into it.

Philosophy: BUILD → UNDERSTAND → TEST → COMMIT → MOVE FORWARD. Never jump layers ahead of a validated one. One day's task = something the developer can understand, implement, test and debug in one sitting (e.g. "registration API", not "all of auth").

## 4. Response format for each task
**Today** (one sentence goal) → **Why** (one short reason) → **Learn** (only needed concepts) → **Implement** (per step: file path, what to change, code, why) → **Test** (exact commands + expected output; ask the developer to confirm the result before moving on) → **Commit** (suggested message) → **Progress** (short summary) → **Next** (one step only, no long future lists).
Teaching pattern: concept → tiny example → CodeForge implementation → test → explanation. Example: "frontend asks backend for problems" → build `GET /api/problems` → explain frontend → HTTP → route → DB → JSON → frontend.

## 5. Hard rules
- **No silent changes.** Never swap a decision (React→Next, Postgres→Mongo, Express→Nest, Prisma→other ORM, JWT↔sessions, AWS→other). If change is needed: state the old decision, why, impact, ask confirmation if significant, update PROJECT_CONTEXT.md.
- **No blind code.** Verify file paths, imports, syntax, dependency versions, env vars, schema, routes. Never invent files/functions that may not exist; inspect first.
- **Dependencies.** Explain why before installing; prefer existing tools; no duplicates (one HTTP client only).
- **Token efficiency.** Don't re-explain the project or past concepts. Use focused diffs ("replace this function", "add this below X") instead of reprinting files; print whole files only if new/small. No unnecessary alternatives or docs. Recommend ONE approach (balance: beginner clarity, production relevance, simplicity, scalability, resume/interview value); mention another only if significantly better.
- **No overengineering.** No microservices, Kubernetes, Kafka, GraphQL, CQRS, event sourcing, service mesh. Start as a modular monolith; extract only the judge worker later (real isolation/scaling need).
- **Don't create folders/tables/infra until needed.**
- **Debugging.** Read the exact error → locate origin → reproduce → hypothesis → inspect → smallest fix → test → explain root cause vs symptom vs fix vs prevention. Don't rewrite everything. When stuck, ask only for what's needed: exact error, file, command, expected vs actual (never the whole project).
- **Testing.** Manual first, then API tests, unit, integration, frontend, E2E — introduce progressively, one framework at a time.
- **Git.** Commit every meaningful milestone, Conventional style: feat / fix / refactor / docs / test / chore.
- **Priorities:** working app > developer understanding > correct architecture > security > testing > performance > scalability > advanced DevOps. Don't optimize before it works.

## 6. Stack (fixed unless explicitly changed)
- **Frontend:** React, Vite, JavaScript (no TypeScript initially), Tailwind CSS, React Router, fetch or Axios (pick one), Monaco Editor.
- **Backend:** Node.js, Express, JavaScript, REST.
- **DB:** PostgreSQL + Prisma, with migrations.
- **Auth:** password hashing, HTTP-only cookies, server-side authorization, roles USER / ADMIN (MODERATOR maybe later). Keep it simple.
- **Judge:** Docker. Initial language: C++ only; add others only after the judge is reliable.
- **Queue:** Redis + BullMQ.
- **DevOps:** Git, GitHub, Docker, Docker Compose, GitHub Actions, AWS later. **Monitoring** (logs, health checks, Prometheus, Grafana) only late.

## 7. Phases (follow in order)
0. **JS foundation** — DOM, events, arrays/objects, functions, modules, destructuring, spread/rest, promises, async/await, fetch, error handling (only as needed).
1. **Static frontend** — HTML/CSS/JS with mock data: landing, navbar, problems, problem page, login, register, profile. No backend, no React yet.
2. **React** — components, props, state, hooks (useState/useEffect), Router, forms, API calls, conditional rendering, reusable components.
3. **Backend** — HTTP, REST, routes, controllers, middleware, status codes, validation, error handling.
4. **PostgreSQL/Prisma** — entities User, Problem, TestCase, Submission (add Tag, UserProblem, Contest, etc. only when needed).
5. **Authentication** — register, login, logout, hashing, auth middleware, protected routes, roles.
6. **Problem management** — browse/search/filter/detail/solved tracking; admin create/edit/delete problems and test cases.
7. **Monaco editor** — write, reset, run, submit (C++ first).
8. **Basic judge** — Frontend → POST submission → Backend → Submission record → Judge → Docker → compile → run → tests → compare → verdict → DB → Frontend. Verdicts: QUEUED, RUNNING, ACCEPTED, WRONG_ANSWER, COMPILE_ERROR, RUNTIME_ERROR, TIME_LIMIT_EXCEEDED, MEMORY_LIMIT_EXCEEDED, SYSTEM_ERROR.
9. **Redis + BullMQ workers** — teach queues, workers, jobs, retries, concurrency, async processing (prevents blocking, handles load, scales).
10. **Statistics** — dashboard (solved/attempted, recent submissions, streak, difficulty split), profile, per-problem stats (total/accepted/acceptance rate).
11. **Contests** (only after the platform is stable) — create, start/end, problems, registration, in-contest submissions, ranking, scoring.
12. **Leaderboard** — global; simple first (solved count, contest points, streak/XP later).
13. **Docker** — containerize frontend, backend, DB, Redis, judge worker via docker-compose (introduce only once backend/DB exist).
14. **CI** (GitHub Actions: install → lint → test → build), then **CD** (image build/push/deploy).
15. **AWS deployment** — HTTPS → load balancer → app → DB/Redis → judge workers; managed services where sensible; never expose Postgres/Redis publicly.
16. **Monitoring, security hardening, scalability.**

## 8. Judge security (critical)
Submitted code is UNTRUSTED and must NEVER run on the backend host. Expect infinite loops, memory hogs, fork bombs, file/secret/internal-network access, host attacks. Minimum protections: Docker isolation, CPU/memory/time/process limits, restricted filesystem, no network, non-root user, throwaway containers with cleanup, max source size, max input/output size. Keep the judge separate from the backend.

## 9. General security
Password hashing, HTTP-only cookies, CORS, CSRF awareness, input validation, SQL-injection and XSS protection, rate limiting, authN + authZ, secure headers, env vars/secrets management, upload restrictions if ever added. Never commit secrets (DB passwords, JWT secrets, API keys, AWS credentials); use `.env` + `.gitignore`.

## 10. Database rules
Relational and normalized: primary keys, foreign keys, unique constraints, timestamps. Add an index only after explaining why. Always use migrations; never casually change production schema.

## 11. API rules
REST conventions: `GET /api/problems`, `GET /api/problems/:id`, `POST`, `PATCH /:id`, `DELETE /:id`. Status codes: 200, 201, 400, 401, 403, 404, 409, 429, 500. Consistent errors: `{ "success": false, "message": "Problem not found" }`.

## 12. Code quality & structure
Readable, small functions, descriptive names, separation of concerns, meaningful comments; avoid huge functions, duplication, magic numbers, deep nesting, needless abstraction, premature optimization.
```
frontend/src/{components,pages,layouts,hooks,services,utils,context,assets}, App.jsx, main.jsx
backend/src/{controllers,routes,middleware,services,validators,utils,config,prisma}, server.js
judge/{workers,executors,languages,sandbox,utils,Dockerfiles}
```
Create each folder only when it's actually needed. Move logic out of routes into services as complexity grows.

## 13. Architecture evolution
Stage 1: React → Express → PostgreSQL. Stage 2: + Redis. Stage 3: Express → Redis queue → Judge worker → Docker. Stage 4: Users → Load balancer → multiple backends → Redis → judge workers → PostgreSQL. Evolve naturally; the destination is NOT the starting point.

## 14. Final goal
By the end the developer can explain: React (components, state, API calls), backend (HTTP, REST, Express, middleware, auth), database (SQL, relations, indexing, transactions), judge (compilation, sandboxing, Docker, queues, workers, resource limits), DevOps (Git, Docker, CI/CD, cloud, env management, monitoring), and system design (async processing, horizontal scaling, caching, queues, DB and worker scaling).

## 15. Golden rule
Never optimize for the most code. Optimize for: CORRECT CODE + UNDERSTANDING + SMALL STEPS + TESTABILITY + GOOD ARCHITECTURE. If unsure what's next: read PROJECT_CONTEXT.md and pick the smallest logical task.
