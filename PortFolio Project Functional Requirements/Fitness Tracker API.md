# Fitness Tracker API – Requirements

---

## 1. Product Vision

A secure, extensible API-centric platform for individuals and fitness professionals to track workouts, log health data, set and analyze progress against goals, connect with devices and external fitness/nutrition APIs, and visualize trends. Designed for mobile, web, or integration into third-party health apps; supports single and multi-user use, with robust data models (sessions, sets, metrics), insights, and notification/automation.

---

## 2. Target Audience

- **Individuals/Fitness Enthusiasts:** Anyone tracking personal workouts, health metrics, goals, nutrition, or habits.
- **Personal Trainers/Coaches:** Manage multiple clients, assign workouts, monitor compliance/progress, analyze trends.
- **Gyms/Studios:** Provide members access to log sessions, group challenges, and social/leaderboards.
- **App Developers:** Leverage a robust, well-documented API for custom or branded health apps, integrations, or wearables.
- **Healthcare & Research:** Optional modules for patient data, cohort analytics, or longitudinal studies.

---

## 3. Key Value Propositions

- **Full-featured workout logging:** Track routines, session details, sets/reps, weights, time, cardio, and more.
- **Goal-centric personalization:** Set, monitor, and revise goals for weight, reps, distance, calories, streaks, milestones.
- **Health metrics:** Log and visualize body measurements (weight, fat %, HRV, BP, water, sleep).
- **Data import/integration:** Connect with device APIs (e.g., Fitbit, Garmin), public activity APIs (Strava, Apple Health), and nutrition/food (Spoonacular, USDA).
- **Advanced analytics:** Charts, PR tracking, streaks, trends, training load, injury management.
- **Social and Motivation:** (Optional/Planned) Challenges, friend/follow leaderboards, shared progress, comments, reactions.
- **Open API:** Allow integration in 1st/3rd party mobile/web apps and services.

---

## 4. Complete Functional Requirements

### 4.1 User & Profile Management

- **Auth:**  
  Email/password, OAuth (Google, FB, Apple, Fitbit, Garmin), optional 2FA; session and device tracking; password reset.
- **Profile:**  
  Avatar (image/S3), display/user name, health details (age, DOB, height, weight, gender), notification preferences, privacy (public/private profile).
- **Multi-user:**  
  Allow trainers/gym accounts to create/manage multiple profiles for clients/members, with role management (trainer, client, admin).
- **Privacy Controls:**  
  Control data sharing (public/private), export (GDPR), or account deletion.

---

### 4.2 Workout & Exercise Management

- **Workout CRUD:**  
  Create, clone, update, and delete workouts (template or historical logs).
  - Fields: name, description, tags, visibility, planned date/time, coach-assigned or self-logged, summary statistics.
- **Exercise CRUD:**  
  Manage exercise catalog (per org/public); fields: name, type (strength/cardio/flexibility/etc.), muscle group, equipment, video/media link.
- **Session/Log Entry:**  
  Log a workout session: date/time, location (address/GPS), duration, subjective effort, notes.
  For each set: reps, weight, time, RPE/pain, heart rate (if tracked), observations, failure/completion status.
  Multiple exercises per session; supports rest intervals, supersets/giant sets, custom fields.
- **Progression & Templates:**  
  Assign templates (planned workouts) to self/clients; schedule weekly plans.
  Templates can be built, versioned, shared, or copied. Personal best (“PR”/1RM), streak, or progress toward goal, tracked and displayed.

---

### 4.3 Goals & Analytics

- **Goal Management:**  
  Set short/long-term goals for weight, reps, sets, distance, calories, activity streaks, sleep hours, or body composition.
  Inline metric visualization, daily/weekly analytics dashboard, personal best notifications.
- **Trends/History:**  
  Graphs: performance over time (weight lifted, speed, distance, etc.), progress toward goals, injury/rehab tracking.
  Table views and chart exports; export data for research/coaches/patients as CSV/Excel.
- **Custom dashboard views:**  
  Widget-based dashboards; select favorite/most relevant metrics to visualize.

---

### 4.4 Health & Body Metrics

