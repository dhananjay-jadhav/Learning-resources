````markdown name=ProjectIndex.md
# Project Index and Tech Stack Summary

This section provides an at-a-glance summary and backend/frontend/infrastructure technology map for each system, including brief rationales for architectural choices. Designed for easy reference and as a learning aid for system design and modern SaaS stack division.

---

## 1. Task Management Platform

- **Backend API:** Nx Monorepo + NestJS (modular REST API, DTO/validation, OpenAPI docs)
- **Queue/Worker:** BullMQ (NestJS process) using Redis for reminders, jobs, notifications
- **Database:** PostgreSQL (with TypeORM for schema/migrations, UUIDs, strict constraints)
- **Search:** Elasticsearch (tasks, comments, projects, fulltext & advanced filter)
- **Cache/Session:** Redis (user sessions, board/dash precompute, pub/sub notifications)
- **File Storage:** S3 (AWS) or Minio (local/dev) via presigned URLs
- **Frontend:** React (Nx, Redux Toolkit/RTK Query or React Query, SSR capable, a11y-first)
- **Authentication:** Passport.js (JWT, Google/MS OAuth, 2FA)
- **Docs & Types:** Shared types in Nx libs, auto-generated OpenAPI, Markdown/Swagger UI
- **Infra/Operations:** Docker Compose (local/dev), Helm for production (Kubernetes-ready), AWS ECS/EKS as cloud deployment option, S3/Elasticache/Elasticsearch in AWS
- **Monitoring:** Prometheus & Grafana (metrics), Sentry (tracing/errors), Winston logs (ELK-forwarded)
- **Internationalization:** i18next
- **Security:** RBAC in API/routes, encrypted secrets via AWS Secrets Manager
- **System Design Note:** API, worker, search, and file-storage run as independently scalable pods/services, following modern microservice/cloud-native patterns for future scale-out to AWS (EKS or ECS, S3, OpenSearch, etc.)

---

## 2. Book Library System

- **Backend API:** Nx Monorepo + NestJS (primary API) + Apollo GraphQL (complex search, personalized fetch, UI flexibility)
- **REST:** For file uploads, webhooks, and backward-compat integrations
- **Worker:** BullMQ (Node/Nest) for import, enrichment, notification, background processing
- **Database:** PostgreSQL (TypeORM/Prisma), option for MongoDB (user highlights/notes)
- **External APIs:** OpenLibrary, Google Books, ISBNdb, barcode scan integration
- **Search:** Elasticsearch (books, authors, shelves, fuzzy/discovery)
- **Cache:** Redis (sessions, activity, trending, pub/sub)
- **File Storage:** S3/Minio (cover art, attachments, user files)
- **Frontend:** Nx + React (Apollo Client, SSR/SPA, strong GraphQL-generated types)
- **Authentication:** JWT/Auth guards, Google/Apple OAuth
- **Reporting/Export:** CSV/XLS/JSON export, PDF generation (job-queued)
- **Infra/DevOps:** Docker Compose (local), Helm+K8s (cloud), AWS S3/Elasticache/OpenSearch
- **Monitoring:** Prometheus, Sentry, Elastic logs
- **Internationalization:** i18next, locale detection
- **System Design Note:** Apollo GraphQL enables highly flexible UI and advanced filtering, while REST endpoints remain for external APIs and webhooks. Backend and worker can be deployed/scaled as services. AWS deployment uses EKS, S3, OpenSearch, SES/SNS for notifications.

---

## 3. Event Registration Platform

- **Backend API:** Nx + Express + Postgraphile (auto-GraphQL/REST from PostgreSQL with rich constraints, direct data mapping)
- **Custom Express:** For flows needing custom logic (webhooks, payments, SMS/email triggers)
- **Worker/Queue:** Node/NestJS + BullMQ for emails, reminders, check-ins, exports
- **Database:** PostgreSQL (strict schema, audit logs, inventory/ticket tracking)
- **Payments:** Stripe/PayPal adapters (pluggable, webhooks, PCI-compliant)
- **Search:** Elasticsearch (events, sessions, ticket search/filter)
- **Cache:** Redis (sessions, ticket cache, rate limits, pub/sub for real-time updates)
- **File Storage:** S3/Minio (event resources, PDFs, QR codes)
- **Frontend:** Next.js (Nx) + React (SSR for SEO, role-specific dashboards, responsive/mobile)
- **Authentication:** JWT/OAuth, SSO/SAML (enterprise)
- **Infra/DevOps:** Docker Compose (dev), prod with Helm/K8s, AWS RDS/S3/Lambda (for fast check-in API), SES/SNS for mails/SMS, CloudFront for CDN
- **Analytics:** Prometheus, Grafana dashboards, Sentry (error log)
- **Internationalization:** i18next
- **Security:** PCI compliance, RBAC, rate limiting, audit logs
- **System Design Note:** Postgraphile excels for a data-centric, relation-rich event domain with many CRUD and query/reporting requirements and rapid API evolution. Payments and check-in are handled as separate horizontally scalable worker services.

