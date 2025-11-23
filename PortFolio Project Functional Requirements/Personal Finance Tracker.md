# Personal Finance Tracker – Requirements

---

## 1. Product Vision

A secure, user-focused platform for individuals or families to manage income, expenses, budgets, investments, and financial goals. The system enables users to track transactions, analyze spending by category and period, connect external accounts, receive personalized insights, plan budgets, and export/visualize all financial data. Designed for extensibility (multi-currency, crypto, stocks, external APIs) and robust automation.

---

## 2. Target Audience

- **Individuals and families:** Wanting to budget, track, and analyze their finances and savings.
- **Small business owners/freelancers:** Managing personal + business income/expenses, invoicing, and taxes.
- **Power users:** Integrating with banking/finance APIs, importing data, monitoring investments, or tracking crypto/stocks.

---

## 3. Key Value Propositions

- **Comprehensive tracking:** Record and categorize all incomes, expenses, transfers, and investments.
- **Automated data import:** Connect to banks, brokers, and wallets for real-time updates; bulk import from CSV/Excel.
- **Budgeting:** Create monthly/weekly budgets by category; get alerts on overspending/variance.
- **Insights & analytics:** Dashboards, charts, and reports by category, merchant, time period, or user.
- **Secure, private, and compliant:** Data isolation, encryption, audit logs, GDPR tools, and fine-grained privacy controls.
- **Integrations:** Currency exchange rates, crypto price APIs, stock/ETF sync, bill-reminder emails, and webhook export.
- **Mobile-first and multi-device:** Responsive, accessible, and fast on any screen.

---

## 4. Complete Functional Requirements

### 4.1 User Management

- **Authentication:**  
  Email/password (strong requirements), OAuth (Google/MS, Plaid/TrueLayer for open banking).
  Email verification and 2FA (app/email/SMS) options.
  Session & device management, account lockout after repeated fails.
- **Profile:**  
  Name, picture/avatar, home country/currency, notification settings, data export/delete.
  Multiple users/profiles per account (family/partner feature).
- **Privacy:**  
  Account deletion, full data export, ability to anonymize certain transactions, and GDPR/CCPA compliance options.

---

### 4.2 Accounts & Connections

- **Account Types:**  
  Bank, credit card, cash, wallet, investment, loan, asset (customizable).
- **API Integrations:**  
  Add bank/investment/crypto/exchange accounts by OAuth (Plaid/TrueLayer/etc.), with real-time, daily, or manual sync.
  Robust error/retry/circuit breaker for API failures, user notification if API sync fails.
- **Manual Import:**  
  CSV/XLS import wizard with column mapping; error reporting for bad data and duplicate detection.
- **Account Management:**  
  Assign nickname, color/icon, group by type/usage, archive/inactivate, configure sync/refresh timing.

---

### 4.3 Transactions, Categories & Rules

- **CRUD for Transactions:**  
  Add/edit/delete; assign to account, date/time, amount, currency, payee/merchant, receipt (file/photo), tags/notes.
  Split transactions (e.g., one supermarket run covers groceries, household, pet).
  Recurring transactions (salary/direct debit/utility bills) with schedule and notification.
  Transfer handling (between user’s accounts, e.g., credit card payment does not double-count).
- **Categories & Rules:**  
  System and user-defined categories (nested, re-orderable).
  Auto-categorization rules (by merchant/regex/amount) for new or imported transactions.
- **Bulk edit:**  
  Batch update, categorize, split, or delete.

---

### 4.4 Budgeting & Planning

- **Budget Creation:**  
  Monthly/weekly/daily by category or overall; rollover, strict/soft limits.
- **Tracker:**  
  Visual real-time progress, alerts for overspend/near-limit, variance analysis.
- **Goal Management:**  
  Savings/investment goals, target with timeline, automatic update as transactions occur.
- **Budget Templates:**  
  Clone/copy/pull templates for quick reuse.

---

### 4.5 Insights, Reports, & Analytics

