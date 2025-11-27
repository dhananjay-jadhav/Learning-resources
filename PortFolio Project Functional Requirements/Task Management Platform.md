# Task Management Platform – Requirements

---

## 1. Product Vision

A collaborative, secure, and extensible task management system for individuals and teams, supporting the full lifecycle of projects and tasks, advanced collaboration, data analytics, external tool integration, and robust operational excellence. The goal is a scalable platform useful for solo professionals, agencies, and enterprises with real-time collaboration, workflow automation, advanced reporting, and compliance needs.

---

## 2. Target Audience

- **Individual professionals:** Freelancers, consultants, or anyone handling multiple personal or client projects, needing centralized management, reminders, secure file storage, and audit/history of interactions.
- **Small/medium teams:** Teams must manage parallel projects, divvy up tasks, see progress in real time, share files securely, and avoid duplicate communication/churn.
- **Project managers:** Need macro- and micro-views (dashboards, boards, calendar, analytics), bottleneck alerts, exportable reports, and flexible assignment.
- **Organizations (with compliance):** Businesses, non-profits, or regulated industries that value data isolation, full audit trails, export for GDPR, granular role management, and API access.
- **Educational groups:** Classrooms, study circles, and collaborative learning teams managing assignments, deadlines, and submission audits.
- **Agencies/service providers:** Need org separation, custom reports per client, quota management, private file sharing, and project clones/templates.

---

## 3. Key Value Propositions

- **Unified work hub:** Create, assign, and track tasks/projects with all files and communications in one secure location.
- **Flexible visualization:** Toggle between Kanban (board), Calendar, Gantt, and List/Table for any project.
- **Real-time collaboration:** Task updates, comments, file sharing, and notifications propagate instantly across all devices and active team members.
- **Data-driven management:** Built-in analytics, trend-spotting, overdue reporting, and project dashboards for insights/decisions.
- **Deep integration:** Two-way sync with Google/MS Calendars, Slack/Teams, exposed API, and outbound webhook automation.
- **Compliance-ready:** Logging, audit trails for every action, permissions, and data privacy tools for modern organizational needs.

---

## 4. Complete Functional Requirements

### 4.1 User and Organization Management

- **Sign-Up & Login:**  
  - Register by email/password or OAuth (Google/MS).  
  - Email verification on registration (with customizable email templates).  
  - 2FA (TOTP) optional per user/org policy.  
  - Account lockout after configurable failed attempts; per-device/session login view.
- **Profile Management:**  
  - Edit name, avatar (with image CDN/S3 uploads), time zone, language/locale, and notification preferences.
  - View and manage all org memberships (multiple orgs w/ role tracking).
  - Account deletion (with data export/download and final acknowledgment modal).
- **Organization & Roles:**  
  - Create/join organizations with unique namespace; each org is a top-level workspace.  
  - Role system: Owner > Admin > Member > Guest (with row- and field-level RBAC).  
  - Invite by email (with role preassignment), accept/decline onboarding flow, pending invites visible to admins.
  - Remove, ban, or transfer ownership (full audit log, reason required).
  - List and search for org members with filters for last active, projects involved, and role.
  - GDPR "Right to be Forgotten" for orgs and users: hard delete, with audit trail scrubbing.

---

### 4.2 Projects and Task Management

- **Projects:**  
  - CRUD via API/web, attributes: name, color, description, created/updated/archivedAt, custom metadata.
  - Archiving disables edits but preserves for reference.
  - Member-level/project-level granularity for access.
  - Chat thread per project (real-time, markdown, basic @mention support).
  - Dashboard view: tasks by status/user/date, activity feed, recent file uploads, upcoming deadlines.
