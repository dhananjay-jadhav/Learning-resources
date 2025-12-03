# 🏗️ Shared Infrastructure Guide

<div align="center">

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)

**📚 Common Infrastructure Patterns & Best Practices for All 8 Projects**

</div>

---

## 📋 Overview

This document describes the shared infrastructure patterns, configurations, and deployment strategies common to all 8 portfolio projects. It serves as a central reference for consistent architecture decisions across the entire portfolio.

---

## 📑 Table of Contents

| Section | Description |
|---------|-------------|
| [1. Technical Stack](#technical-stack-specification) | Complete tech stack definition for Node.js and Python |
| [2. AWS Architecture](#aws-architecture) | Cloud architecture patterns and VPC design |
| [3. Database Patterns](#database-design-patterns) | PostgreSQL, MongoDB, Redis conventions |
| [4. Security](#authentication--security) | JWT, RBAC, and security headers |
| [5. Observability](#observability-stack) | Metrics, logging, and tracing |
| [6. CI/CD](#cicd-pipeline) | GitHub Actions workflows |
| [7. Local Development](#local-development) | Docker Compose setup |
| [8. Deployment](#deployment-playbook) | AWS deployment checklist |

---

## 🔧 Technical Stack Specification

### 📦 Complete Stack Definition

<table>
<tr>
<td width="50%">

#### Node.js Backend (Projects 1-4, 7-8)
```yaml
Node_Backend:
  Runtime: Node.js 20 LTS
  Frameworks:
    - NestJS 10.x (Projects 1, 2)
    - Express.js 4.x (Project 3)
    - Fastify 4.x (Project 4)
  API_Styles:
    - REST (OpenAPI 3.0)
    - GraphQL (Apollo Server 4.x)
    - WebSocket (Socket.io / ws)
  ORM:
    - TypeORM 0.3.x
    - Prisma 5.x
  Validation: class-validator, Zod
  Documentation: Swagger, GraphQL Playground
```

</td>
<td width="50%">

#### Python Backend (Projects 5-6, 7-8 AI)
```yaml
Python_Backend:
  Runtime: Python 3.11+
  Frameworks:
    - FastAPI 0.100+ (Project 5, AI)
    - Django 4.2 LTS (Project 6)
  API_Styles:
    - REST (OpenAPI via FastAPI/DRF)
    - gRPC (AI service communication)
  ORM:
    - SQLAlchemy 2.0 (FastAPI)
    - Django ORM (Django)
  Validation: Pydantic v2, Django Forms
  Documentation: FastAPI auto-docs
```

</td>
</tr>
</table>

### 🎨 Frontend Stack
```yaml
Frontend:
  Framework: React 18 / Next.js 14
  State_Management: Redux Toolkit / React Query / Zustand
  Styling: TailwindCSS 3.x
  Forms: React Hook Form + Zod
  GraphQL_Client: Apollo Client 3.x
  Build: Vite / Next.js
```

### 💾 Data Layer
```yaml
Databases:
  Primary_SQL: PostgreSQL 15
  Document_Store: MongoDB 7.0
  Search_Engine: Elasticsearch 8.x
  Cache: Redis 7.x
  Time_Series: TimescaleDB (optional)

Messaging:
  Task_Queue: BullMQ (Redis-backed)
  Event_Streaming: Apache Kafka (Projects 7-8)
  Pub_Sub: Redis Pub/Sub
```

### ☁️ Infrastructure & Cloud
```yaml
Infrastructure:
  Containerization: Docker + Docker Compose
  Orchestration: Kubernetes 1.28+ (Helm Charts)
  CI_CD: GitHub Actions
  IaC: Terraform 1.6+
  Service_Mesh: Istio (optional)

AWS_Services:
  Compute: ECS Fargate / EKS
  Database:
    SQL: RDS PostgreSQL
    NoSQL: DocumentDB (MongoDB-compatible)
    Search: OpenSearch Service
  Cache: ElastiCache (Redis)
  Storage: S3 (with Glacier for archives)
  CDN: CloudFront
  Secrets: Secrets Manager
  DNS: Route 53
  SSL: Certificate Manager
  WAF: AWS WAF
  Monitoring: CloudWatch, X-Ray
```

---

## ☁️ AWS Architecture

### 🏗️ High-Level Architecture Diagram

```mermaid
graph TB
    subgraph "🌐 Users"
        Web[🖥️ Web Browser]
        Mobile[📱 Mobile App]
        API_Client[🔌 API Clients]
    end

    subgraph "🌍 AWS Edge Services"
        R53[📍 Route 53 DNS]
        CF[🚀 CloudFront CDN]
        WAF[🛡️ AWS WAF]
    end

    subgraph "🔒 AWS VPC"
        subgraph "🔓 Public Subnets"
            ALB[⚖️ Application Load Balancer]
            NAT[🌐 NAT Gateway]
        end

        subgraph "💻 Private Subnets - Compute"
            subgraph "📦 ECS Cluster"
                API[🚀 API Service<br/>Fargate Tasks]
                Worker[⚙️ Worker Service<br/>Fargate Tasks]
                AI[🧠 AI Service<br/>Fargate Tasks]
            end
        end

        subgraph "💾 Private Subnets - Data"
            RDS[(🐘 RDS PostgreSQL<br/>Multi-AZ)]
            DocDB[(🍃 DocumentDB<br/>MongoDB)]
            ElastiCache[(⚡ ElastiCache<br/>Redis Cluster)]
            OpenSearch[(🔍 OpenSearch<br/>Domain)]
        end

        subgraph "📨 Private Subnets - Messaging"
            MSK[📬 Amazon MSK<br/>Kafka]
        end
    end

    subgraph "📁 AWS Storage"
        S3[(🪣 S3 Buckets<br/>Files & Assets)]
        S3_Backup[(💾 S3 Backup<br/>DB Snapshots)]
    end

    subgraph "🔐 AWS Security"
        SM[🔑 Secrets Manager]
        KMS[🔐 KMS<br/>Encryption Keys]
        IAM[👥 IAM Roles<br/>& Policies]
    end

    subgraph "📊 AWS Observability"
        CW[📈 CloudWatch<br/>Logs & Metrics]
        XRay[🔬 X-Ray<br/>Tracing]
    end

    Web --> R53
    Mobile --> R53
    API_Client --> R53
    R53 --> CF
    CF --> WAF
    WAF --> ALB
    ALB --> API
    
    API --> RDS
    API --> DocDB
    API --> ElastiCache
    API --> OpenSearch
    API --> S3
    API --> MSK
    
    Worker --> RDS
    Worker --> ElastiCache
    Worker --> MSK
    Worker --> S3
    
    AI --> RDS
    AI --> MSK
    
    SM --> API
    SM --> Worker
    SM --> AI
    
    API --> CW
    Worker --> CW
    AI --> CW
```

### 🌐 VPC Network Design

```mermaid
graph TB
    subgraph "🌐 VPC: 10.0.0.0/16"
        subgraph "🔓 Public Subnets"
            PS1[AZ-A: 10.0.1.0/24<br/>ALB, NAT]
            PS2[AZ-B: 10.0.2.0/24<br/>ALB, NAT]
            PS3[AZ-C: 10.0.3.0/24<br/>ALB, NAT]
        end
        
        subgraph "💻 Private - Compute"
            PC1[AZ-A: 10.0.10.0/24<br/>ECS Tasks]
            PC2[AZ-B: 10.0.11.0/24<br/>ECS Tasks]
            PC3[AZ-C: 10.0.12.0/24<br/>ECS Tasks]
        end
        
        subgraph "💾 Private - Data"
            PD1[AZ-A: 10.0.20.0/24<br/>RDS, Redis]
            PD2[AZ-B: 10.0.21.0/24<br/>RDS, Redis]
            PD3[AZ-C: 10.0.22.0/24<br/>OpenSearch]
        end
    end
```

| Subnet Type | CIDR Range | Resources |
|-------------|------------|-----------|
| **Public Subnets** | 10.0.1.0/24 - 10.0.3.0/24 | ALB, NAT Gateway, Bastion |
| **Private Compute** | 10.0.10.0/24 - 10.0.12.0/24 | ECS Fargate, EKS Nodes |
| **Private Data** | 10.0.20.0/24 - 10.0.22.0/24 | RDS, DocumentDB, ElastiCache, OpenSearch |
| **Private Messaging** | 10.0.30.0/24 - 10.0.31.0/24 | Amazon MSK (Kafka) |

### 🔐 Security Groups

| Security Group | Inbound | Outbound | Purpose |
|----------------|---------|----------|---------|
| `alb-sg` | 443 from 0.0.0.0/0 | VPC CIDR | Internet-facing load balancer |
| `api-sg` | 3000-4000 from ALB | VPC + 443 external | API containers |
| `worker-sg` | None | VPC + 443 external | Background workers |
| `rds-sg` | 5432 from api-sg, worker-sg | None | PostgreSQL database |
| `redis-sg` | 6379 from api-sg, worker-sg | None | Redis cache |
| `opensearch-sg` | 443 from api-sg, worker-sg | None | Search engine |

---

## 💾 Database Design Patterns

### 🐘 PostgreSQL Schema Conventions

Every table should follow these standards:

```sql
-- ✅ Standard columns for all tables
CREATE TABLE example_table (
    -- Primary key
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    
    -- Timestamps
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    deleted_at TIMESTAMPTZ NULL, -- Soft delete
    
    -- Optimistic locking
    version INTEGER NOT NULL DEFAULT 1,
    
    -- Audit fields
    created_by UUID REFERENCES users(id),
    updated_by UUID REFERENCES users(id)
);

-- ✅ Standard indexes
CREATE INDEX idx_example_created_at ON example_table(created_at);
CREATE INDEX idx_example_deleted_at ON example_table(deleted_at) WHERE deleted_at IS NULL;

-- ✅ Auto-update timestamp trigger
CREATE OR REPLACE FUNCTION update_updated_at()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    NEW.version = OLD.version + 1;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trigger_update_updated_at
    BEFORE UPDATE ON example_table
    FOR EACH ROW
    EXECUTE FUNCTION update_updated_at();
```

### 🍃 MongoDB Collection Patterns

```javascript
// ✅ Standard document structure
{
  _id: ObjectId,
  
  // Timestamps
  createdAt: ISODate,
  updatedAt: ISODate,
  deletedAt: null | ISODate,
  
  // Audit trail
  auditLog: [
    {
      action: "created" | "updated" | "deleted",
      timestamp: ISODate,
      userId: ObjectId,
      changes: { field: { from: any, to: any } }
    }
  ],
  
  // Multi-tenancy
  organizationId: ObjectId,
  
  // Schema version for migrations
  schemaVersion: Number
}

// ✅ Standard indexes
db.collection.createIndex({ organizationId: 1, createdAt: -1 });
db.collection.createIndex({ deletedAt: 1 }, { sparse: true });
```

### ⚡ Redis Key Patterns

| Pattern | Example | TTL | Use Case |
|---------|---------|-----|----------|
| `session:{userId}:{sessionId}` | `session:123:abc` | 7 days | User sessions |
| `cache:{entity}:{id}` | `cache:user:123` | 5-15 min | Entity caching |
| `ratelimit:{ip}:{endpoint}` | `ratelimit:1.2.3.4:/api/login` | 1 min | Rate limiting |
| `lock:{resource}:{id}` | `lock:order:456` | 30 sec | Distributed locks |
| `notifications:{userId}` | `notifications:123` | N/A | Pub/Sub channel |
| `updates:{orgId}:{resourceType}` | `updates:org123:tasks` | N/A | Pub/Sub channel |

---

## 🔐 Authentication & Security

### 🎫 JWT Token Structure

```mermaid
graph LR
    subgraph "Access Token (15 min)"
        AT[JWT RS256]
        AT --> Claims1[sub: user_id]
        AT --> Claims2[org: organization_id]
        AT --> Claims3[roles: admin, user]
        AT --> Claims4[permissions: ...]
    end
    
    subgraph "Refresh Token (7 days)"
        RT[JWT RS256]
        RT --> RClaims1[sub: user_id]
        RT --> RClaims2[jti: token_id]
        RT --> RClaims3[family: family_id]
    end
```

| Token Type | Algorithm | TTL | Storage | Purpose |
|------------|-----------|-----|---------|---------|
| Access Token | RS256 | 15 min | Client memory | API authentication |
| Refresh Token | RS256 | 7 days | Redis + HttpOnly cookie | Token renewal |
| API Key | HMAC | None | Database | Service-to-service |

### 👥 RBAC Implementation

```mermaid
graph TD
    SA[SuperAdmin] --> OA[OrgAdmin]
    OA --> M[Manager]
    M --> MB[Member]
    MB --> G[Guest]
    
    SA -.-> |"*:*"| ALL[All Permissions]
    OA -.-> |"org:*, users:*"| ORG[Org Permissions]
    M -.-> |"projects:*, tasks:*"| PROJ[Project Permissions]
    MB -.-> |"tasks:write:own"| OWN[Own Resources]
    G -.-> |"read:public"| PUB[Public Only]
```

| Role | Permissions | Scope |
|------|-------------|-------|
| **SuperAdmin** | `*:*` | All resources, all actions |
| **OrgAdmin** | `org:*`, `users:*`, `settings:*` | Organization-wide |
| **Manager** | `projects:*`, `tasks:*`, `users:read` | Projects and tasks |
| **Member** | `projects:read`, `tasks:read`, `tasks:write:own` | Own resources |
| **Guest** | `projects:read:public`, `tasks:read:assigned` | Assigned only |

### 🛡️ Security Headers

```yaml
Headers:
  Strict-Transport-Security: max-age=31536000; includeSubDomains
  X-Content-Type-Options: nosniff
  X-Frame-Options: DENY
  X-XSS-Protection: 1; mode=block
  Content-Security-Policy: |
    default-src 'self';
    script-src 'self' 'unsafe-inline';
    style-src 'self' 'unsafe-inline';
    img-src 'self' data: https:;
    font-src 'self';
    connect-src 'self' https://api.*;
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: geolocation=(), microphone=(), camera=()
```

---

## Observability Stack

### Prometheus Metrics

```yaml
Standard_Metrics:
  HTTP:
    - http_requests_total{method, path, status}
    - http_request_duration_seconds{method, path}
    - http_request_size_bytes{method, path}
    - http_response_size_bytes{method, path}
    
  Database:
    - db_query_duration_seconds{query_type}
    - db_connections_active
    - db_connections_idle
    
  Cache:
    - cache_hits_total{cache_name}
    - cache_misses_total{cache_name}
    - cache_operation_duration_seconds{operation}
    
  Queue:
    - queue_jobs_total{queue, status}
    - queue_job_duration_seconds{queue}
    - queue_depth{queue}
    
  Business:
    - users_active_total
    - operations_total{operation_type}
```

### Logging Structure

```json
{
  "timestamp": "2024-01-15T10:30:00.000Z",
  "level": "info",
  "service": "api",
  "version": "1.0.0",
  "environment": "production",
  
  "trace_id": "abc123",
  "span_id": "def456",
  "parent_span_id": "ghi789",
  
  "request": {
    "id": "req-uuid",
    "method": "POST",
    "path": "/api/v1/tasks",
    "user_id": "user-uuid",
    "org_id": "org-uuid",
    "ip": "1.2.3.4",
    "user_agent": "..."
  },
  
  "message": "Task created successfully",
  "data": {
    "task_id": "task-uuid"
  },
  
  "duration_ms": 125
}
```

### Alerting Rules

```yaml
Alerts:
  HighErrorRate:
    Expression: rate(http_requests_total{status=~"5.."}[5m]) > 0.05
    Severity: critical
    
  HighLatency:
    Expression: histogram_quantile(0.95, http_request_duration_seconds) > 2
    Severity: warning
    
  DatabaseConnections:
    Expression: db_connections_active > 80
    Severity: warning
    
  QueueBacklog:
    Expression: queue_depth > 1000
    Severity: warning
    
  DiskSpace:
    Expression: node_filesystem_avail_bytes / node_filesystem_size_bytes < 0.1
    Severity: critical
```

---

## CI/CD Pipeline

### GitHub Actions Workflow

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  AWS_REGION: us-east-1
  ECR_REGISTRY: ${{ secrets.AWS_ACCOUNT_ID }}.dkr.ecr.us-east-1.amazonaws.com

jobs:
  lint-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Lint
        run: npm run lint
        
      - name: Type check
        run: npm run typecheck
        
      - name: Unit tests
        run: npm run test:unit -- --coverage
        
      - name: Upload coverage
        uses: codecov/codecov-action@v3

  integration-tests:
    runs-on: ubuntu-latest
    needs: lint-and-test
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_DB: test
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
        ports:
          - 5432:5432
      redis:
        image: redis:7
        ports:
          - 6379:6379
    steps:
      - uses: actions/checkout@v4
      - name: Run integration tests
        run: npm run test:integration

  build-and-push:
    runs-on: ubuntu-latest
    needs: integration-tests
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}
          
      - name: Login to ECR
        uses: aws-actions/amazon-ecr-login@v2
        
      - name: Build and push Docker image
        run: |
          docker build -t $ECR_REGISTRY/api:${{ github.sha }} .
          docker push $ECR_REGISTRY/api:${{ github.sha }}

  deploy-staging:
    runs-on: ubuntu-latest
    needs: build-and-push
    environment: staging
    steps:
      - name: Deploy to ECS
        run: |
          aws ecs update-service \
            --cluster staging-cluster \
            --service api-service \
            --force-new-deployment

  deploy-production:
    runs-on: ubuntu-latest
    needs: deploy-staging
    environment: production
    steps:
      - name: Deploy to ECS
        run: |
          aws ecs update-service \
            --cluster production-cluster \
            --service api-service \
            --force-new-deployment
```

---

## Local Development

### Docker Compose Configuration

```yaml
# docker-compose.yml
version: '3.8'

services:
  api:
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
      - DATABASE_URL=postgresql://dev:dev@postgres:5432/app_dev
      - REDIS_URL=redis://redis:6379
      - ELASTICSEARCH_URL=http://elasticsearch:9200
      - MONGODB_URL=mongodb://mongo:27017/app_dev
      - MINIO_ENDPOINT=minio
      - MINIO_PORT=9000
    depends_on:
      - postgres
      - redis
      - elasticsearch
      - mongo
      - minio

  worker:
    build:
      context: .
      dockerfile: Dockerfile.dev
    command: npm run worker
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
      - DATABASE_URL=postgresql://dev:dev@postgres:5432/app_dev
      - REDIS_URL=redis://redis:6379
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:15
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_USER=dev
      - POSTGRES_PASSWORD=dev
      - POSTGRES_DB=app_dev
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

  elasticsearch:
    image: elasticsearch:8.11.0
    ports:
      - "9200:9200"
    environment:
      - discovery.type=single-node
      - xpack.security.enabled=false
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    volumes:
      - es_data:/usr/share/elasticsearch/data

  mongo:
    image: mongo:7
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db

  minio:
    image: minio/minio
    ports:
      - "9000:9000"
      - "9001:9001"
    environment:
      - MINIO_ROOT_USER=minioadmin
      - MINIO_ROOT_PASSWORD=minioadmin
    command: server /data --console-address ":9001"
    volumes:
      - minio_data:/data

  mailhog:
    image: mailhog/mailhog
    ports:
      - "1025:1025"
      - "8025:8025"

volumes:
  postgres_data:
  redis_data:
  es_data:
  mongo_data:
  minio_data:
```

### Environment Variables Template

```bash
# .env.example

# Application
NODE_ENV=development
PORT=3000
API_VERSION=v1
LOG_LEVEL=debug

# Database
DATABASE_URL=postgresql://dev:dev@localhost:5432/app_dev
MONGODB_URL=mongodb://localhost:27017/app_dev

# Cache & Queue
REDIS_URL=redis://localhost:6379

# Search
ELASTICSEARCH_URL=http://localhost:9200

# File Storage
MINIO_ENDPOINT=localhost
MINIO_PORT=9000
MINIO_ACCESS_KEY=minioadmin
MINIO_SECRET_KEY=minioadmin
MINIO_BUCKET=uploads

# Auth
JWT_PRIVATE_KEY_PATH=./keys/private.pem
JWT_PUBLIC_KEY_PATH=./keys/public.pem
JWT_ACCESS_TOKEN_TTL=15m
JWT_REFRESH_TOKEN_TTL=7d

# OAuth
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
MICROSOFT_CLIENT_ID=
MICROSOFT_CLIENT_SECRET=

# Email
SMTP_HOST=localhost
SMTP_PORT=1025
SMTP_FROM=noreply@example.com

# External APIs
SENDGRID_API_KEY=
TWILIO_ACCOUNT_SID=
TWILIO_AUTH_TOKEN=

# Monitoring
SENTRY_DSN=
```

---

## Deployment Playbook

### Pre-Deployment Checklist

```yaml
Infrastructure:
  - [ ] VPC and subnets created
  - [ ] Security groups configured
  - [ ] RDS instance provisioned
  - [ ] ElastiCache cluster created
  - [ ] OpenSearch domain created
  - [ ] S3 buckets created
  - [ ] ECR repository created
  - [ ] ECS cluster created
  - [ ] ALB configured
  - [ ] CloudFront distribution created
  - [ ] Route 53 records configured
  - [ ] SSL certificates provisioned

Secrets:
  - [ ] Database credentials in Secrets Manager
  - [ ] JWT keys in Secrets Manager
  - [ ] OAuth credentials in Secrets Manager
  - [ ] API keys in Secrets Manager

Application:
  - [ ] Docker images built and pushed to ECR
  - [ ] Task definitions created
  - [ ] ECS services configured
  - [ ] Auto-scaling policies set
  - [ ] Health checks configured

Monitoring:
  - [ ] CloudWatch log groups created
  - [ ] CloudWatch alarms configured
  - [ ] Sentry project configured
  - [ ] Grafana dashboards created
```

### Terraform Structure

```
infrastructure/
├── environments/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── terraform.tfvars
│   ├── staging/
│   │   └── ...
│   └── production/
│       └── ...
├── modules/
│   ├── vpc/
│   ├── ecs/
│   ├── rds/
│   ├── elasticache/
│   ├── opensearch/
│   ├── s3/
│   └── cloudfront/
└── shared/
    ├── ecr/
    └── iam/
```

### Rollback Procedure

```bash
# 1. Identify the previous working task definition
aws ecs describe-services \
  --cluster production-cluster \
  --services api-service \
  --query 'services[0].deployments'

# 2. Update service to previous task definition
aws ecs update-service \
  --cluster production-cluster \
  --service api-service \
  --task-definition api-task:PREVIOUS_VERSION

# 3. Monitor deployment
aws ecs wait services-stable \
  --cluster production-cluster \
  --services api-service

# 4. Verify health
curl -f https://api.example.com/health
```

---

## ✅ Best Practices Summary

### 📁 Code Organization
| Practice | Description |
|----------|-------------|
| Monorepo | Use Nx workspace for managing multiple apps |
| Separation | API, Worker, Web, Shared libs in separate packages |
| DDD | Follow domain-driven design principles |
| Standards | ESLint, Prettier, consistent coding standards |

### 🔐 Security
| Practice | Description |
|----------|-------------|
| Secrets | Never commit to repository, use Secrets Manager |
| Validation | Input validation on all endpoints |
| RBAC | Role-based access control everywhere |
| Audits | Regular security audits and dependency updates |

### ⚡ Performance
| Practice | Description |
|----------|-------------|
| Caching | Multi-level caching (Redis, CDN) |
| Pooling | Database connection pooling |
| Indexing | Proper database indexes |
| Pagination | Always paginate large datasets |

### 🛡️ Reliability
| Practice | Description |
|----------|-------------|
| Health checks | Liveness and readiness probes |
| Circuit breakers | For external service calls |
| Retry logic | Exponential backoff |
| Graceful degradation | Design for partial failures |

### 📊 Observability
| Practice | Description |
|----------|-------------|
| Logging | Structured logs with correlation IDs |
| Metrics | Prometheus metrics for all services |
| Tracing | Distributed tracing with OpenTelemetry |
| Alerting | Proactive alerts for anomalies |

---

<div align="center">

**[⬅️ Back to Project Index](./00-Project-Index-Overview.md)**

---

**🔧 This guide is shared across all 8 portfolio projects**

Made with ❤️ for production-ready applications

*Last Updated: December 2024*

</div>
