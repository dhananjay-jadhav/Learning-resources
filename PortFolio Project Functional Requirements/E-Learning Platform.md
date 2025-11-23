# E-Learning Platform – Requirements

---

## 1. Product Vision

A modern, scalable e-learning platform for educators, students, organizations, and coaches. Supports course creation (live/recorded), enrollment, modular learning, assignments, progress tracking, live classes, interactive quizzes, discussion, notifications, analytics, certification, and deep integration with external resources. API-centric, multilingual, a11y-first, resilient, and extensible for self-hosted or SaaS deployment.

---

## 2. Target Audience

- **Educators & Trainers:** Individual teachers, corporate trainers, universities, schools, and coaching centers wanting to deliver structured courses, assignments, and assessments.
- **Students/Learners:** Individuals enrolling for self-paced or instructor-led learning, tracking progress, submitting assignments, and earning certificates.
- **Organizations:** HR/L&D teams aiming to manage onboarding, compliance, and upskilling with reporting and multi-role management.
- **Content Creators & Coaches:** Subject matter experts sharing courses, quizzes, video content, and selling access/subscriptions.
- **Developers/Integrators:** Embedding or extending e-learning into parent portals, SaaS tools, or mobile apps.

---

## 3. Key Value Propositions

- **Comprehensive learning:** Courses, modules, lessons (video, text, files), assignments, quizzes, and live classes, all connected by user progress logic.
- **Powerful authoring:** Drag-drop/copy/collaborative authoring tools, reusable modules, rich file/media support, and versioning.
- **Engagement & interaction:** Threaded discussion, announcements, email/SMS/app notifications, gamified badges, points, leaderboards (opt-in).
- **Assessment & feedback:** Auto-graded quizzes, assignment submission, peer/self/mentor review, instant feedback, certificates.
- **Analytics:** Dashboards for engagement, scores, completion, retention, instructor effectiveness, group progress and per-user journey.
- **Integration & extensibility:** SCORM/xAPI import, external API for SSO, LTI, CRM/HRIS, webhooks, and plugin support.
- **A11y, privacy, and standards:** Multilingual, accessibility-first (WCAG 2.1 AA+), GDPR/FERPA/COPPA ready.

---

## 4. Complete Functional Requirements

### 4.1 User, Organization & Role Management

- **Authentication:**  
  Email/password, OAuth (Google, MS), SSO (SAML, OIDC), 2FA optional, device/session view, password reset, email verification.
- **Profile management:**  
  Avatar/S3, bio, timezone, skills/interests, user type (student/coordinator/instructor/admin), notifications preferences.
- **Organization/Group:**  
  Multi-org tenancy, invite users, assign courses/groups/batches; team/role management (admin, instructor, reviewer, mentor, student).
- **Role-based access:**  
  Fine-grained RBAC for courses, content, learners, and admin pages.
- **GDPR-compliant data export/delete.**

---

### 4.2 Course Creation/Authoring

- **Course CRUD:**  
  Create, update, duplicate, delete, archive. Public|private|paid|invite-only|drip.
  - **Structure:** Multi-module, each w/ lessons (markdown, video, SCORM, attachment), quiz/assignment, estimates, order.
  - **Content Upload:** Video (mp4, external link, embed), docs, images, slides, external links (YouTube, Google Drive), S3/Minio file manager.
  - **Draft/version:** Autosave, revision, draft/publish status, rollback.
  - **Collaboration:** Multiple authors, comments/feedback, activity log.

- **Reusable resources:**  
  Library of reusable lessons, questions, slides, checklist, templates.

---

### 4.3 Enrollment, Access & Payments

- **Enrollment flows:**  
  Self-enroll/public sign-up, instructor/invite-only/add, code or payment gated (Stripe/PayPal integration).
  Pre-reqs, waitlist, batch start/end scheduling, multiple roles per enrollment.
- **Subscription/payment:**  
  Recurring billing or one-off via Stripe/PayPal, course/package plans, refund/pro-ration, invoices/receipts and export.
- **API for 3rd-party enrollment/HRIS sync.**

---

### 4.4 Lessons, Assignments, Quizzes

- **Lesson delivery:**  
  SCORM/xAPI, video, slides, text, downloadable files, quizzes inline, PDF/sheets, reading/assignment logs.
- **Assignment workflow:**  
  Instructions, file upload, plagiarism check, submission status, deadline/late policy, instructor/peer/self review, rubric grading, feedback loop.
- **Quizzes:**  
  Multiple types: MCQ, open text, drag-drop, code, images.  
  Timed, randomization, question banks, explain after, instant/manual grading.
- **Progress logic:**  
  Linear (must pass module X), branch, skipping/placement test, return-to-prev, unlock-on-complete.

---

### 4.5 Interaction, Discussion & Engagement

- **Announcements:**  
  Course/group/module targeted, in-app/email/push/SMS, scheduling, history.
- **Threaded Discussion:**  
  per lesson|module|course; markdown, attach, tag users, reply/mention moderator flagging.
- **Live classes:**  
  Integrated video embed (Zoom/Jitsi/BBB/Webex), calendar invites, attendance export, recording upload/link, breakout groups, chat.
- **Gamification:**  
  Badges, leaderboards, progress bar, streaks, points, unlocks, config by course or org.

---

