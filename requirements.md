# QuickHire — Requirements

> A platform connecting buyers (job posters) with sellers (service providers) for local, skilled work. Think Upwork for local services — handyman, plumbing, electrical, cleaning, etc.

---

## 1. Functional Requirements

### 1.1 Identity & Authentication

| ID | Description | Priority |
|----|------------|----------|
| FR-1 | Users can sign up as a buyer or seller | P0 |
| FR-2 | Users can log in with email + password | P0 |
| FR-3 | Users can log in with Google or GitHub (OAuth 2.0) | P0 |
| FR-4 | Users can link multiple social accounts to one profile | P1 |
| FR-5 | Users can reset their password via email | P1 |
| FR-6 | Users can enable two-factor authentication | P2 |
| FR-7 | Session management with refresh token rotation | P1 |

### 1.2 Jobs (Buyer)

| ID | Description | Priority |
|----|------------|----------|
| FR-8 | Buyer can save a job as draft and finish later | P0 |
| FR-9 | Buyer can post a published job visible to sellers | P0 |
| FR-10 | Buyer can see response count (how many sellers applied) | P0 |
| FR-11 | Buyer can mark a job as finished (completed) | P1 |
| FR-12 | Buyer can cancel a job (no longer available) | P1 |
| FR-13 | Buyer can rate a seller after job completion | P1 |
| FR-14 | Buyer can hire a seller for their job | P0 |
| FR-15 | Job states: draft → published → in_progress → completed / cancelled | P0 |

### 1.3 Applications (Seller)

| ID | Description | Priority |
|----|------------|----------|
| FR-16 | Seller can view nearby jobs with distance | P0 |
| FR-17 | Seller can filter jobs by distance radius | P0 |
| FR-18 | Seller can filter jobs by category (handyman, plumbing, etc.) | P0 |
| FR-19 | Seller can follow job categories for notifications | P1 |
| FR-20 | Seller can view a detailed job page | P0 |
| FR-21 | Seller can apply to a job with a Markdown proposal | P0 |
| FR-22 | Seller can attach one file per application (PDF, image, resume) | P1 |
| FR-23 | Seller receives hire confirmation from buyer | P0 |

### 1.4 Real-Time Messaging

| ID | Description | Priority |
|----|------------|----------|
| FR-24 | Buyer and seller can exchange real-time messages after hire | P1 |
| FR-25 | Messages are persisted and viewable on reconnect | P1 |
| FR-26 | Offline users see missed messages on login | P2 |
| FR-27 | Video call support (future feature) | P3 |

### 1.5 Notifications

| ID | Description | Priority |
|----|------------|----------|
| FR-28 | Email: signup welcome, password reset | P0 |
| FR-29 | Real-time notification when a seller applies (to buyer) | P1 |
| FR-30 | Real-time notification when matching job posted (to seller) | P1 |
| FR-31 | Email fallback for offline notification delivery | P1 |
| FR-32 | Notification preferences per user, per channel | P2 |
| FR-33 | Auto-reminder after configurable duration if no response | P2 |
| FR-34 | Follow notifications: new jobs matching followed categories near seller | P1 |

### 1.6 Payments

| ID | Description | Priority |
|----|------------|----------|
| FR-35 | Buyer pays via escrow when hiring a seller | P0 |
| FR-36 | Funds held in escrow until job marked complete | P0 |
| FR-37 | Platform fee (take rate) deducted on release | P0 |
| FR-38 | Refund / dispute handling for cancelled jobs | P1 |
| FR-39 | Double-entry ledger for audit trail | P1 |
| FR-40 | Idempotency on all payment operations | P0 |

### 1.7 Admin

| ID | Description | Priority |
|----|------------|----------|
| FR-41 | Single super admin account for platform management | P0 |
| FR-42 | Admin can view platform analytics (jobs, users, revenue) | P2 |
| FR-43 | Admin can view heat map of platform activity | P3 |

### 1.8 Public Pages

| ID | Description | Priority |
|----|------------|----------|
| FR-44 | Landing page explaining platform value | P0 |
| FR-45 | Pricing page comparing plans | P1 |

### 1.9 Media & Files

| ID | Description | Priority |
|----|------------|----------|
| FR-46 | Proposal attachments: single file per application, max 10MB | P1 |
| FR-47 | File type validation (magic bytes, not just extension) | P1 |
| FR-48 | Malware scan on uploaded files | P2 |
| FR-49 | CDN-backed file delivery | P1 |

