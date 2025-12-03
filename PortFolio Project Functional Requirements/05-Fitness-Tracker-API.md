# Project 5 of 8: Fitness Tracker API

## 1. Project Overview

### Project Name and Number
**Project 5 of 8: Fitness Tracker API**

### Executive Summary
A secure, extensible API-centric platform for individuals and fitness professionals to track workouts, log health data, set and analyze progress against goals, and connect with devices and external fitness/nutrition APIs. Built with Python and FastAPI, this project demonstrates async Python patterns, device integrations, health data handling, and HIPAA-ready architecture.

### Target Audience
- **Fitness Enthusiasts:** Individuals tracking personal workouts, health metrics, and goals
- **Personal Trainers/Coaches:** Managing multiple clients with workout assignments and progress monitoring
- **Gyms/Studios:** Providing members access to logging, challenges, and leaderboards
- **App Developers:** Leveraging robust APIs for custom health apps and wearable integrations
- **Healthcare & Research:** Optional modules for patient data and cohort analytics

### Key Value Propositions
1. **Full-Featured Workout Logging** – Track routines, sets, reps, weights, cardio, and more
2. **Goal-Centric Personalization** – Set and monitor goals for weight, reps, distance, calories, streaks
3. **Comprehensive Health Metrics** – Log body measurements, HR, BP, sleep, hydration
4. **Device Integrations** – Connect with Fitbit, Garmin, Apple Health, Google Fit
5. **Nutrition Tracking** – Food logging with macro/micro breakdown via external APIs
6. **Advanced Analytics** – Charts, PR tracking, trends, training load analysis
7. **Social Features** – Challenges, leaderboards, and shared progress (optional)

### Developer Learning Objectives
- Build high-performance async APIs with FastAPI and Python
- Implement OAuth2 flows for third-party device integrations
- Handle sensitive health data with HIPAA-ready patterns
- Design flexible schema for varied workout types
- Build recommendation algorithms for workout suggestions
- Master Python async patterns with asyncio and aiohttp

---

## 2. Detailed Functional Requirements

### 2.1 User Management & Authentication
- **Registration & Login**
  - Email/password with email verification
  - OAuth2 (Google, Facebook, Apple, Fitbit, Garmin)
  - Optional 2FA with authenticator apps
  - Session and device tracking

- **Profile Management**
  - Avatar upload (S3/CDN)
  - Health details: age, DOB, height, weight, gender
  - Notification preferences
  - Privacy controls (public/private profile)

- **Multi-User Support**
  - Trainer/gym accounts with client management
  - Role management: trainer, client, admin
  - Client assignment and progress access

- **Privacy & Compliance**
  - Data sharing controls
  - GDPR/HIPAA-compliant data export and deletion
  - Consent management

### 2.2 Core Business Logic (Workout Management)
- **Workout CRUD**
  - Create, clone, update, delete workouts
  - Templates and historical logs
  - Fields: name, description, tags, visibility, planned date/time
  - Coach-assigned or self-logged workouts

- **Exercise Management**
  - Exercise catalog (per org or public)
  - Fields: name, type, muscle group, equipment, video/media
  - Custom exercise creation

- **Session Logging**
  - Date/time, location (GPS), duration, effort rating
  - Per-set tracking: reps, weight, time, RPE, heart rate
  - Rest intervals, supersets, custom fields
  - Failure/completion status

- **Progression & Templates**
  - Weekly workout plans
  - Template versioning and sharing
  - Personal best (PR) tracking
  - Streak and goal progress visualization

### 2.3 Search & Discovery
- **Elasticsearch-Powered Search**
  - Full-text search on workouts, exercises, metrics
  - Fuzzy search with auto-suggest
  - Filter by goal type, frequency, muscle group, equipment

- **Exercise Library**
  - Video demonstrations
  - How-to instructions
  - Best practices per movement

### 2.4 Notifications & Communication
- **Reminder System**
  - Workout reminders
  - Goal achievement notifications
  - PR alerts ("PR possible on deadlift!")
  - Health trend alerts

- **Delivery Channels**
  - In-app notifications
  - Email notifications
  - SMS via Twilio
  - Push notifications

### 2.5 Analytics & Reporting
- **Goal Management**
  - Short/long-term goals: weight, reps, distance, calories, streaks
  - Inline metric visualization
  - Daily/weekly analytics dashboard
  - Personal best notifications