- **Logging & Trends:**  
  Weight, body fat, muscle mass, waist, HR, BP, glucose, sleep, hydration, resting HR, VO2 max, temperature.
  Supports manual entry and automatic device sync; validation and trends analysis on each metric.
- **Reminders & Alerts:**  
  E.g., “Weigh in today”; “PR possible on deadlift!”; “Your BP is trending high—consider a break.”
  Delivered via email, in-app, SMS, or push.

---

### 4.5 Nutrition Integration

- **Food Log:**  
  Manual entry/logging per meal/snack with macro & calorie breakdown; multi-add and copy functionality; saved/favorite meals.
- **API Integration:**  
  Pull nutrition data for foods via Spoonacular, USDA, or similar public APIs (macro/micro info, portion sizing, food images).
- **Meal plans:**  
  Assign recipes/plans; scheduled reminders for tracking, auto-cloning for recurring meals.

---

### 4.6 Social & Motivation Features (Optional/Planned)

- **Challenges:**  
  Create/join challenges (e.g., “30-day plank”, “200km run in July”); leaderboards/achievements, badges.
- **Community:**  
  Share logs on activity feed, comment, react, and share templates/workouts.
- **Notifications:**  
  In-app, email, and push for challenge status, friend/follow milestones, PRs.

---

### 4.7 Search, Sort, and Discovery

- **Search:**  
  By workout, template, exercise, body metric, trainer, or tag.  
  Full-text/fuzzy search (Elasticsearch); auto-suggest for tags/exercises.
- **Sort and filter:**  
  By goal type, status, frequency, date, muscle, equipment, difficulty.
- **Library:**  
  Exercise gallery with videos, how-to, and best practices per movement.

---

### 4.8 File Handling & Device Data

- **File Uploads:**  
  Attach photos/videos to exercises/sessions, e.g., form checks or progress; quota per user/org. Preview in dashboard.
- **Device Integrations:**  
  OAuth/secure connections to third party APIs (Fitbit, Garmin, Apple Health, Google Fit, Withings, Oura, etc.).
  Regular or real-time sync of steps, workouts, sleep, HR, calories, device location.
  Device error/re-auth/status displayed in UI with historical sync logs.
- **Data Ownership:**  
  All raw/device data accessible/exportable per GDPR.

---

### 4.9 Accessibility, i18n, and Mobile

- **A11y & Responsive:**  
  Keyboard, screen-reader, high-contrast, touch gestures (for mobile).
- **Localization:**  
  All UIs, emails, notifications, and reports/csvs available in multiple languages; user can set language.
- **Mobile App support:**  
  API fully documented (Swagger/OpenAPI/GraphQL); REST available for mobile, wearable, web.

---

### 4.10 Security, Compliance, and Privacy

- **Auth & Session Security:**  
  JWT (RS256), OAuth, 2FA, session expiration, device management, role-based access enforcement.
- **Data Isolation:**  
  Per-user, per-org; resource operations filtered server- and query-side; coach/client RBAC.
- **File security:**  
  All files in S3/Minio, AV scan on upload, expiring presigned URLs, never public.
- **GDPR, HIPAA support:**  
  Data export/delete by user, consent banner, activity/audit logs, encryption at rest/in transit.
- **Audit Logging:**  
  Every user action, data change, and sensitive event is timestamped and attributed; audit trail exportable.

---

## 5. Developer/Architectural Requirements

### 5.1 Nx Monorepo Structure

- **Nx workspace:**  
  - `apps/api` (NestJS REST preferred; could add Apollo GraphQL for client-customizable dashboards, subscriptions for real-time)
  - `apps/worker` (BullMQ queue for reminders, device sync, analytics, challenge recalc, cron)
  - `apps/web` (Next.js/React for dashboards, client app)
  - `libs/db` (entities/migrations, TypeORM or Prisma for PostgreSQL)
  - `libs/types` (OpenAPI or GraphQL codegen for client/server contracts)
  - `libs/utils` (metrics, notifications, adapters)
  - `apps/devops` (Docker, scripts, documentation)
- **Typescript strict everywhere.**
- **Feature folders** in backend for scalability and clear separation.

---

### 5.2 API & Data Design

