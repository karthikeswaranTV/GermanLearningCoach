German Language Coach

Product Requirements Document — Step 15: Deployment, Release & Operations

Version: 0.1
Status: Draft

1. Purpose

This document defines how the German Language Coach is built, deployed, released, monitored, backed up, and operated after launch.

The goal is:

Create a simple, secure, repeatable, low-cost deployment process for the MVP, with clear rollback and operational procedures.

The MVP should avoid unnecessary infrastructure complexity.

2. Deployment Principles

The deployment architecture should be:

Simple
Low cost
Automated
Secure
Observable
Repeatable
Rollback-friendly

Prefer managed services where they significantly reduce operational work.

3. Target Environments

Use three primary environments:

Development
     ↓
Staging
     ↓
Production

Development is for active coding.

Staging validates release candidates.

Production serves real users.

4. Recommended MVP Architecture

Conceptually:

                    INTERNET
                       │
                       ▼
                Web / Mobile App
                       │
                       ▼
                 Backend API
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
      PostgreSQL      AI API      Storage
          │
          ▼
       Backups

Backend
  +
Frontend
  +
Database

The exact hosting provider can be selected based on cost, region, reliability, and current free-tier availability.

5. Free-First Infrastructure

For the MVP:

Use free/low-cost hosting where practical
Use managed PostgreSQL where practical
Use free HTTPS
Use a free CI/CD tier where practical
Use free monitoring where sufficient
Avoid dedicated servers initially

Important:

Free-tier limits change over time. Verify current pricing and quotas before production deployment.

6. Repository Structure

Recommended:

GermanLearningCoach/
├── frontend/
├── backend/
├── database/
├── PRD/
├── docs/
├── scripts/
├── .github/
└── README.md

Adapt this to the existing repository structure rather than restructuring unnecessarily.

7. Branch Strategy

Simple MVP approach:

main
  ↑