---

## 2. Non-Functional Requirements

### 2.1 Performance

| NFR | Target | Priority |
|-----|--------|----------|
| NFR-1 | API p95 latency < 200ms for read endpoints | P1 |
| NFR-2 | API p95 latency < 500ms for write endpoints | P1 |
| NFR-3 | Job search (geo-filtered) p95 < 300ms | P1 |
| NFR-4 | Real-time message delivery < 500ms p99 | P1 |
| NFR-5 | Page load (LCP) < 2.5s on landing page | P1 |

### 2.2 Scalability

| NFR | Target | Priority |
|-----|--------|----------|
| NFR-6 | Target scale: 1M to 10M users | P0 |
| NFR-7 | Scalability must be validated with load testing and benchmarking before production | P0 |
| NFR-8 | Architecture should scale to 50M users with linear cost growth | P2 |
| NFR-9 | Handle 10K concurrent buyers browsing jobs | P1 |

### 2.3 Reliability

| NFR | Target | Priority |
|-----|--------|----------|
| NFR-10 | 99.9% uptime SLA target (non-financial ops) | P1 |
| NFR-11 | Payment operations: 99.99% correctness (no double-charges) | P0 |
| NFR-12 | Graceful degradation under load (waiting room pattern) | P2 |
| NFR-13 | Retry with exponential backoff + jitter for async ops | P1 |

### 2.4 Security

| NFR | Target | Priority |
|-----|--------|----------|
| NFR-14 | OWASP Top 10 audit pass | P0 |
| NFR-15 | Rate limiting per endpoint, per user | P0 |
| NFR-16 | Brute force protection on login | P0 |
| NFR-17 | reCAPTCHA on signup/login | P1 |
| NFR-18 | Secrets never in code (env vars / vault) | P0 |
| NFR-19 | CORS + CSP properly configured | P1 |

### 2.5 Observability

| NFR | Target | Priority |
|-----|--------|----------|
| NFR-20 | Structured logging (JSON) with correlation IDs | P1 |
| NFR-21 | p50/p95/p99 latency metrics per endpoint | P1 |
| NFR-22 | Error tracking (Sentry or equivalent) | P1 |
| NFR-23 | Health checks (liveness + readiness) | P1 |

### 2.6 Data & Compliance

| NFR | Target | Priority |
|-----|--------|----------|
| NFR-24 | GDPR/CCPA: data deletion and export | P2 |
| NFR-25 | PII encrypted at rest and in transit | P1 |
| NFR-26 | Audit logging for sensitive operations | P1 |
| NFR-27 | Database backups with defined RPO/RTO | P1 |

### 2.7 Testing & Quality

| NFR | Target | Priority |
|-----|--------|----------|
| NFR-28 | End-to-end tests (Playwright) covering critical buyer and seller flows | P1 |
| NFR-29 | E2E tests run on every PR in CI pipeline | P1 |

---

## 3. Technical Constraints

| ID | Constraint |
|----|-----------|
| TC-1 | Monorepo (Turborepo) — all code in one repo |
| TC-2 | TypeScript as primary language (NestJS backend + Next.js frontend) |
| TC-3 | PostgreSQL as primary database |
| TC-4 | Zod for shared validation schemas |
| TC-5 | Docker + Kubernetes for deployment |
| TC-6 | IaC with Terraform or AWS CDK |
| TC-7 | Public GitHub repository (portfolio project) |
| TC-8 | All PRs reviewed via CodeRabbit |

---

## 4. Out of Scope (Phase 2+)

| Item | Rationale |
|------|-----------|
| SSO Module (standalone OIDC/SAML service) | Separate product, not part of QuickHire core |
| Recommendation engine (ML) | Requires data volume from production use |
| Video calls | Complex real-time infra, deferrable |
| Mobile apps (iOS/Android) | Web-first; mobile later |
| Multi-language i18n | US/Canada English first |
| Advanced analytics dashboard | Admin MVP: basic metrics only |
| Data pipeline / ETL | Requires scale to justify |

---

## 5. Glossary

| Term | Definition |
|------|-----------|
| Buyer | User who posts jobs and hires sellers |
| Seller | User who applies to and completes jobs |
| Job | A piece of work posted by a buyer |
| Application | A seller's proposal to work on a job |
| Escrow | Funds held by platform until job completion |
| Take rate | Platform fee percentage deducted from payments |