- **Trends & History**
  - Performance over time graphs
  - Progress toward goals
  - Injury/rehab tracking
  - Export: CSV, Excel

- **Custom Dashboards**
  - Widget-based dashboards
  - Favorite metrics selection

### 2.6 File Management
- **Media Uploads**
  - Progress photos and form check videos
  - S3/MinIO storage with presigned URLs
  - User quotas and validation
  - Preview in dashboard

### 2.7 External Integrations
- **Device APIs**
  - Fitbit, Garmin, Apple Health, Google Fit, Withings, Oura
  - OAuth token management with refresh
  - Real-time or scheduled sync
  - Steps, workouts, sleep, HR, calories

- **Nutrition APIs**
  - Spoonacular, USDA food database
  - Macro/micro breakdown
  - Food images and portion sizing
  - Saved/favorite meals

- **Error Handling**
  - Circuit breaker pattern
  - Re-auth flow for expired tokens
  - Device error status in UI
  - Historical sync logs

### 2.8 Accessibility & Internationalization
- **Responsive Design**
  - Keyboard navigation
  - Screen reader support
  - High contrast mode
  - Touch gestures for mobile

- **Localization**
  - Multi-language support
  - Unit preference (metric/imperial)
  - Locale-aware date/time

### 2.9 Security & Compliance
- **Authentication Security**
  - JWT (RS256) tokens
  - OAuth2 with device token handling
  - 2FA support
  - Session expiration and management

- **Data Protection**
  - Per-user, per-org data isolation
  - RBAC for coach/client access
  - Encryption at rest and in transit

- **File Security**
  - All files in S3 with AV scan
  - Expiring presigned URLs
  - Never publicly accessible

- **Compliance**
  - HIPAA-ready architecture
  - GDPR data export/delete
  - Full audit logging
  - Consent banners

---

## 3. Technical Stack Specification

```yaml
Backend:
  Runtime: Python 3.11+
  Framework: FastAPI 0.100+
  API_Style: REST (OpenAPI 3.0 auto-generated)
  ORM: SQLAlchemy 2.0 (async)
  Validation: Pydantic v2
  Documentation: FastAPI auto-docs (Swagger/ReDoc)
  Async: asyncio, aiohttp, httpx

Frontend:
  Framework: Next.js 14
  State_Management: React Query / Zustand
  Styling: TailwindCSS 3.x
  Charts: Chart.js / Recharts
  Forms: React Hook Form + Zod

Databases:
  Primary_SQL: PostgreSQL 15
  Document_Store: MongoDB 7.0 (device logs)
  Search_Engine: Elasticsearch 8.x
  Cache: Redis 7.x

Message_Queue:
  Queue: Celery (Redis broker) or ARQ
  Scheduler: Celery Beat / APScheduler

File_Storage:
  Development: MinIO
  Production: AWS S3 + CloudFront CDN

Authentication:
  Strategy: FastAPI OAuth2
  Tokens: JWT (RS256) via python-jose
  OAuth: Google, Facebook, Apple, Fitbit, Garmin

Infrastructure:
  Containerization: Docker + Docker Compose
  Orchestration: Kubernetes (Helm Charts)
  CI_CD: GitHub Actions
  IaC: Terraform

AWS_Services:
  Compute: ECS Fargate
  Database: RDS (Postgres)
  Search: OpenSearch Service
  Cache: ElastiCache (Redis)
  Storage: S3
  Secrets: AWS Secrets Manager
  Scheduler: Lambda + EventBridge
  Monitoring: CloudWatch

Monitoring_Observability:
  Metrics: Prometheus + Grafana
  Logging: structlog → ELK Stack
  Error_Tracking: Sentry
  APM: AWS X-Ray
```

---

## 4. Database Schema Design

### Entity Relationship Diagram (PostgreSQL)