---

## 4. Personal Finance Tracker

- **Backend API:** Nx + NestJS REST API (v1), strong DTO/validation, OpenAPI docs, ready to support GraphQL for custom reporting/flex queries
- **Worker/Queue:** BullMQ (Nest/Node) for bank sync, import, alerts, scheduled reports/exports
- **Database:** PostgreSQL (TypeORM/Prisma), relational: accounts, transactions, rules, budgets, categories, goals
- **External Integrations:** Plaid/TrueLayer (OAuth, banking), Yahoo Finance, CoinGecko
- **Search:** Elasticsearch for transactions/notes/payees/full-text
- **Cache:** Redis (session, query cache, notification pub/sub)
- **File Storage:** S3/Minio (attachments, receipts)
- **Frontend:** Nx + React (Redux/RTKQ, charts/analytics, mobile-first)
- **Authentication:** JWT/OAuth/2FA, device/session management
- **Infra/DevOps:** Docker Compose, AWS ECS/EKS, S3, RDS, OpenSearch, CloudWatch, Lambda for batch imports
- **Monitoring:** Prometheus, Sentry, Winston logs
- **i18n:** i18next, currency+locale aware
- **Security:** PCI, RBAC, encrypted keys/secrets, GDPR/CCPA flows
- **System Design Note:** Strict modular separation of data ingestion (bank/crypto APIs), reporting (workers/queue), and API, with a view toward future ML analytics. All integrations are async, error-handling robust, and easy to swap for providers.

---

## 5. Fitness Tracker API

- **Backend API:** Nx + NestJS (REST + planned Apollo GraphQL for dynamic dashboards/queries)
- **Worker/Queue:** BullMQ (Nest) for reminders, scheduled sync (device, nutrition), long-running data jobs
- **Database:** PostgreSQL (user/exercise/session/goal/log/history/pr), optionally MongoDB for raw device logs
- **Device Integrations:** OAuth for Fitbit, Garmin, Apple Health APIs, nutrition (Spoonacular/USDA)
- **Search:** Elasticsearch (exercise/catalog, logs, notes)
- **Cache:** Redis for session, metrics, notification pub/sub, trending analytics
- **File Storage:** S3/Minio (progress photos, videos)
- **Frontend:** Nx + React/Next.js (mobile-ready, dashboard widgets, accessibility tested)
- **Authentication:** JWT/OAuth, device token handling
- **Infra/DevOps:** Docker Compose, Helm/k8s, AWS S3, RDS, Lambda for scheduled tasks, Prometheus/Sentry for metrics/logging
- **Internationalization:** i18next, all units user-configurable (metric/imperial)
- **Security:** HIPAA-ready option, RBAC, full audit trails, data export/deletion per GDPR
- **System Design Note:** Device and integration job workers separated from core API, leveraging background jobs and independent horizontal scale. CRDT/OT considered for live session tracking (future).

---

## 6. Movie/Media Watchlist Manager

- **Backend API:** Nx + NestJS + Apollo GraphQL (rich, dynamic entity fetch, subscriptions for real-time updates), REST for batch imports/files
- **Worker/Queue:** BullMQ (Node/Nest) for enrichment sync, notification, trending calc, exports
- **Database:** PostgreSQL (core), option for hybrid PG+Mongo for rich note/review blobs
- **External APIs:** TMDb, OMDb, JustWatch (fetch metadata; scheduled enrichment jobs)
- **Search:** Elasticsearch for titles, actors, genres, reviews, list discovery
- **Cache:** Redis (session, trending, pub/sub for updates)
- **File Storage:** S3/Minio (artwork, reviews, user uploads)
- **Frontend:** Nx + React/Next.js (SSR for SEO, responsive, mobile friendly)
- **Authentication:** JWT/OAuth, SSO providers
- **Infra/DevOps:** Docker Compose, AWS EKS, S3, OpenSearch, SES/SNS
- **Monitoring:** Prometheus, Grafana, Sentry, winston logs
- **Internationalization:** i18next, locale-sensitive
- **System Design Note:** GraphQL enables powerful recommendations/discovery features and social integration (activity feed, sharing, etc.), leveraging subscriptions for friend activity in real time.