- **Tasks & Subtasks:**  
  - CRUD, unlimited per project.  
  - Attributes: title, description (markdown), priority, estimate (time/story points), due date/time (UTC/user'S TZ aware), list of assignees, tags, one-level subtasks, parent_id, dependencies (blockers, precedents), recurrence (RFC 5545), completedAt, soft-deleted, attachments.
  - Batch actions: bulk assign, status change, move/copy between projects, batch upload via CSV.
  - Task sorting and grouping by field(s) (examples: by assigned user, by due date).
  - Recurring tasks spawn automatically according to CRON/recurrence pattern.
  - Import/export: CSV, XLS, zipped JSON (w/ attachments for export).
  - Task history/audit: every field change, assignment, status shift, attachment, or comment has a timestamped record; timeline UI exposes this per task and at project level.
  - Deletion: soft (flags, recoverable) — permanent deletion via scheduled batch after N days (policy-configurable).
- **Assignment & Notification:**  
  - Assign/reassign (instantly notifies assignee(s)).
  - Notification (in-app, email, push, webhook/chat) on assignment, mention, status changes, comments/files, overdue/reminder.
  - User notification center: filter by type, mark as read, clear all, snooze.

---

### 4.3 Comments, Discussion & Collaboration

- **Task & Project threaded comments:**  
  - Markdown, emojis, file attachments per comment (images/docs/videos, previews for common formats).
  - Edit/delete by author (with complete edit history for admins).
  - @mention users or teams; triggers notification; clickable profile links/hovercards.
  - Per-task/project activity feed: comment, assign, status, file upload/download, deadline adjust, etc.
  - Bulk operations: resolve/comment on multiple tasks/projects in one action (API/UI/worker), automations for "bulk done"/"bulk reassign".

---

### 4.4 Analytics, Reporting & Dashboards

- **Per-user/project/org dashboards:**  
  - Kanban, Gantt, Calendar, and List/Table views toggle via UI state.
  - Key stats: open/overdue tasks, sprint burndown, completion rate, time-to-completion, per-user load, recent activity, idle tasks flagged.
  - Downloadable reports: CSV, XLS, PDF (async job w/ email notify if takes >2s).
  - Weekly summary emails and export automation settings.
  - Advanced: custom chart builder (filters, fields toggled, group by/tag by), snapshot sharing for stakeholders.

---

### 4.5 External Integrations

- **Calendar Sync:**  
  - OAuth2 for Google/MS, per-user preferences, per-project mapping.
  - Two-way sync (create/update/delete → calendar as event, calendar update → task due date recalc/notify).
  - External API failures logged and visible in UI (w/ user warning + retry option).
- **Public Holiday Detection:**  
  - On task create/update, backend queries external API (country/state aware, cache per week/country) to flag deadline conflicts.
- **Outbound Webhooks:**  
  - Project/task status changes, comments, or assignments can trigger custom webhook POST (with env-config HMAC signing per org, delivery logs visible to admins).
- **Chat Integration:**  
  - Optional: Slack/MS Teams/Discord notifications for project/task events (configurable per org/project/channel).
- **Public API:**  
  - OAuth/app tokens or API keys; docs auto-generated (Swagger/OpenAPI), allows custom dashboards/integrations with external services.

---

### 4.6 Search & Organization

- **Global/contextual search:**  
  - Elasticsearch for all indexed: tasks, comments, project fields, assignees, tags, attachments (index on write).
  - Faceted UI: combine filters (AND/OR/group by status/assignee/etc.), autocomplete with real-time suggestions.
  - Saved filters & views (“Smart Views”), searchable/cloneable per user/team/org.
  - Recent activity and favorites auto-updated from use/events.

---

### 4.7 File Storage & Sharing

- **Attaching files to tasks/comments/projects:**  
  - S3/Minio for all uploads/files, path structure covers org/project/task.
  - Direct-to-S3 upload support, secure download (expiring signed URLs), content-type/virus scan via stub/job.
  - Quotas by role/org/plan enforced on backend.
  - File list per resource, sortable, preview in UI (images, PDFs, videos, etc.).
  - File events (upload, download, delete) logged in per-resource audit.

---

### 4.8 Accessibility & Internationalization

- **WCAG 2.1 AA compliant UI:**  
  - All navigation/context menu/modal/dialog via keyboard.
  - ARIA-labels, high contrast, readable colors, screenreader tested.
- **Localization:**  
  - UI, emails, and notifications support multiple languages, fallback to English.
  - All interface text and system messages in resource files, extensible via i18next or similar.
  - User settings include locale preference; all date/time/currency formatted accordingly.

---

### 4.9 Security & Compliance

- **Auth:**  
  - JWT (RS256) for stateless, cookie or header-based sessions, renew via refresh tokens. Passport.js (NestJS) for all strategies.
  - OAuth2 (Google/MS) for SSO/UI, backCHANNEL tokens encrypted at rest.
  - Argon2 or bcrypt (>=12 rounds) for password hashing; passwords never stored or logged.
- **RBAC:**  
  - Route and service-level guards in Nest/TypeORM; can only access resources in allowed org/project/role scope.
  - All leaks or invalid access attempts logged and surfaced in UI for admin review.
  - Per-field permission system for sensitive data.
- **API Safety:**  
  - Input sanitation (class-validator for DTOs), output field filtering (class-transformer), safe error/exception handling (no stacktrace in prod).
  - Rate limiting at endpoint/user/ip/plan levels.
  - All secrets/config via environment or remote vault/secrets store; rotateable without redeploy.
  - Helmet+CORS enabled; dynamic origin list per-stage.
- **Audit logging:**  
  - Deep event logging—CRUD, login/logout, role changes, file access, API key use, webhook invocations.
- **GDPR:**  
  - Data export endpoints, account/org delete, complete data scrubs, background job for hard-delete.

---

### 4.10 MongoDB-Powered Features & Learning Objectives

In addition to relational data in PostgreSQL, the system will add a dedicated MongoDB database for select features that are naturally document-oriented, suited for flexible/no-schema storage, or require handling large, nested, or rapidly mutating data. This enables in-depth learning and hands-on experience with MongoDB (from basics to advanced).

#### 4.10.1 MongoDB Use Cases

- **Activity & Audit Logs (Uncapped, Nested, and Time-Series):**
  - Store all raw activity/audit/event logs in MongoDB as documents, including unstructured metadata, request payloads, computed diffs, and trace context.
  - Use time-based partitioning/TTL collections for auto-expiry and efficient querying/scanning.
  - Implement aggregation pipelines and MapReduce for analytic queries over log/event data.

- **User Action History/Undo Stack (Versioning & Temporal Documents):**
  - Every user’s “undo stack” (recent actions that could be reverted) is modeled as a MongoDB document with embedded subdocs per action, including context, before/after states.
  - Learn and demonstrate upsert, array-updating operators, positional/document matching, anchors, nested querying.

- **Document Attachments Metadata (Meta, Hierarchical, Taggable):**
  - Store all file attachment metadata in MongoDB (the actual file stays on S3/Minio), leveraging document references: a single doc per file with arbitrary tags, rich user comments, review history, and derived properties.
  - Practice MongoDB indexing for fast lookups, compound/partial/text indexes, and covering queries.

- **Real-Time Collaboration State (Ephemeral Session State):**
  - Use MongoDB as a backing store for in-progress edit/collaboration buffers (e.g., when multiple users are editing the same project/task note), using change streams, atomic document updates, and concurrent editing patterns.
  - Learn about shard keys, optimistic/pessimistic concurrency, and watch/subscribe via MongoDB drivers.

- **Notification Preferences (Dynamic, Arbitrary Schemas):**
  - Support user-customizable notification rules—stored as arbitrary JSON structures in MongoDB and interpreted at runtime (e.g., “when X happens, send email/push to Y devices unless Z”).
  - Query/filter on deeply nested fields, validate against JSON schemas.

- **Learning Objectives Checklist** (demonstrated via above features):
  - Get hands-on with CRUD, updates, deletes, upserts, aggregations, indexes, text search, TTL, geo-queries, and schema validation.
  - Master advanced MongoDB: transaction support, bulk writes, GridFS basics, change streams, sharding, performance tuning, and backup/restore/replica sets (for dev clusters).
  - Integrate strongly with the NestJS API (using Mongoose or the official driver) for best practices in schema management, lifecycle hooks, and data modeling in real code.

#### 4.10.2 Developer Experience

- **Database Bootstrap:** Seed scripts will auto-populate MongoDB with sample event logs, files metadata, user undo histories, and notification rules for playground/demo use.
- **Admin & Explore UI:** Dedicated admin pages and “Mongo Playground” to run test queries, inspect collections, run aggregations, and view schema/model examples—all within your working app.
- **Observability:** Metrics and logs for MongoDB usage, slow queries, replication/availability alerts, and “last query” explorer.

**Note:** These MongoDB features are additive to existing PostgreSQL models; primary relational data (users, projects, tasks, workflow states) remain in Postgres to reinforce hybrid RDBMS + NoSQL architecture best practices.

---

## 5. Developer/Architectural Requirements

### 5.1 Nx Monorepo/Structure

- Nx workspace with: `apps/api` (NestJS), `apps/worker` (NestJS Queue/Cron), `apps/web` (React), `libs/types` (TS API models), `libs/db` (entities/migrations), `libs/utils` (common), `apps/devops` (compose, scripts, docs).
- Use Nx code scaffolding for all module generation, keep clear folder boundaries.
- Libraries versioned and changelogged for all API-breaking updates.

### 5.2 Database/Persistence

- PostgreSQL 15+ with UUIDs, all relations, indices, soft deletes, and indexed metadata for search/caching.
- TypeORM entity models and migration scripts, validated in CI pre-merge.
- DB changes only via migrations, version locked.
- File metadata (name/type/hash/url/size/owner) in DB with reference for every upload/download/delete/integration event.

### 5.3 API & Service Design

- Strict RESTful resources, versioned in URL (`/api/v1/...`).
- All input/output DTOs with strong typing, class-validator rules.
- NestJS OpenAPI (swagger) generation, custom example payloads for docs/testing/mock data.
- Output formatted via interceptors for field masking (private fields not returned by default).
- All errors/exceptions handled and standardized (custom error class mapped to HTTP status, including error codes for all failure modes).

### 5.4 AuthZ/AuthN

- JWT (bearer headers only, APIs locked by default except for login/register/public docs).
- Per-user, per-session audit logs (uuid, ip, ua, last activity, failed logins).
- Role and permission checks via custom NestJS decorators at controller and service layer.
- Password reset tokens in Redis, one-off usage, audit trail for each attempt/success/fail.
- Org-level AND project-level access checks at both route/controller and DB query level.

### 5.5 Queues & Async Jobs

- BullMQ jobs (in Redis):  
  - Reminders (per-user/per-task/date),  
  - Recurring-task generation,  
  - Weekly/monthly reporting emails,  
  - File hard deletes,
  - Calendar & external sync,  
  - Webhook retries (with DLQ).
- Workers have logging, idempotency, Sentry/error reporting, and heartbeat monitoring.
- All job configs and attempts visible in UI (admin only).

### 5.6 External Integration Pattern

- All outgoing calls (Calendar, Holidays, Chat, Webhook) are async and always retried with exponential backoff and circuit breaker (down time alert to admin).
- Integrations configured per-org via UI/settings; keys encrypted and rotated with admin event log.
- All major integration events (success/failure/disabled) raised as admin notifications.

### 5.7 File Architecture

- S3 or Minio integration (localhost), writable via signed URLs. In production use S3; in local/dev support single-bucket Minio.
- All uploads validated for type/size at both UI & API level.
- Virus scan via stub, all results logged; files tagged as "suspicious" not served until review.
- Files stored in per-org/project/task folder hierarchy.
- File download gets expiring signed URL, all downloads audited.

### 5.8 Search & Caching

- Elasticsearch 8.x for all indexed fields, with per-org filtering (multi-tenant).
- Instant write-to-index on any CRUD event.
- Redis used for session, dashboard hot queries, fulltext cache, and event pubsub.
- Admin tool/scheduled job for reindex broken/corrupt/missing records.

### 5.9 Observability, Logging, Deployment

- Winston/ELK/Sentry for logs: message, user/session/context, trace id, endpoint, status.  
  - All errors, all warn, API and worker logs shipped to ELK/Sentry in prod.
- Prometheus metrics: request latency, error rate, redis/queue depth, slow query, API events, health.
- `/metrics`, `/health`, `/readiness` endpoints (API/worker), HTTP 200 for green.
- Docker Compose: one command brings up API, web, worker, postgres, redis, minio, elastic, all pre-configured; local seed script for demo data.
- Production: Helm chart file for k8s, secrets via volume/env, built-in resource requests/limits.

### 5.10 Testing and CI/CD

- Unit tests via Jest, 90%+ coverage and branch check.
- Integration tests: spin up containers (testcontainers/compose), cover CRUD, integration, async jobs, file flows.
- E2E via Cypress or Playwright, test all user flows/scenarios under simulated load.
- Load tests: Artillery/JMeter, simulate 100+ workers, validate SLOs.
- Security scan: OWASP ZAP/Burp, code analysis for CVEs.
- Github Actions: test/lint/typecheck on PRs/branches; auto-deploy to staging/preprod; Docker multi-stage builds and artifact push.

### 5.11 Lint, Format, Docs, Accessibility

- ESLint strict, no disables allowed; Prettier on save/commit (husky).
- Docs: README, sample envs, bootstrapping, error codes, API docs, integration setup, diagrams in Mermaid/Draw.io.
- UI: React components tested for accessibility, axe-core; ARIA roles, keyboard navigation ensured.

### 5.12 Extensibility and Maintenance

- All new features behind DB/env flags via OpenFeature or custom flags module.
- Feature toggles auditable: show who enabled/disabled, when, why.
- Weekly CVE/dependency audit; update script for core libs, API agrees with frontend on shared contract, changelogbot for docs.

---

## 6. Success Criteria

- All features testable by E2E and covered by integration/unit tests; green CI build means deploy-ready.
- All organization/user data isolated, exportable, and deletable per UI flow and API.
- All integrations tested for success/fail modes with fallback handling.
- System can be booted from scratch on any dev system in <15 min, with all base flows available on localhost.
- Documentation and code quality standards enable onboarding of a new engineer in <4 hours.