```mermaid
erDiagram
    USERS ||--o{ WORKOUTS : logs
    USERS ||--o{ GOALS : sets
    USERS ||--o{ BODY_METRICS : records
    USERS ||--o{ DEVICE_CONNECTIONS : has
    USERS ||--o{ NUTRITION_LOGS : tracks
    
    TRAINERS ||--o{ TRAINER_CLIENTS : manages
    USERS ||--o{ TRAINER_CLIENTS : assigned_to
    
    WORKOUTS ||--o{ WORKOUT_EXERCISES : contains
    WORKOUTS ||--o{ WORKOUT_SETS : includes
    
    EXERCISES ||--o{ WORKOUT_EXERCISES : used_in
    EXERCISES ||--o{ EXERCISE_MEDIA : has
    
    USERS {
        uuid id PK
        string email UK
        string password_hash
        string display_name
        string avatar_url
        date birth_date
        enum gender
        float height_cm
        float weight_kg
        string timezone
        jsonb preferences
        boolean is_trainer
        timestamp created_at
        timestamp updated_at
    }
    
    TRAINERS {
        uuid id PK
        uuid user_id FK UK
        string business_name
        text bio
        jsonb certifications
        boolean is_verified
    }
    
    TRAINER_CLIENTS {
        uuid id PK
        uuid trainer_id FK
        uuid client_id FK
        enum status
        timestamp assigned_at
    }
    
    EXERCISES {
        uuid id PK
        string name
        enum exercise_type
        jsonb muscle_groups
        jsonb equipment
        text instructions
        string video_url
        boolean is_public
        uuid created_by FK
        timestamp created_at
    }
    
    EXERCISE_MEDIA {
        uuid id PK
        uuid exercise_id FK
        string s3_key
        enum media_type
        integer sort_order
    }
    
    WORKOUTS {
        uuid id PK
        uuid user_id FK
        uuid template_id FK
        string name
        text notes
        enum status
        timestamp scheduled_for
        timestamp started_at
        timestamp completed_at
        integer duration_minutes
        integer calories_burned
        float perceived_exertion
        jsonb location
        timestamp created_at
    }
    
    WORKOUT_TEMPLATES {
        uuid id PK
        uuid created_by FK
        string name
        text description
        jsonb structure
        boolean is_public
        integer times_used
        timestamp created_at
    }
    
    WORKOUT_EXERCISES {
        uuid id PK
        uuid workout_id FK
        uuid exercise_id FK
        integer sort_order
        text notes
    }
    
    WORKOUT_SETS {
        uuid id PK
        uuid workout_exercise_id FK
        integer set_number
        integer reps
        float weight_kg
        integer duration_seconds
        float distance_meters
        integer rpe
        integer heart_rate
        boolean is_warmup
        boolean is_failure
        timestamp logged_at
    }
    
    GOALS {
        uuid id PK
        uuid user_id FK
        enum goal_type
        string name
        float target_value
        string unit
        float current_value
        date start_date
        date target_date
        enum status
        timestamp created_at
        timestamp updated_at
    }
    
    BODY_METRICS {
        uuid id PK
        uuid user_id FK
        enum metric_type
        float value
        string unit
        text notes
        timestamp recorded_at
    }
    
    PERSONAL_RECORDS {
        uuid id PK
        uuid user_id FK
        uuid exercise_id FK
        enum record_type
        float value
        string unit
        uuid workout_id FK
        timestamp achieved_at
    }
    
    DEVICE_CONNECTIONS {
        uuid id PK
        uuid user_id FK
        enum provider
        string provider_user_id
        text access_token_encrypted
        text refresh_token_encrypted
        timestamp token_expires_at
        jsonb sync_settings
        timestamp last_sync_at
        enum sync_status
        timestamp created_at
    }
    
    NUTRITION_LOGS {
        uuid id PK
        uuid user_id FK
        enum meal_type
        string food_name
        float serving_size
        string serving_unit
        integer calories
        float protein_g
        float carbs_g
        float fat_g
        jsonb micronutrients
        timestamp logged_at
    }
```

### MongoDB Collections (Device Data)

```javascript
// Raw Device Sync Data Collection
{
  _id: ObjectId,
  userId: ObjectId,
  provider: String, // "fitbit", "garmin", etc.
  dataType: String, // "steps", "sleep", "heart_rate"
  rawData: Object,  // Original API response
  processedData: Object,
  syncedAt: ISODate,
  dateRange: {
    start: ISODate,
    end: ISODate
  }
}

// Activity Stream Collection
{
  _id: ObjectId,
  userId: ObjectId,
  date: ISODate,
  activities: [
    {
      type: String, // "workout", "goal_achieved", "pr"
      data: Object,
      timestamp: ISODate
    }
  ]
}
```

---

## 5. Technical Architecture Diagram

