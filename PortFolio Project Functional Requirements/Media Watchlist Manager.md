# Movie/Media Watchlist Manager – Requirements

---

## 1. Product Vision

A modern, extensible movie and TV show cataloging platform enabling users to track what they watch, maintain personal and shared watchlists, review and rate titles, discover recommendations, sync with third-party content APIs, and socialize with friends. Designed for individuals, groups, and film enthusiasts—API- and integration-ready, feature-rich, reliable, and secure.

---

## 2. Target Audience

- **Individuals:** Users keeping track of movies/shows they’ve watched, want to watch, or own; personal notes, reviews, and ratings a priority.
- **Film/TV Enthusiast Communities:** Movie clubs or viewing groups who want shared lists, group ratings, and comments.
- **Reviewers/Bloggers:** Those writing or sharing public reviews/ratings, requiring discoverability, rich metadata, and export options.
- **Families:** Maintaining shared “to watch” and “watched” lists, tracking multiple profiles in an account.
- **App Developers:** Integrating with public API for watchlist management within other media apps.

---

## 3. Key Value Propositions

- **Unified catalog:** One place to manage all watched, owned, and planned-to-watch media.
- **Metadata-rich discovery:** Title, year, cast, genres, synopsis, streaming availability, trailers, posters, fetched from APIs like TMDb/OMDb/JustWatch.
- **Personalization:** Custom lists/tags, user notes, personal ratings, and episode tracking.
- **Socialization:** Follow friends, compare lists, see trends, recommend content.
- **Powerful search and analytics:** Full-text/fuzzy search (title, actor, genre, etc.), filtering, stats on watching habits, genre breakdown, and streaks.
- **Seamless integration:** API for clients, browser extensions, smart TVs, and export/import with other watchlist apps.

---

## 4. Complete Functional Requirements

### 4.1 User Management

- **Authentication:**  
  Signup via email/password, OAuth (Google, Facebook, Apple, optional Twitter/TMDb), email verification, password reset; optional 2FA.
- **Profile:**  
  Avatar upload (S3/CDN), display name, bio, location, social links, timezone, privacy settings (public/private/friends-only lists and ratings).
- **Multi-profile support:**  
  Single account can host multiple profiles (e.g., family, kid, guest mode); switch and manage profiles.
- **GDPR/data export and delete:**  
  Full self-service data download and removal.

---

### 4.2 Watchlist, Lists & Media Library

- **CRUD Watchlist/Lists:**  
  Users can create multiple lists (e.g., “Favorites”, “To Watch”, “TV Binge 2025”, “Anime”); set as main/current or custom name/tags/icon/emoji.
  Add/remove/reorder/repeat items in any list (by drag/drop or multi-select).
- **Library Views:**  
  All “owned” and “watched” titles by user, group, or family; filter/sort by any metadata.  
  Mark owned (Blu-ray, streaming, VHS) or previously owned for collectors.

---

### 4.3 Media Cataloging & Enrichment

- **Adding Titles:**  
  Search TMDb/OMDb/JustWatch APIs (with circuit breaker, retry on error) or manual entry form. Fetch title, description, year, cast, poster, streaming.
  Batch import from file (CSV, exported list, IMDB/TMDb), mapping fields (dedupe).
  Add full TV shows or custom seasons/episodes; track progress for each (including restart/multi-watch).
- **Metadata:**  
  Per title: director(s), cast, genres, language(s), runtime, trailer/videos, streaming sources, synopses, posters and artwork, ratings (MPAA etc.).

---

### 4.4 Reviewing, Rating, Social

- **Ratings:**  
  Per title or episode, numeric/emoji/star scale; quick or detailed review.
  Edit, delete, or undo last action. Optionally anonymous or public/topic reviews.
  Aggregate ratings shown for friends/all users.
- **Reviews:**  
  Markdown or WYSIWYG, tag topics/spoilers, comments on reviews (threaded), like/dislike.
- **Reactions:**  
  Emoji or quick-reaction as supplement to reviews/ratings.
  Pin/favorite a review to a title, flag inappropriate, admin moderation.
- **Social:**  
  Friend/follow systems, activity feed for friends (e.g., “Alice watched The Godfather”, “Bob recommended Interstellar”).
  DM or comment on friends' lists; notification settings for new recommendations/comments.

---

### 4.5 Discovery, Search & Recommendations

- **Search:**  
  Full Elasticsearch-based search for any title, actor, director, tag, year, language, streaming provider.
  Faceted filter interface (genre, year, watched, owned, list, tag, rating range).
  Autocomplete in all search boxes.
- **Trending, Recommendation, & Analytics:**  
  Highlight trending titles (by user, friends, global), new releases, recommendations based on lists/ratings/play history, daily/weekly highlights.
  Analytics dashboard: viewing streaks, favorite genre breakdown, director/actor stats, top “unwatched” referred titles, historical data.
  “Remind me to watch”, “rewatch”, “awaiting release” lists with reminders.
- **Calendar/Release Reminders:**  
  Subscribe to release calendars (per list/tag/franchise), receive emails/SMS for anticipated titles or episode drops.

---

### 4.6 Integrations & Exports

- **API Integrations:**  
  Scheduled TMDb/OMDb sync for library enrichment.
  JustWatch for streaming links (where-to-watch), with robust fallback, error handling, and cache.