---

## 7. E-Learning Platform

- **Backend API:** Nx + NestJS REST (v1) + optional Apollo GraphQL (advanced analytics, activity feeds)
- **Worker/Queue:** BullMQ (Nest) for grading, notifications, reminder jobs, grading jobs, backup
- **Database:** PostgreSQL (modular DDD entities: user/org/course/lesson/module/quiz/assignment/grade), TypeORM/Prisma for migration/schema
- **SCORM/xAPI/LTI External Integrations:** Ingest standard e-learning packages; SSO, payments (Stripe)
- **Search:** Elasticsearch (courses, lessons, user progress, content, discussions)
- **Cache:** Redis (session, pub/sub, hot stats)
- **File Storage:** S3/Minio (lessons, assignments, uploads, video)
- **Frontend:** Nx + React (SSR for landing/course SEO, role-specific experiences)
- **Authentication:** JWT/OAuth/SAML, RBAC at API
- **Infra/DevOps:** Docker Compose, Helm/k8s, AWS S3/PostgreSQL/OpenSearch, CI/CD with Github Actions
- **Monitoring:** Prometheus, Grafana, Sentry, winston/ELK logs
- **i18n:** i18next, date/time/currency localization
- **Security:** RBAC, audit, retention policies, GDPR, optional COPPA/FERPA modes
- **System Design Note:** Course logic/lesson ingestion, grading, notifications, and reporting engineered as domain features/modules, scalable across worker/API web services, supporting LTI/xAPI integration for school/enterprise use.

---

## 8. Document Collaboration Platform

- **Backend API:** Nx + NestJS + Apollo GraphQL (CRDT/OT for real-time, Yjs or Automerge for sync), REST for exports/imports/webhooks
- **Real-Time Engine:** WebSockets for editing sync, presence/typing, comment/notification/out-of-band signals
- **Worker/Queue:** BullMQ (notifications, export, audit archiving, batch search reindex, compliance jobs)
- **Database:** PostgreSQL (users, docs, folders, permissions, version/audit, metadata); S3 for large docs/revisions; hybrid “blob” meta for rich media
- **Search:** Elasticsearch (fulltext, tags, comments, versions)
- **Cache:** Redis (session, online presence, hot docs, pub/sub)
- **File Storage:** S3/Minio (media, attachments, backups)
- **Frontend:** Nx + React/Next.js (multi-user cursors/highlighting, markdown and WYSIWYG, real-time, highly accessible)
- **Authentication:** JWT/OAuth2/SAML, RBAC/ACL, password hash with Argon2/bcrypt
- **Infra/DevOps:** Docker Compose, EKS/k8s, AWS S3/OpenSearch, CloudFront CDN, CI/CD, Helm
- **Observability:** Sentry, Prometheus, Grafana, ELK logs
- **Internationalization:** i18next, right-to-left, timezone, currency localized
- **Concurrency/Scaling:** CRDT/OT synchronized state sharded per doc/session; real-time servers auto-scale
- **Security:** Org/team boundaries, granular permissions, legal hold, full audit log, RBAC on API and at DB, strict data isolation
- **System Design Note:** Real-time editing and presence relies on distributed CRDT/OT, sessions sharded for scaling; hybrid storage for blob/meta, per-org and per-user tenancy; workspace supports Connect/SSO and plugin APIs.

---

> **AWS Deployment/Scaling General Practices (for all):**
> - Use EKS/ECS for container orchestration, deploy services as auto-scaling pods and jobs
> - Store files in S3, user data in RDS (Postgres), cache in Elasticache (Redis), search in OpenSearch/Elasticsearch
> - CI/CD pipelines push containers to ECR, deploy via Helm/ArgoCD/Terraform
> - Observability via CloudWatch, custom Prometheus exporters, Sentry error tracking
> - Secrets managed via AWS Secrets Manager/Parameter Store, IAM roles for service access
> - CDN for frontend/assets (CloudFront)
> - Use Route 53, Certificate Manager for DNS/SSL

---

**Each of these platforms follows modern SaaS separation principles:**  
- Clear API vs UI vs worker/service split  
- Queues/async for scale  
- Dedicated external services for storage, search, and cache  
- Strong RBAC, security, and full operational pipeline  
- Designed from first deployment for cloud auto-scale and observability