- **Dashboards:**  
  Cashflow/Net worth over time, category spending pie chart, monthly trend lines, “what changed” summary, compare with previous period.
  Drill-down to all transactions for any visual.
- **Reports:**  
  Filter by account, category, time, merchant, user, tag.  
  Export as CSV/XLS/PDF, with scheduled/batched delivery to email or external webhook.
- **Automated Insights:**  
  Unusual transactions flagged, duplicate/payment anomalies, predictions (“your rent is about to be due”), merchant review.
- **Notifications:**  
  Email/SMS/in-app alerts: large/suspicious transactions, goal/budget progress, periodic summary.

---

### 4.6 Investments, Crypto, and Currency

- **Holdings:**  
  Track investments (stocks, ETFs, crypto, bonds); record buy/sell, dividends, position, cash balance.
  Pull real-time prices from APIs (CoinGecko, Alpha Vantage, Yahoo Finance); cache and display value/profit/loss.
- **Multi-currency:**  
  All fields amounts stored as both native and user’s reporting currency, auto-converted via exchange rate APIs (periodic update, history stored for accurate P&L).
- **Related Insights:**  
  Sorting/ranking by investment return, allocation pie, risk/volatility flagging (advanced/optional).

---

### 4.7 File Handling

- **Transaction Attachments:**  
  Upload receipts, invoices, statements (image/PDF/CSV), stored securely in S3/Minio with reference to transaction.
  Virus scan and type validation on upload; quota management.
- **Bulk Uploads/Downloads:**  
  Import/export of receipts, batch transactions, zipped PDF download for archiving or tax/reporting.

---

### 4.8 Search, Filters, and Custom Views

- **Search:**  
  Fulltext and advanced filter of transactions, payees, notes, categories, and tags with autocomplete.
  Faceted filtering (amount range, account, merchant, date, category), save favorite queries as quick-access views.
- **Smart Views & Shortcuts:**  
  E.g., “All Amazon purchases over $100 in 2024”, “Unreconciled over last 90 days”, "Groceries above planned budget".

---

### 4.9 External Integration & Automation

- **APIs:**  
  Pull from Plaid/TrueLayer and push to external bill/budgeting/reminder SaaS via webhook.
  External price tracking APIs for currencies, stocks, and cryptos with scheduled updates.
  Webhooks for “on new expense”, “budget exceeded”, “goal reached”.
- **Bank Sync Automation:**  
  Sync triggers via cron/queue; retries, fallback, and user alert in event of failures or required MFA/reauth.
- **Calendar Integration:**  
  Push due dates/recurring bills to Google/O365 Calendar; reminders via iCal/email/SMS.

---

### 4.10 Accessibility, Internationalization & Compliance

- **UI:** WCAG 2.1 AA compliance; fully keyboard navigable, ARIA labels, color contrast, and responsive.
- **Localization:** All screens/messages/notices in translatable string files (i18next); currency/date/locale everywhere.
- **GDPR Tools:**  
  Data export, user-redaction, consent banner, full audit file for user-initiated deletions or requests.

---

## 5. Developer/Architectural Requirements

### 5.1 Monorepo and Structure

- **Nx workspace:**  
  `apps/api` (NestJS REST API preferred; can offer GraphQL for advanced dashboard/flexible report queries later),  
  `apps/worker` (Nest Queue/BullMQ jobs for integrations, batch),  
  `apps/web` (Nx React, Redux/React Query),  
  `libs/types` (OpenAPI/GraphQL schema codegen),  
  `libs/db` (TypeORM/Prisma entities, migrations),  
  `libs/utils`, `apps/devops` (infra/scripts), seeders.
- **Typescript strict everywhere**.  
- **All shared types** autogen’d from OpenAPI/Swagger or GraphQL schema.

---

### 5.2 Backend Design

- **NestJS API:**  
  RESTful design, composite resource paths (e.g., `/users/:uid/accounts/:aid/transactions/:tid`).  
  Authentication middleware (JWT/OAuth), session checks, and RBAC guards for multi-user/multi-profile.
  Controllers/services modular by feature and domain (users, accounts, tx, budget, goal, notification, integration).