```mermaid
graph TB
    subgraph "Client Layer"
        Web[React Web App]
        Mobile[Mobile App]
        Wearables[Wearable Devices]
    end

    subgraph "CDN & Edge"
        CF[CloudFront CDN]
        R53[Route 53]
    end

    subgraph "AWS VPC"
        subgraph "Public Subnets"
            ALB[Application Load Balancer]
        end

        subgraph "Private Subnets - Compute"
            subgraph "ECS Cluster"
                API[FastAPI Service<br/>Fargate Tasks]
                Worker[Celery Worker<br/>Fargate Tasks]
            end
            Lambda[Lambda<br/>Scheduled Sync]
        end

        subgraph "Private Subnets - Data"
            RDS[(RDS PostgreSQL)]
            DocDB[(DocumentDB<br/>Device Logs)]
            ElastiCache[(ElastiCache Redis<br/>Cache + Queue)]
            OpenSearch[(OpenSearch)]
        end
    end

    subgraph "AWS Storage"
        S3[(S3 Bucket<br/>Media Files)]
    end

    subgraph "External APIs"
        Fitbit[Fitbit API]
        Garmin[Garmin API]
        Apple[Apple Health]
        Spoon[Spoonacular API]
    end

    Web --> CF
    Mobile --> CF
    CF --> ALB
    ALB --> API
    
    Wearables --> Fitbit
    Wearables --> Garmin
    Wearables --> Apple
    
    API --> RDS
    API --> DocDB
    API --> ElastiCache
    API --> OpenSearch
    API --> S3
    
    Worker --> RDS
    Worker --> ElastiCache
    Worker --> Fitbit
    Worker --> Garmin
    Worker --> Spoon
    
    Lambda --> Worker
```

---

## 6. AWS Deployment Architecture

### Compute Strategy
- **ECS Fargate** for API and Celery worker services
- **Lambda** for scheduled device sync triggers
- Auto-scaling based on API request volume
- Separate task definitions for API and workers

### Database Strategy
- **RDS PostgreSQL** for structured workout and user data
- **DocumentDB** for raw device sync data and activity streams
- **ElastiCache Redis** for caching, sessions, and Celery broker
- **OpenSearch** for exercise and workout search

### Device Sync Architecture
- **EventBridge** schedules sync jobs per user preferences
- **Lambda** triggers Celery tasks for device sync
- Circuit breaker pattern for API failures
- Token refresh handling with encrypted storage

### Storage Strategy
- **S3** for progress photos and exercise videos
- **CloudFront** for global content delivery
- Presigned URLs with expiration
- Lambda@Edge for image resizing

### CI/CD Pipeline
```yaml
Pipeline:
  1. Push to GitHub → Trigger Actions
  2. Run pytest (unit + integration)
  3. Type checking (mypy)
  4. Linting (ruff)
  5. Build Docker Images
  6. Push to ECR
  7. Deploy to Staging
  8. Integration Tests with Device APIs
  9. Deploy to Production
```

---

## 7. AI/ML Feature Specification

### Use Case: Workout Recommendations & PR Predictions

#### Problem Statement
Users want personalized workout suggestions based on their history, goals, and recovery status. Additionally, the system should predict when users are likely to achieve personal records.

#### Model Architecture
1. **Workout Recommendation Model**
   - Type: Collaborative filtering + Content-based hybrid
   - Input: User workout history, goals, preferences, similar users' patterns
   - Output: Recommended exercises, sets, reps, weight progressions

2. **PR Prediction Model**
   - Type: Time-series regression
   - Input: Historical performance data, training load, recovery metrics
   - Output: Predicted 1RM and PR probability per exercise

#### Architecture Integration
```mermaid
graph LR
    subgraph "FastAPI Service"
        API[API Endpoints]
        Rec[Recommendation<br/>Service]
    end
    
    subgraph "ML Service"
        Model[scikit-learn/<br/>PyTorch Model]
        Cache[Redis<br/>Model Cache]
    end
    
    subgraph "Data Pipeline"
        ETL[Data Pipeline]
        Features[Feature Store]
    end
    
    API --> Rec
    Rec --> Cache
    Cache --> Model
    ETL --> Features
    Features --> Model
```

#### Data Flow
1. **Training Pipeline**
   - Scheduled job extracts workout history
   - Feature engineering: volume, frequency, progression rate
   - Model training with cross-validation
   - Model versioning and A/B testing

2. **Inference Flow**
   - User requests workout recommendation
   - API calls recommendation service
   - Model generates personalized suggestions
   - Results cached for performance

---

## 8. API Design (FastAPI)