- **Browser extension/mobile-ready API:**  
  Endpoints for quick-add, mark-watched from web/mobile/extension.
- **Export/Import:**  
  CSV/JSON export of all lists, ratings, reviews, notes, history; import from other watchlist apps or services.
- **Calendar integration:**  
  “Add to Google Calendar/O365/iCal” for release dates.
- **Webhook:**  
  Outbound for watched/added/removed/review event; incoming support for 3rd-party triggers.

---

### 4.7 Notifications & Settings

- **Notifications:**  
  Web, push, and email for friend activity, new title availability, reminders, comments/mentions, release/premiere dates.
  Per-user granularity (per list, recommendation, DM, etc.), snooze, DND hours, outbox for all sent notifications.

---

### 4.8 Accessibility, I18n, Compliance

- **Accessibility:**  
  Keyboard navigation, aria-labels, contrast, screen readers, responsive.
- **Internationalization:**  
  i18next-managed language resource strings, UI and email notifications. Date/time/number localized.
- **Privacy:**  
  Visibility options per list/review/profile, opt-out of analytics, consent for 3rd-party integrations, data deletion/export, audit all access.

---

## 5. Developer & Technical Requirements

### 5.1 Monorepo Structure

- **Nx workspace:**  
  - `apps/api` (NestJS + Apollo GraphQL, optionally REST for batch/legacy or file endpoints)
  - `apps/worker` (Nest/Node for background jobs: enrichment, batch sync, notifier, update chase, trending analytics)
  - `apps/web` (React/Next.js SSR for SEO, mobile-first)
  - `libs/types` (GraphQL/OpenAPI codegen)
  - `libs/db` (TypeORM/Prisma for PostgreSQL or hybrid PG+Mongo for list/notes flexibility)
  - `libs/utils`
  - `apps/devops` (infra, compose, scripts)
- **Strict Typescript everywhere; codegen updated on schema change.**

### 5.2 API, Data, Jobs

- **Apollo GraphQL API:**  
  - Queries for search, recommendations, tracking, list/library, account management; mutations for actions/logs.
  - Subscriptions for realtime (friend watched, comment, reminder events).
  - All resolvers strictly typed, tested, paginated, and complexity-guarded.
- **Files/Posters:**  
  - S3/Minio with presigned urls, resolutions for artwork, attachment quotas.
- **Elasticsearch:**  
  - Search on all major entities, scheduled trending/batch jobs, cache hot search results in Redis.
- **BullMQ/Redis:**  
  - Background jobs: batch external sync, trending/recommendations update, notification triggers, data integrity, scheduled reminders.

### 5.3 Integrations

- **TMDb/OMDb/JustWatch:**  
  - All keys managed via .env or secrets, rotated, strictly rate capped, auto-retry and fallback chains, cache last good results.
- **Browser/mobile extension:**  
  - Token-based, limited-scope public API for watch/mark/add actions.
- **Webhook engine:**  
  - HMAC signed outbound; user/admin UI for config; error/retry log with admin resolution; test endpoint.
- **Calendar export:**  
  - All “next release”/reminder endpoints generate dynamic .ics and “Add to Calendar” links per item/list.

### 5.4 DevOps, Security, Observability

- **Docker Compose includes:**  
  postgres, redis, s3/minio, elastic, api, worker, web (dev and local demo), full seeders.
- **CI:**  
  Github Actions for lint/test/typecheck; build/test/deploy steps for all apps/libs; Docker build/push for prod.
- **K8s/Helm manifests:**  
  prod deployment, secret injection, autoscale, health/readiness endpoints on all services.
- **Prometheus and Sentry:**  
  All core metrics/alerts logged to dashboards, error/warn/alert events with trace/sample context.
- **Audit logs:**  
  DB log for all critical actions (CRUD, integrations, reviews, access, data export), per-org/user.

### 5.5 Testing & Docs

- **Jest unit tests:** All modules, resolvers, helpers, adapters, mutations.
- **Integration tests:** End-to-end flows with test containers for all infra.
- **Cypress/Playwright E2E:** For user flows: registration, watchlist CRUD, social, review, search, external import.
- **Coverage:** Enforced >=90% in CI.
- **OpenAPI/GraphQL playground auto-docs**, README/architecture/infra docs, codebase setup and UX onboarding guides.

### 5.6 Extensibility, Accessibility, Versioning

- **Feature Flags:**  
  DB/env toggled, all usage logged/auditable, rollout/rollbacks logged with time, user, before/after.
- **A11y (WCAG 2.1 AA):** Keyboard, screenreader, high contrast, focus state; axe-core/CI audit all pushes.
- **i18n:**  
  Language files updated for core and new features.
- **DB Migration/Upgrades:**  
  All code/dbs versioned, semantic-release for schema/API, migration scripts in repo, dockerized upgrade steps.
  
---

## 6. Success Criteria

- Every user flow is covered by E2E and integration/unit tests; documented and demonstrable locally and in CI.
- All integrations degrade gracefully and alert admin on failure.
- Trends/recommendations and analytics update accurately and on schedule.
- Search and dashboard queries performant and cache-hot under realistic use.
- Onboarding time for new devs is <1 day, with docs, seed scripts, and “local up” reliability.