feature/*

Optional:

develop

Do not introduce complex GitFlow unless the team actually needs it.

8. Pull Request Flow

Recommended:

Developer
   ↓
feature/branch
   ↓
Pull Request
   ↓
Automated Tests
   ↓
Code Review
   ↓
Merge
   ↓
Staging

Production should deploy only from an approved branch/tag.

9. Versioning

Use semantic-style versions:

v0.1.0
v0.2.0
v1.0.0

Example:

v0.1.0 = initial MVP
v0.1.1 = bug fix
v0.2.0 = feature release

10. Release Candidate

Before production:

Merge approved changes
        ↓
Build
        ↓
Automated tests
        ↓
Deploy staging
        ↓
Run smoke tests
        ↓
UAT
        ↓
Release approval

11. CI/CD

Recommended pipeline:

Push / PR
   ↓
Lint
   ↓
Type Check
   ↓
Unit Tests
   ↓
Integration Tests
   ↓
Security Scan
   ↓
Build
   ↓
Deploy Staging
   ↓
E2E Tests

Production:

Approved release
   ↓
Deploy Production
   ↓
Smoke Tests
   ↓
Monitor

12. CI Pipeline Requirements

CI should verify:

Frontend builds
Backend builds
Tests pass
Lint passes
Type checks pass
Dependencies are valid
No obvious secrets are committed

13. Environment Variables

Separate configuration by environment.

Example:

DATABASE_URL
JWT_SECRET
AI_PRIMARY_PROVIDER
AI_PRIMARY_API_KEY
AI_MODEL
AI_MAX_TOKENS
AI_DAILY_LIMIT
APP_ENV
APP_URL

Never commit real values.

14. Environment Files

Repository may contain:

.env.example

Do not commit:

.env
.env.production

unless they contain no secrets and are intentionally used as templates.

15. Secret Management

Production secrets should be stored in:

Hosting platform secrets
CI/CD secret store
Dedicated secret manager

Do not store secrets in:

Git
Docker image
Frontend JavaScript
Logs
Database records

16. Frontend Environment Variables

Only variables that are safe to expose should be available to the frontend.

Never expose:

Database credentials
AI API keys
JWT signing secret
Private service credentials

17. Database Deployment

Production database should be managed separately from application deployment.

Deployment flow:

Application release
        +
Database migration
        ↓
Compatibility check
        ↓
Application deployment

Prefer backward-compatible migrations for zero/minimal downtime.

18. Database Migration Rules

Every schema change should have a migration.

Examples:

Add table
Add column
Add index
Add constraint
Modify relationship

Never rely on manually editing production tables.

19. Migration Safety

Before migration:

Backup

Then:

Run migration
 ↓
Verify schema
 ↓
Verify application

For high-risk migrations, test against a production-like staging database first.

20. Deployment Strategy

MVP can use:

Rolling deployment

or the hosting platform's standard managed deployment.

Advanced options such as:

Blue/green
Canary

are future enhancements.

21. Frontend Deployment

Typical flow:

Git push
 ↓
CI
 ↓
Build
 ↓
Deploy
 ↓
CDN / Hosting

Verify:

Routes
Assets
API configuration
HTTPS
Environment variables

22. Backend Deployment

Typical flow:

Git push
 ↓
CI
 ↓
Tests
 ↓
Build
 ↓
Deploy
 ↓
Health check

The backend must expose a health endpoint.

Example:

GET /health

23. Health Check

Example response:

{
  "status": "ok"
}

Do not expose:

Database credentials
AI keys
Internal infrastructure details

24. Readiness Check

Optional:

GET /ready

Can verify that required dependencies are available.

Keep health endpoints lightweight.

25. Domain & HTTPS

Production should use:

HTTPS

Example:

app.example.com
api.example.com

Use a trusted certificate provided by the hosting platform or certificate authority.

26. CORS

Configure CORS explicitly.

Allow only expected frontend origins.

Do not use:

Access-Control-Allow-Origin: *

for authenticated production APIs unless there is a deliberate security reason.

27. Database Security

Production database should:

Require authentication
Use encrypted connections where supported
Restrict network access
Use least-privilege credentials
Have backups

Do not expose the database directly to the public internet unless absolutely necessary.

28. Monitoring

Monitor:

Application errors
API latency
Database health
AI failures
AI quota
Authentication failures
Notification failures
CPU/memory where applicable

29. Logging

Production logs should include:

timestamp
requestId
service
endpoint
HTTP status
latency
error category

Never log:

Passwords
API keys
Session secrets
Full authentication tokens
Unnecessary private learner conversations

30. Error Tracking

Use an error tracking service if practical.

Track:

Exception
Stack trace
Release version
Environment
Request ID

Group repeated errors to identify regressions.

31. Application Metrics

Track:

Active users
Learning sessions
Lesson completion
Exercise completion
AI requests
AI success rate
AI latency
Fallback rate
Notification delivery
API error rate

32. AI Monitoring

Because AI is central to the product, monitor:

Requests
Success
Failures
Latency
Tokens
Quota
Provider
Model
Fallbacks

Alert on unusual increases.

33. Cost Monitoring

Even for a free MVP:

AI usage
Database usage
Storage
Bandwidth
Hosting limits
Notification usage

must be monitored.

The goal is to avoid unexpected bills.

34. Free-Tier Protection

Use:

Application quotas
Provider quotas
Rate limits
Maximum request sizes
Maximum AI tokens
Usage alerts

If a service approaches its free limit, the team should know before the limit is exceeded.

35. Backups

Production database should have automated backups where supported.

Recommended:

Daily backup
+
Provider-managed point-in-time recovery if available

Retention depends on hosting capability and cost.

36. Backup Verification

A backup is not considered reliable until it has been restored successfully.

Test:

Create backup
 ↓
Restore test copy
 ↓
Run application
 ↓
Verify critical data

Perform this periodically.

37. Disaster Recovery

Document:

Database restore
Application rollback
Secret recovery
AI provider outage
Hosting outage
Domain recovery

Keep the procedure in the repository.

38. Recovery Objectives

Define initial targets:

RPO — acceptable data loss
RTO — acceptable recovery time

Example MVP target:

RPO: 24 hours
RTO: 4 hours

These are initial planning targets and should be adjusted based on actual business requirements and backup capabilities.

39. Rollback

Every production release needs a rollback plan.

Application rollback:

Current release
      ↓
Problem detected
      ↓
Deploy previous known-good release
      ↓
Smoke test

40. Database Rollback

Database rollback requires extra care.

Prefer:

Backward-compatible migration

Example:

Deploy schema addition
 ↓
Deploy application using new field
 ↓
Later remove old field

Avoid destructive schema changes in the same release as the application change.

41. Feature Flags

Future:

FEATURE_AI_COACH
FEATURE_NEW_LESSON_UI
FEATURE_PUSH_NOTIFICATIONS

Feature flags allow controlled rollout.

Not every MVP feature requires a flag.

42. Release Checklist

Before production:

[ ] PR approved
[ ] CI passes
[ ] Tests pass
[ ] Security checks pass
[ ] Database migration tested
[ ] Backup verified
[ ] Environment variables configured
[ ] Staging smoke test passes
[ ] UAT complete
[ ] Release notes prepared
[ ] Rollback plan ready

43. Production Smoke Test

After deployment:

[ ] Homepage loads
[ ] Registration works
[ ] Login works
[ ] Dashboard works
[ ] Lesson opens
[ ] Exercise works
[ ] Progress updates
[ ] AI Coach responds
[ ] Notifications work
[ ] Admin login works
[ ] Database writes succeed

44. Post-Release Monitoring

For the first period after release, monitor:

Error rate
Login failures
Learning completion
AI failures
Database errors
Latency

Compare with the previous release.

45. Release Notes

Every production release should document:

Version
Release date
Features
Bug fixes
Breaking changes
Database changes
Known issues

Keep release notes concise.

46. Incident Severity

Use:

P0 — Critical
P1 — High
P2 — Medium
P3 — Low

Examples:

P0:
Application unavailable.

P1:
Learning progress is being lost.

P2:
AI Coach has elevated failures.

P3:
Minor UI issue.

47. Incident Response

Basic flow:

Detect
 ↓
Assess
 ↓
Contain
 ↓
Recover
 ↓
Verify
 ↓
Document
 ↓
Prevent recurrence

48. Incident Communication

For serious incidents:

What happened?
Who is affected?
What is being done?
Has the issue been resolved?
What happens next?

Keep communication factual.

49. Postmortem

For significant incidents, record:

Timeline
Root cause
Impact
Detection
Resolution
Contributing factors
Preventive actions

Avoid blame-focused reporting.

50. Dependency Updates

Regularly review:

Frontend dependencies
Backend dependencies
Build tools
Database drivers
AI SDKs
Security packages

Update safely through normal PR/testing processes.

51. Security Updates

Critical security updates should be prioritized.

Process:

Security advisory
 ↓
Assess impact
 ↓
Update dependency/configuration
 ↓
Run tests
 ↓
Deploy

52. Domain Operations

Maintain:

Domain ownership
DNS configuration
HTTPS certificate
Email configuration if used

Document where these are managed.

53. Email Operations

If email is added:

Verify sender domain
Configure SPF
Configure DKIM
Configure DMARC
Monitor delivery

Provider-specific configuration should be documented separately.

54. Mobile Release Operations

If mobile apps are released:

Build
 ↓
Automated tests
 ↓
Internal testing
 ↓
Beta/TestFlight/Play testing
 ↓
Production release

Use staged rollout where appropriate.

55. Web Release Operations

For the web MVP:

Merge
 ↓
CI
 ↓
Staging
 ↓
UAT
 ↓
Production

The web release should be independent of mobile releases where possible.

56. Database Seed Data

Maintain safe seed data for:

Development
Testing
Staging

Never use real production learner data as development seed data.

57. Production Access

Use least privilege.

Separate:

Developer access
Admin application access
Database access
Infrastructure access

Production database access should be restricted.

58. Operational Documentation

Repository should contain:

docs/
├── deployment.md
├── rollback.md
├── database.md
├── monitoring.md
├── incident-response.md
└── troubleshooting.md

These documents should be updated when operational procedures change.

59. Troubleshooting Guide

Document common problems:

Login failure
Database unavailable
AI unavailable
Notification failure
Deployment failure
Migration failure
High API latency

Each should include:

Symptoms
Checks
Likely causes
Recovery
Escalation

60. MVP Infrastructure Recommendation

Keep the first release simple:

Frontend
   ↓
Managed web hosting

Backend
   ↓
Managed application/container hosting

Database
   ↓
Managed PostgreSQL

AI
   ↓
Free-tier cloud AI provider

CI/CD
   ↓
Git-based CI

Monitoring
   ↓
Free/low-cost monitoring

Backups
   ↓
Managed database backups

Do not introduce Kubernetes, microservices, or complex infrastructure for the MVP unless actual requirements justify them.

61. Avoid Premature Microservices

MVP should preferably use:

Modular monolith

rather than:

Many independent services

Suggested:

Backend
├── Auth
├── Learning
├── AI Coach
├── Gamification
├── Notifications
├── Admin
└── Analytics

These are logical modules, not necessarily separate deployments.

62. Scaling Path

Start:

1 frontend
1 backend
1 database

Scale when needed:

Load balancer
Multiple backend instances
Redis/cache
Background workers
Read replicas
Object storage

Only add infrastructure based on measurable demand.

63. Background Jobs

The application may eventually need workers for:

Notifications
Weekly reports
AI processing
Analytics
Maintenance

MVP can use a managed scheduler/worker where available.

64. Production Configuration Checklist

[ ] APP_ENV=production
[ ] Production database configured
[ ] Secrets configured
[ ] AI provider configured
[ ] AI quotas configured
[ ] CORS configured
[ ] HTTPS enabled
[ ] Logging enabled
[ ] Error tracking enabled
[ ] Backup enabled
[ ] Health check enabled

65. Pre-Launch Checklist

Product:

[ ] Registration
[ ] Login
[ ] Onboarding
[ ] Learning
[ ] Exercises
[ ] Progress
[ ] Gamification
[ ] AI Coach
[ ] Notifications
[ ] Admin

Technical:

[ ] Tests pass
[ ] Security checks pass
[ ] Database backup works
[ ] Deployment works
[ ] Rollback works
[ ] Monitoring works

66. Launch Day Checklist

[ ] Final backup
[ ] Final release approved
[ ] Production deployment
[ ] Health check
[ ] Smoke test
[ ] Login verification
[ ] Learning verification
[ ] AI verification
[ ] Admin verification
[ ] Monitor errors

67. Post-Launch Checklist

Within the first days:

[ ] Review errors
[ ] Review AI usage
[ ] Review provider quota
[ ] Review database usage
[ ] Review user feedback
[ ] Review learning completion
[ ] Review notification engagement
[ ] Fix critical issues

68. Definition of Done

Deployment & Operations is complete when:

[ ] Development environment works
[ ] Staging environment works
[ ] Production environment works
[ ] CI/CD is configured
[ ] Secrets are protected
[ ] Database migrations are automated
[ ] HTTPS works
[ ] Health checks exist
[ ] Monitoring exists
[ ] Error tracking exists
[ ] Backups exist
[ ] Backup restoration has been tested
[ ] Rollback procedure exists
[ ] Release checklist exists
[ ] Incident procedure exists
[ ] Operational documentation exists

69. Final Release Pipeline

Developer
    ↓
Feature Branch
    ↓
Pull Request
    ↓
CI
    ├── Lint
    ├── Type Check
    ├── Unit Tests
    ├── Integration Tests
    ├── Security Scan
    └── Build
    ↓
Staging
    ↓
E2E
    ↓
UAT
    ↓
Release Approval
    ↓
Production
    ↓
Health Check
    ↓
Smoke Test
    ↓
Monitoring

70. Final MVP Architecture

                         USERS
                           │
                           ▼
                    WEB / MOBILE APP
                           │
                           ▼
                    HTTPS / DOMAIN
                           │
                           ▼
                     BACKEND API
                           │
       ┌───────────────────┼───────────────────┐
       ▼                   ▼                   ▼
 Authentication       Learning Engine       AI Coach
       │                   │                   │
       │                   ├── Gamification    │
       │                   └── Notifications  │
       │                                       │
       └───────────────────┬───────────────────┘
                           ▼
                     PostgreSQL
                           │
                           ▼
                       Backups

AI Coach ────────────────► Cloud AI Provider

Application ─────────────► Monitoring / Logs

Git ─────────────────────► CI/CD ─────► Staging
                                      └─► Production

71. Key Architectural Rule

The MVP should be easy to deploy, easy to observe, easy to restore, and easy to roll back.

Avoid infrastructure complexity until actual usage requires it.

72. Final PRD Phase Outcome

With Step 15 complete, the German Language Coach PRD defines:

Product
UX/UI
Architecture
Database
API
AI Coach
Learning Engine
Gamification
Curriculum
Authentication
AI Providers
Notifications
Admin
Testing
Deployment
Operations

The project is now ready to move from:

PLANNING

to:

IMPLEMENTATION

The next phase should be executed incrementally:

1. Repository inspection
2. Development environment setup
3. Backend foundation
4. Database implementation
5. Authentication
6. Learning Engine
7. AI Coach
8. Gamification
9. Frontend
10. Notifications
11. Admin
12. Testing
13. Deployment
14. MVP launch

73. Final MVP Principle

Build the smallest complete learning product first, validate that users actually learn and return, then scale the architecture based on real usage