### 4.6 Progress, Analytics, & Certification

- **Progress tracking:**  
  Per student and group, visual timeline, unit/course reporting, streaks, email/app reminders for inactivity or deadlines.
- **Analytics dashboards:**  
  Per student|org|batch: enrollment funnel, video watched %, quiz stats, assignment scores and feedback, completion, time on task, drop-off/churn points.

- **Certificate engine:**  
  Configurable templates, auto/gated issue (based on completion/grade), export PDF/badge, public verification API.

---

### 4.7 Search, Export, Compliance

- **Search:**  
  Elastic-powered, instant search for courses, lessons, users, tags, attachments, with autocomplete. Fuzzy search + advanced filter (quiz, assignment, instructor, date, progress).
- **Export:**  
  Pdf/csv/xls export: users, grades, submissions, course/quiz/assignment stats, engagement logs.
  - **Audit Logs:** All content, enrollment, grades, score changes, login,  privileged actions. Configurable retention per org/region.
- **Accessibility & i18n:**  
  All UI, scripts, and notifications localizable; keyboard/screenreader tested; a11y automated in CI.
- **Compliance tools:**  
  Parental consent, GDPR data access/erase, academic integrity checks.

---

## 5. Developer & Architectural Requirements

### 5.1 Monorepo, Structure, Stack

- **Nx workspace:**  
  - `apps/api` (NestJS REST primary, Apollo GraphQL optional for advanced reporting/analytics, modularized by domain).
  - `apps/worker` (BullMQ for jobs: notification, grading, reporting, reminder, backup).
  - `apps/web` (React, SSR for SEO).
  - `libs/types` (OpenAPI/GraphQL codegen for shared types, strong typing).
  - `libs/db` (Prisma or TypeORM/PostgreSQL, all migrations, audit, assignment/history tables).
  - `libs/utils`, `apps/devops` (docker, infra, scripts).
- **Strict TypeScript everywhere.**
- **Domain-driven folder structure, per-feature code/app separation.**

### 5.2 Backend/API Logic

- **REST API:**  
  Versioned (`/api/v1`), DTO input+output validation, error wrapping by code/message/detail.
- **Authentication:**  
  Passport (JWT, OAuth, SAML) integration; user role context in all requests.
- **RBAC:**  
  Role/permission guards on every endpoint and at resolver/service/db query level; per-org/per-course scope.
- **OpenAPI (Swagger)** for all endpoints, used for frontend and docs.
- **GraphQL (option):**  
  Code-first, federation support, complexity-limiting, subscription (live quiz/events).

### 5.3 Data, Search, Queue

- **PostgreSQL:**  
  UUID pks, audit, soft/hard delete, all relations, trigger-enforced referential integrity; scheduled backups.
- **ElasticSearch:**  
  Courses, users, content, discussion—fast, facet, fulltext search.
- **Redis:**  
  Hot cache, session, pubsub for real-time chat/notification, background jobs.
- **BullMQ workers:**  
  Grading, reminders, scheduled backup, notification, certificate render; job results logged/audited.

### 5.4 File, Content, Media Handling

- **S3/Minio:**  
  For video, docs, images, course templates; signed urls for download/upload; AV scan, preview, quotas/billing per user/org.

### 5.5 Integration/Extensibility

- **Third-party:**  
  SCORM/xAPI ingest, LTI 1.3 endpoint, SSO, payment processors (Stripe), calendar APIs, notification SMS/email (SendGrid/Twilio).
- **Pluggable plugin system** for custom blocks, widgets, grading.
- **Webhooks:**  
  Outbound triggers for completion, submissions, scores, reminders.
- **API tokens:**  
  Custom app/client integrations; admin/role/list-based API key system.

### 5.6 DevOps, Testing, CI/CD

- **Docker Compose up for all dependencies:**  
  Postgres, Redis, S3/Minio, Elastic, web/api/worker, local SMTP/dev mail.
- **Helm for prod, K8s readiness.**
- **GitHub Actions for build/lint/test/typecheck/coverage/dockerize/deploy.**
- **Pre-commit: lint, audit, format, test.**
- **Testing:**  
  Jest (unit, integration), Playwright/Cypress E2E for all major flows, seed scripts.
- **Observability:**  
  Prometheus, Sentry, winston logs, `/health`, `/metrics`, `/readiness` endpoints, error monitoring, slack alerting.

### 5.7 Accessibility, i18n, Docs, Maintenance

- **A11y checks in pipeline (axe-core) and CI blockage for failures.**
- **i18next for all UI/text; date, currency, numeric and calendar localization.**
- **Auto-generated API docs (swagger, graph playground), ER diagrams, code standards, arch/infra diagrams in markdown/mermaid.**
- **Feature flags:** Granular enable/disable, logs/audit flag changes.
- **Changelog, semantic release, dependency audit on schedule.**

---

## 6. Success Criteria

- Educators can launch, schedule, and run complex courses independently via UI or API.
- Students and admins can enroll, track, engage, and certify without friction or staff help.
- The stack handles thousands of concurrent learners with low error/latency under real load and scales elastically.
- Accessibility, security, privacy, and compliance workflows fully implemented, documented, and tested.
- Local/dev onboarding for devs is <30min, with CI coverage, reproducible “one-command-up”, and all flows verifiable from seed data.

---