```python
# Router structure
from fastapi import APIRouter, Depends, HTTPException
from typing import List

router = APIRouter(prefix="/api/v1", tags=["workouts"])

# Workout endpoints
@router.post("/workouts", response_model=WorkoutResponse)
async def create_workout(
    workout: WorkoutCreate,
    current_user: User = Depends(get_current_user),
    db: AsyncSession = Depends(get_db)
):
    """Create a new workout session."""
    pass

@router.get("/workouts", response_model=List[WorkoutSummary])
async def list_workouts(
    start_date: date = None,
    end_date: date = None,
    status: WorkoutStatus = None,
    skip: int = 0,
    limit: int = 20,
    current_user: User = Depends(get_current_user),
    db: AsyncSession = Depends(get_db)
):
    """List user's workouts with filtering."""
    pass

@router.post("/workouts/{workout_id}/sets", response_model=SetResponse)
async def log_set(
    workout_id: UUID,
    set_data: SetCreate,
    current_user: User = Depends(get_current_user),
    db: AsyncSession = Depends(get_db)
):
    """Log a set during a workout."""
    pass

# Device sync endpoints
@router.post("/devices/connect/{provider}")
async def initiate_device_connection(
    provider: DeviceProvider,
    current_user: User = Depends(get_current_user)
):
    """Start OAuth flow for device connection."""
    pass

@router.post("/devices/{provider}/sync")
async def trigger_device_sync(
    provider: DeviceProvider,
    current_user: User = Depends(get_current_user),
    task_queue: Celery = Depends(get_celery)
):
    """Trigger manual sync for a device."""
    pass

# Analytics endpoints
@router.get("/analytics/progress", response_model=ProgressAnalytics)
async def get_progress_analytics(
    exercise_id: UUID = None,
    period: AnalyticsPeriod = AnalyticsPeriod.MONTH,
    current_user: User = Depends(get_current_user),
    db: AsyncSession = Depends(get_db)
):
    """Get progress analytics for exercises."""
    pass
```

---

## 9. Monorepo Structure

```
fitness-tracker-api/
├── apps/
│   ├── api/                    # FastAPI Application
│   │   ├── src/
│   │   │   ├── api/
│   │   │   │   ├── v1/
│   │   │   │   │   ├── endpoints/
│   │   │   │   │   │   ├── auth.py
│   │   │   │   │   │   ├── users.py
│   │   │   │   │   │   ├── workouts.py
│   │   │   │   │   │   ├── exercises.py
│   │   │   │   │   │   ├── goals.py
│   │   │   │   │   │   ├── metrics.py
│   │   │   │   │   │   ├── devices.py
│   │   │   │   │   │   └── nutrition.py
│   │   │   │   │   └── router.py
│   │   │   │   └── deps.py
│   │   │   ├── core/
│   │   │   │   ├── config.py
│   │   │   │   ├── security.py
│   │   │   │   └── exceptions.py
│   │   │   ├── models/
│   │   │   ├── schemas/
│   │   │   ├── services/
│   │   │   │   ├── workout_service.py
│   │   │   │   ├── device_sync_service.py
│   │   │   │   └── recommendation_service.py
│   │   │   └── main.py
│   │   └── tests/
│   ├── worker/                 # Celery Worker
│   │   └── src/
│   │       ├── tasks/
│   │       │   ├── device_sync.py
│   │       │   ├── notifications.py
│   │       │   └── analytics.py
│   │       └── celery_app.py
│   └── web/                    # React Frontend
│       └── src/
├── libs/
│   ├── db/
│   │   ├── models/
│   │   ├── migrations/
│   │   └── repositories/
│   ├── device_integrations/
│   │   ├── fitbit/
│   │   ├── garmin/
│   │   └── apple_health/
│   └── ml/
│       ├── recommendation/
│       └── pr_prediction/
├── infrastructure/
│   ├── terraform/
│   └── helm/
├── docker-compose.yml
├── pyproject.toml
└── requirements.txt
```

---

## 10. Success Criteria

1. **API Performance**: <100ms response time for 95th percentile requests
2. **Device Sync Reliability**: 99% successful sync rate with proper error handling
3. **Data Accuracy**: Zero data loss during device synchronization
4. **Search Quality**: Relevant exercise search results with typo tolerance
5. **Compliance**: HIPAA-ready architecture with full audit logging
6. **Test Coverage**: >90% code coverage with pytest
7. **Scalability**: Handle 10,000+ concurrent users with auto-scaling

---

*Last Updated: December 2024*