- **NestJS Modular REST API:**  
  - Controllers for users, workouts, exercises, metrics, goals, nutrition, integrations, notifications, files.
  - DTOs for strict validation, class-validator for complex types/regex rules.
  - Pagination, filtering, and search endpoints for all lists.
  - Rate limiting and strict error object for all API errors.
  - OpenAPI auto-docs for all endpoints, ready for frontends/mobile devs.
- **Database/Postgres:**  
  - UUID PKs, indexed relationships for user(s), trainer/client links, session id, exercise id.
  - Write-ahead logging and audit tables for compliance.
  - Soft deletes for logs/metrics, periodic archive jobs.
- **ElasticSearch:**  
  - For all large searchable fields: names, tags, descriptions, logs, notes.
  - Real-time index sync on CRUD; search APIs for instant discovery.

---

### 5.3 Queues, Worker Jobs, Integrations

- **BullMQ (Redis):**  
  - Queue for reminders, device sync, notifications, data pulls from external providers, challenge refresh, job scheduling (daily/weekly reporting).
  - Jobs must be idempotent, retries with exponential backoff.
  - Dead-letter queue for failures, admin alerting/logging.
- **Device/Nutrition APIs:**  
  - Integration adapters for Fitbit/Garmin/etc.; secure OAuth token store (encrypted), refresh and re-auth flow, detailed error/logging with UI surfacing for failures.

---

### 5.4 File, Media, Caching

- **Attach/image/video upload:**  
  S3/Minio path by user/year/workout or session id; presigned URL upload (API-wrapped), max size/type enforced.
- **Image rendering/preview:**  
  CDN-backed in prod, raw in local/dev.
- **Redis:**  
  Session storage, dashboard cache, notification pubsub, trending workouts, leaderboard stats.

---

### 5.5 Security & Testing

- **Password hash (Argon2/bcrypt, no plaintext).**
- **Strict DTO input/output validation everywhere.**
- **Helmet, CORS per domain/environment.**
- **Unit tests for all core logic; integration tests (API + DB + device/mock data flows) with ephemeral containers; E2E for all key flows (add workout, log session, sync device, receive reminder, export data).**
- **Sentry for error tracking, Prometheus for metrics.**
- **OWASP security checks, regular CVE audit.**
- **CI/CD:** GitHub Actions builds, tests, lints, dockerizes, auto-deploys; pre-commit hooks for lint and typecheck; pr build pipelines.

---

### 5.6 Frontend Design

- **React/Next.js:**  
  - Dashboards: overall stats, daily/weekly views, per-metric widgets.
  - Responsive from mobile to widescreen.
  - Workout entry, live editing, upload/check-in, analytics with Chart.js/D3.
  - File upload modal w/ drag-drop, status and preview.
  - Settings/Preferences, account management, notification center.
  - Accessibility audit, language switcher (i18next), dark/light mode.
- **Types generated from OpenAPI or GraphQL schema for strong-typed API access.**

---

### 5.7 Observability & Documentation

- **Health/readiness endpoints on all services (API, worker, queue).**
- **Structured logging (winston, context, requestId, userId).**
- **Prometheus metrics: request time, job duration, registration/engagement/active user trends.**
- **Auto-generated API docs (Swagger, GraphQL Playground); in-repo architecture and data flow diagrams.**
- **Seed scripts, sample datasets, responsive local development (`docker-compose up`).**

---

### 5.8 Extensibility

- **Feature flags: DB/env toggled for gradual rollouts and A/B tests.**
- **DB migration scripts; versioned, rollback-ready; backward compatible API releases.**
- **Adapters for nutrition/device providers swappable, able to extend for future API support (Apple Health, Samsung, Oura, etc.).**
- **Plugin system foundation for challenges, analytics, reporting modules.**

---

## 6. Success Criteria

- Users can log, edit, visualize all core data types (workout, exercise, body metric, goal) and retrieve it with privacy and compliance guarantees.
- All main flows are covered by extensive tests and observed in metrics/logs, with self-healing jobs and alerting for system failures.
- The system supports seamless device/API sync, file upload, and real-time notification under load.
- Entire stack runs locally and in CI; onboarding for new developers is short and documented; new features can be added via modular plugins.
- All compliance and privacy flows (data export, account deletion, secure storage) supported as per regulatory requirements.