- **TypeORM/Prisma Postgres (primary):**  
  Entities for accounts, transactions, categories, splits, budgets, users, integrations, files, audit logs.
  Rich schema (enums for type, status, etc.), soft delete as needed, all changes timestamped/attributed.
- **ElasticSearch for search:**  
  Write-side sync of all tx and documents; admin job for reindexing.

---

### 5.3 File, Caching, and Queue System

- **File handling:**  
  S3/Minio bucket structured per user/account/year for uploads; direct file/multipart upload endpoint; all links presigned, expire after X min; audit download.
- **Redis** for session/user cache, hot queries, pubsub for notification/event.
- **BullMQ jobs:**  
  Cron jobs for daily sync (by user schedule), external API polling, alert/reminder building / email/SMS, periodic backups and report sends.  
  Idempotent jobs; dead-letter with logging and admin alerts for errors or repeated failures.

---

### 5.4 Integrations & Security

- **External APIs:**  
  Rate limited, queue handled, and circuit breaker (no user lockout on API fail, but visible in status bar/warning in UI).
  All tokens/keys encrypted for each integration; automatic reauth on expiry, all integration errors/exceptions logged/audited.
- **Secure password hashing** (Argon2 or bcrypt 12+) and per-session token invalidation.
- **RBAC at API and DB query levels.**
- **Input sanitation and class-validator** for DTOs, number/date/fx validation, and country/currency checks.
- **Audit logging** for sensitive actions (profile edit, deletion, export, failed logins, API key use).
- **CORS, Helmet, and environment-based origin config.**
- **Secrets via env or secrets manager; rotated and never in codebase.**

---

### 5.5 DevOps, Observability & Testing

- **Docker Compose up:** Boots API, worker, web, redis, minio, Postgres, elastic; seed/dev scripts.
- **K8s/Helm manifests** for scalable deploy; secret injection, rolling updates, resource quotas.
- **Prometheus metrics endpoint, Sentry for tracing and error capture, winston logger to ELK.**
- **Tests:**  
  Jest unit + integration, Cypress e2e; full flows: sign-up, import, budget, attach, export, external sync, goal, alert.
  CI/CD pipeline via Github Actions: lint, build, type, all tests; docker images and staged deployment.

---

### 5.6 Frontend Requirements

- **React/TS SSR app** (nx/web):  
  Responsive, accessible, mobile-first.
- **All data and schema validation via shared types**.
- **Full dashboard:** SPA with boards for accounts, budgets, goals, transactions; drilldown, filter, sort, and chart components.
- **Visualization:**  
  D3/Chart.js for analytics, downloadable/savable as image or file.
- **Forms:**  
  File upload, drag-and-drop bulk import, rich select with autocomplete for categories/payees.
- **Notifications:**  
  Toastr/snackbar, badge counts, full user center for messages.
- **Dark/light mode, color-blind settings, and i18n**
- **Accessibility:** Keyboard nav, ARIA, forms validation UIs.
- **Dev:** All routes/pages as modules, hooks for data fetching, husky lint/prettier, axe-core testing for a11y.

---

### 5.7 Maintenance, Docs, and Extensibility

- **Docs:**  
  Auto-generated OpenAPI and ERD diagrams; onboarding guides, integration setup, code standards.
- **Changelog:**  
  Semantic-release, bot-updated.
- **Feature flags:**  
  DB/env toggled for experimental features and gradual rollout; usage and status are auditable.
- **Dependency audit:**  
  Weekly pipeline, NPM/OS CVE alerts.

---

## 6. Success Criteria

- Finances tracked, imported, categorized, and visualized for users with little/no manual effort.
- Data/import/export and integration flows robust under real-world data and service instability.
- Modern UI/UX across devices and accessibility modalities.
- All privacy, audit logging, and compliance flows implemented as documented.
- Developers can add new integrations, chart types, or reporting flows without system-wide refactor.
