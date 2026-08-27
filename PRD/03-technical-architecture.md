# German Language Coach

## Product Requirements Document — Step 3: Technical Architecture

**Version:** 0.1
**Status:** Draft
**Depends on:**

* `01-product-definition.md`
* `02-ux-ui-specification.md`

---

# 1. Architecture Goal

Build a scalable, maintainable architecture for the German Language Coach MVP.

The architecture must support:

* User authentication
* Personalized learning
* AI tutoring
* A1 curriculum
* Learning progress
* Mistake tracking
* Vocabulary mastery
* Grammar mastery
* Speaking practice
* Conversation
* Spaced repetition
* XP and streaks
* Analytics

The architecture should be simple enough for an MVP but structured enough to grow toward A2, B1 and B2.

---

# 2. Architecture Principle

Use a:

> **Modular Monolith**

for the MVP.

Do NOT start with microservices.

The application should have clearly separated modules inside one deployable application.

Future services can be extracted if scale requires it.

---

# 3. High-Level Architecture

```text
                         USER
                           │
                           ▼
                    ┌─────────────┐
                    │  Next.js UI │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │ API Layer   │
                    └──────┬──────┘
                           │
          ┌────────────────┼─────────────────┐
          │                │                 │
          ▼                ▼                 ▼
     Auth Module     Learning Module    AI Module
          │                │                 │
          │                ▼                 ▼
          │          Learning Engine     AI Provider
          │                │
          ▼                ▼
                    ┌─────────────┐
                    │ PostgreSQL  │
                    └─────────────┘
                           │
                 ┌─────────┴─────────┐
                 ▼                   ▼
             File/Audio           Analytics
              Storage              Events
```

---

# 4. Recommended Technology Stack

## Frontend

Use:

* Next.js
* React
* TypeScript
* Tailwind CSS
* shadcn/ui

## Backend

Use:

* Next.js server-side APIs / route handlers
* TypeScript

Keep business logic in service modules.

## Database

Use:

* PostgreSQL
* Prisma ORM

## Authentication

Use a secure authentication solution supporting:

* Email/password
* OAuth where practical
* Sessions
* Password reset

## AI

Create an AI provider abstraction.

The application must not depend directly on one AI provider throughout the codebase.

## Audio

Create an audio abstraction supporting:

* Text-to-speech
* Speech-to-text

## Testing

Use:

* Vitest or Jest for unit tests
* API/integration testing
* Playwright for E2E testing

---

# 5. Application Structure

Recommended project structure:

```text
src/
│
├── app/
│   ├── page.tsx
│   ├── login/
│   ├── register/
│   ├── onboarding/
│   ├── assessment/
│   ├── dashboard/
│   ├── learn/
│   ├── practice/
│   ├── talk/
│   ├── review/
│   └── profile/
│
├── components/
│   ├── ui/
│   ├── dashboard/
│   ├── lesson/
│   ├── vocabulary/
│   ├── conversation/
│   ├── review/
│   └── progress/
│
├── services/
│   ├── auth/
│   ├── learning/
│   ├── lessons/
│   ├── vocabulary/
│   ├── grammar/
│   ├── assessment/
│   ├── conversation/
│   ├── speaking/
│   ├── review/
│   ├── progress/
│   ├── gamification/
│   ├── ai/
│   └── audio/
│
├── repositories/
│
├── lib/
│   ├── db/
│   ├── auth/
│   ├── validation/
│   └── utils/
│
├── types/
│
└── tests/
```

---

# 6. Architecture Layers

Use four logical layers.

```text
UI
 ↓
API / Server Actions
 ↓
Services
 ↓
Repositories
 ↓
Database
```

### UI

Responsible for:

* Display
* Interaction
* Form state
* Loading states
* Error states

### API

Responsible for:

* Authentication
* Authorization
* Validation
* Request/response handling

### Services

Responsible for:

* Business logic
* Learning logic
* AI orchestration
* Progress
* Gamification

### Repositories

Responsible for:

* Database access
* Queries
* Persistence

UI components must NOT directly access Prisma.

---

# 7. Core Modules

Create these application modules:

```text
Authentication
User Profile
Onboarding
Assessment
Learning
Lessons
Vocabulary
Grammar
Speaking
Conversation
Review
Progress
Gamification
AI
Audio
Analytics
```

---

# 8. Authentication Module

Responsibilities:

* Registration
* Login
* Logout
* Password reset
* Session management
* OAuth
* Authorization

Every protected API must verify the authenticated user.

---

# 9. User Module

Store:

* User identity
* Native language
* German level
* Goals
* Interests
* Daily study time
* Learning preference
* Current progress

Example:

```text
User
 └── Profile
      ├── nativeLanguage
      ├── germanLevel
      ├── goals
      ├── interests
      ├── dailyMinutes
      └── learningPreference
```

---

# 10. Learning Module

This is the central business module.

Responsibilities:

* Determine current learning state
* Track mastery
* Identify weak areas
* Recommend lessons
* Update learner model
* Coordinate reviews

Primary service:

```text
LearningService
```

Functions:

```text
getLearnerState()
getRecommendedActivity()
updateLearningProgress()
detectWeakAreas()
calculateSkillProgress()
```

---

# 11. Coach Brain

Create a central recommendation component.

Conceptually:

```text
                 COACH BRAIN
                      │
       ┌──────────────┼──────────────┐
       ▼              ▼              ▼
   Performance      Mistakes       Goals
       │              │              │
       └──────────────┼──────────────┘
                      ▼
                Recommendation
                      │
                      ▼
              Next Learning Task
```

The Coach Brain should determine:

* What the learner should learn
* What should be reviewed
* What should be practiced
* What difficulty to use

---

# 12. Learning Item

Create a generic learning-item abstraction.

A learning item can represent:

* Vocabulary
* Grammar
* Listening
* Speaking
* Writing
* Reading

Example:

```text
LearningItem

id
type
conceptId
cefrLevel
difficulty
prerequisites
```

This allows different learning activities to use the same mastery system.

---

# 13. Lesson Module

Responsibilities:

* Retrieve lesson
* Start lesson
* Track progress
* Submit answers
* Complete lesson
* Generate lesson summary

Lesson structure:

```text
Lesson
 ├── Warm-up
 ├── Concept
 ├── Vocabulary
 ├── Grammar
 ├── Listening
 ├── Practice
 ├── Speaking
 ├── Application
 ├── Review
 └── Summary
```

---

# 14. Vocabulary Module

Responsibilities:

* Vocabulary catalogue
* User vocabulary
* Mastery
* Review dates
* Search
* Filtering

Vocabulary should support:

```text
word
meaning
nativeLanguageMeaning
article
plural
pronunciation
audio
examples
cefrLevel
topic
```

---

# 15. Grammar Module

Responsibilities:

* Grammar concepts
* CEFR mapping
* Prerequisites
* Mastery
* Practice questions
* Common mistakes

Example:

```text
GrammarConcept

id
name
description
cefrLevel
prerequisites
examples
```

---

# 16. Mistake Module

Mistakes are first-class learning data.

Store:

```text
Mistake

id
userId
conceptId
category
original
correction
explanation
frequency
severity
firstSeen
lastSeen
mastery
reviewPriority
```

Categories:

```text
ARTICLE
GRAMMAR
VOCABULARY
WORD_ORDER
PRONUNCIATION
SPELLING
VERB_CONJUGATION
LISTENING
NATURALNESS
```

---

# 17. Mistake Processing

When a learner makes a mistake:

```text
User Answer
     ↓
AI Evaluation
     ↓
Identify Error
     ↓
Map to Concept
     ↓
Update Mistake
     ↓
Update Mastery
     ↓
Schedule Review
     ↓
Future Recommendation
```

This loop is central to the product.

---

# 18. AI Module

Create an abstraction:

```text
AIService
```

Methods:

```text
generateLesson()
evaluateAnswer()
evaluateWriting()
evaluateSpeaking()
generateConversationResponse()
explainGrammar()
generateQuiz()
generateReview()
analyzeMistake()
estimateCEFRLevel()
```

The rest of the application must interact with this interface rather than directly calling an AI provider.

---

# 19. AI Response Design

Prefer structured responses rather than free-form text whenever possible.

Example:

```json
{
  "correct": false,
  "original": "Ich möchte ein Pizza.",
  "correction": "Ich möchte eine Pizza.",
  "category": "ARTICLE",
  "concept": "FEMININE_ARTICLE",
  "explanation": "Pizza is a feminine noun.",
  "naturalAlternative": null
}
```

The learning engine can then reliably process the result.

---

# 20. AI Prompt Management

Do not scatter prompts throughout the application.

Create centralized prompt templates:

```text
prompts/
├── lesson.md
├── grammar.md
├── correction.md
├── speaking.md
├── conversation.md
├── assessment.md
└── review.md
```

Prompts should be versioned.

Example:

```text
CORRECTION_PROMPT_VERSION = "1.0"
```

This allows future evaluation and improvement.

---

# 21. AI Safety and Reliability

AI output must be treated as untrusted input.

Validate:

* JSON structure
* Required fields
* CEFR level
* Allowed categories
* Maximum response length

If AI output is invalid:

```text
AI
 ↓
Validation fails
 ↓
Retry / fallback
 ↓
Safe response
```

Do not write malformed AI responses directly into the database.

---

# 22. Audio Module

Create:

```text
AudioService
```

Methods:

```text
generateWordAudio()
generateSentenceAudio()
generateSlowAudio()
generateConversationAudio()
transcribeSpeech()
```

Audio should be cached.

Identical text should not repeatedly trigger new TTS generation.

---

# 23. Speaking Flow

```text
User speaks
     ↓
Audio captured
     ↓
Speech-to-text
     ↓
Transcript
     ↓
AI evaluation
     ↓
Speaking result
     ↓
Update skill
     ↓
Record mistakes
     ↓
Schedule review
```

For MVP, focus on:

> Speech transcription + AI evaluation

Advanced phonetic analysis can come later.

---

# 24. Conversation Module

Responsibilities:

* Scenario selection
* Conversation session
* Message history
* AI response
* Corrections
* Speaking input
* Session summary

Conversation:

```text
ConversationSession
 ├── Scenario
 ├── Mode
 ├── Messages
 ├── Mistakes
 └── Summary
```

---

# 25. Conversation Modes

Support:

```text
GUIDED
NORMAL
IMMERSION
TEACHER
INTERVIEW
WORKPLACE
```

---

# 26. Review Module

Responsibilities:

* Determine due items
* Schedule reviews
* Update mastery
* Select review questions

Functions:

```text
getDueReviews()
scheduleReview()
completeReview()
updateReviewMastery()
```

---

# 27. Spaced Repetition

Initial MVP implementation can use a simple interval algorithm.

Example states:

```text
NEW
LEARNING
FAMILIAR
STRONG
MASTERED
```

Rules should be configurable rather than hardcoded into UI.

Example:

```text
Wrong answer
→ shorten interval

Correct answer
→ increase interval

Repeated correct answers
→ move toward MASTERED
```

The algorithm should later be replaceable with a more advanced model.

---

# 28. Progress Module

Calculate:

* Speaking
* Listening
* Reading
* Writing
* Grammar
* Vocabulary
* Pronunciation

Progress should be based on actual performance.

Do not calculate:

```text
Lessons completed / Total lessons
```

as the user's CEFR level.

---

# 29. CEFR Engine

Create:

```text
CEFRService
```

Responsibilities:

* Estimate level
* Evaluate skills
* Track level progression
* Recommend next level

Example:

```text
A1
 ↓
A1 Strong
 ↓
A2 Developing
```

CEFR assessment should use performance data.

---

# 30. Gamification Module

Create:

```text
GamificationService
```

Functions:

```text
awardXP()
updateStreak()
checkAchievements()
getUserLevel()
```

XP events:

```text
LESSON_COMPLETED
REVIEW_COMPLETED
CONVERSATION_COMPLETED
DAILY_GOAL_COMPLETED
SPEAKING_COMPLETED
```

Keep XP rules outside UI components.

---

# 31. Database Architecture

Use PostgreSQL.

Prisma should manage schema and migrations.

Initial entities:

```text
User
Profile
LearningGoal
Lesson
LessonSection
LessonProgress
Vocabulary
UserVocabulary
GrammarConcept
UserGrammarProgress
LearningItem
ReviewItem
Mistake
ConversationSession
ConversationMessage
SpeakingAttempt
WritingAttempt
Assessment
AssessmentResult
XPTransaction
Streak
DailyActivity
Achievement
UserAchievement
```

---

# 32. Important Relationships

```text
User
 │
 ├── Profile
 ├── LearningProgress
 ├── Mistakes
 ├── Vocabulary
 ├── GrammarProgress
 ├── Reviews
 ├── Conversations
 ├── SpeakingAttempts
 ├── Assessments
 ├── XPTransactions
 ├── Streak
 └── DailyActivity
```

---

# 33. Data Ownership

Every learner-specific entity must belong to a user.

Examples:

```text
userId
```

must be present where required.

All queries must enforce ownership.

Never trust a user-provided ID without authorization validation.

---

# 34. API Design

Use resource-oriented APIs.

Examples:

```text
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/logout

GET    /api/profile
PATCH  /api/profile

GET    /api/dashboard

GET    /api/lessons
GET    /api/lessons/:id
POST   /api/lessons/:id/start
POST   /api/lessons/:id/complete

GET    /api/vocabulary
GET    /api/vocabulary/:id

GET    /api/review
POST   /api/review/:id/answer

GET    /api/progress

POST   /api/conversations
POST   /api/conversations/:id/messages

POST   /api/speaking/evaluate

POST   /api/assessment/start
POST   /api/assessment/:id/answer
POST   /api/assessment/:id/complete
```

Use request validation for every API.

---

# 35. API Response Standard

Use consistent responses.

Success:

```json
{
  "success": true,
  "data": {}
}
```

Error:

```json
{
  "success": false,
  "error": {
    "code": "LESSON_NOT_FOUND",
    "message": "Lesson could not be found."
  }
}
```

Do not expose internal stack traces.

---

# 36. Validation

Use a schema validation library such as Zod.

Validate:

* API input
* AI output
* User profile
* Assessment answers
* Conversation input
* Speaking metadata

---

# 37. Authorization

Every protected request should follow:

```text
Request
 ↓
Authenticate
 ↓
Identify user
 ↓
Authorize resource
 ↓
Validate input
 ↓
Execute service
```

Never rely only on frontend route protection.

---

# 38. Caching

Cache where useful:

### Good candidates

* Vocabulary definitions
* TTS audio
* Static lesson content
* Grammar explanations
* Frequently requested content

Do not cache personalized responses incorrectly.

User-specific data must be isolated.

---

# 39. Database Performance

Use indexes for frequently queried fields.

Examples:

```text
userId
lessonId
conceptId
cefrLevel
reviewDate
createdAt
```

Avoid unnecessary database calls from UI rendering.

Use repository/service-level query optimization.

---

# 40. Analytics Architecture

Track product events separately from core learning data.

Example:

```text
AnalyticsEvent

id
userId
eventName
metadata
timestamp
```

Events:

```text
USER_REGISTERED
ONBOARDING_COMPLETED
ASSESSMENT_COMPLETED
LESSON_STARTED
LESSON_COMPLETED
EXERCISE_COMPLETED
SPEAKING_COMPLETED
CONVERSATION_STARTED
CONVERSATION_COMPLETED
REVIEW_COMPLETED
DAILY_GOAL_COMPLETED
```

---

# 41. Error Handling

Use centralized error handling.

Categories:

```text
AUTH_ERROR
VALIDATION_ERROR
NOT_FOUND
AI_ERROR
AUDIO_ERROR
DATABASE_ERROR
RATE_LIMIT_ERROR
INTERNAL_ERROR
```

User-facing messages should be simple.

Internal logs should contain diagnostic information.

---

# 42. Logging

Log:

* API errors
* AI failures
* Audio failures
* Authentication failures
* Database failures
* Important learning-engine failures

Never log:

* Passwords
* Authentication tokens
* API keys
* Sensitive user information

---

# 43. Rate Limiting

AI endpoints should have rate limits.

Especially:

```text
Conversation
Speaking evaluation
Writing evaluation
Lesson generation
```

Prevent abuse and unexpected AI costs.

---

# 44. AI Cost Control

Track:

* Requests
* Tokens
* Estimated cost
* User usage
* Average cost per lesson
* Average cost per conversation

Cache reusable AI-generated content where appropriate.

---

# 45. Security Requirements

Implement:

* HTTPS
* Secure cookies
* Password hashing
* CSRF protection where applicable
* Input validation
* Authorization checks
* Rate limiting
* Secure headers
* Environment variables
* Database backups

Never expose secrets to the frontend.

---

# 46. Environment Configuration

Use environment variables.

Example:

```text
DATABASE_URL=
AUTH_SECRET=
AI_API_KEY=
TTS_API_KEY=
STT_API_KEY=
STORAGE_URL=
```

Provide:

```text
.env.example
```

Never commit real secrets.

---

# 47. Testing Architecture

Testing should exist at four levels.

## Unit

Test:

```text
calculateXP()
calculateMastery()
calculateNextReview()
detectWeakAreas()
recommendNextLesson()
```

## Integration

Test:

```text
Lesson completion
→ Progress
→ XP
→ Mistake tracking
→ Review
```

## API

Test all important endpoints.

## E2E

Test the golden path.

---

# 48. Golden Path E2E

The primary E2E test:

```text
Register
 ↓
Onboarding
 ↓
Assessment
 ↓
Dashboard
 ↓
Start lesson
 ↓
Complete exercise
 ↓
Speaking
 ↓
AI feedback
 ↓
Complete lesson
 ↓
XP updated
 ↓
Progress updated
 ↓
Review created
```

This test should run automatically in CI.

---

# 49. AI Evaluation

Create a dedicated evaluation dataset.

Example:

```text
Input:
Ich möchte ein Pizza.

Expected:
Ich möchte eine Pizza.

Category:
ARTICLE

Level:
A1
```

Evaluate:

* Accuracy
* Correction quality
* Explanation quality
* CEFR appropriateness
* Naturalness
* Safety
* Structured output validity

The AI evaluation dataset should grow as real mistakes are discovered.

---

# 50. Content Validation

Before publishing German learning content, validate:

* Grammar
* Spelling
* Naturalness
* CEFR level
* Translation
* Pronunciation
* Cultural context

AI-generated content should be reviewable before publishing.

---

# 51. CI/CD

Recommended flow:

```text
Developer
   ↓
GitHub branch
   ↓
Pull Request
   ↓
Lint
   ↓
Type Check
   ↓
Unit Tests
   ↓
Integration Tests
   ↓
Build
   ↓
E2E
   ↓
Review
   ↓
Merge
   ↓
Staging
   ↓
Production
```

---

# 52. Git Branching

Recommended:

```text
main
  │
  ├── feature/auth
  ├── feature/onboarding
  ├── feature/lessons
  ├── feature/ai-coach
  └── feature/review
```

Keep `main` deployable.

Use pull requests for meaningful changes.

---

# 53. Deployment Environments

Maintain:

```text
Local
Development/Staging
Production
```

Use separate:

* Database
* Environment variables
* AI credentials
* Storage
* Analytics configuration

---

# 54. Backup Strategy

Production database must have automated backups.

At minimum:

* Daily backup
* Retention policy
* Restore procedure

A backup is only useful if restoration has been tested.

---

# 55. Monitoring

Monitor:

### Technical

* Server errors
* API errors
* Database errors
* AI failures
* Audio failures
* Response latency

### Product

* New users
* Active users
* Lessons completed
* Learning time
* Conversations
* Reviews
* Retention

---

# 56. Scalability Strategy

Do not prematurely optimize.

Start:

```text
Next.js
+
PostgreSQL
+
AI Provider
+
Object Storage
```

When scale increases, consider extracting:

```text
AI Service
Audio Service
Learning Engine
Analytics
```

Only when there is a real need.

---

# 57. Data Migration Strategy

All schema changes must use migrations.

Never manually modify production schema.

Development:

```text
Create migration
 ↓
Test migration
 ↓
Review
 ↓
Deploy
```

---

# 58. Content Versioning

Learning content may change.

Store version information where appropriate.

Example:

```text
Lesson
 ├── version
 ├── status
 └── publishedAt
```

Statuses:

```text
DRAFT
REVIEW
PUBLISHED
ARCHIVED
```

---

# 59. Admin Architecture

Admin functionality should use the same backend services.

Admin permissions must be separate from normal user permissions.

Example:

```text
USER
CONTENT_EDITOR
ADMIN
```

---

# 60. MVP Technical Priority

Build in this order:

```text
1. Project foundation
2. Authentication
3. Database
4. User profile
5. Onboarding
6. Assessment
7. Learning engine
8. Dashboard
9. Lesson engine
10. A1 content
11. AI tutor
12. Vocabulary
13. Mistake tracking
14. Review engine
15. Speaking
16. Conversation
17. XP / streak
18. Analytics
19. Admin
20. Production hardening
```

---

# 61. MVP Vertical Slice

The first implementation should prove:

```text
Register
   ↓
Onboarding
   ↓
Assessment
   ↓
Dashboard
   ↓
Recommendation
   ↓
Lesson
   ↓
AI explanation
   ↓
Exercise
   ↓
Speaking
   ↓
AI feedback
   ↓
Mistake recorded
   ↓
XP awarded
   ↓
Progress updated
   ↓
Review scheduled
```

Do not build unrelated features before this works.

---

# 62. Definition of Done — Technical Architecture

Before development begins:

```text
[ ] Architecture approved
[ ] Technology stack approved
[ ] Repository structure approved
[ ] Database entities approved
[ ] API structure approved
[ ] Authentication approach approved
[ ] AI abstraction approved
[ ] Audio abstraction approved
[ ] Learning engine defined
[ ] Review algorithm defined
[ ] Error tracking defined
[ ] Gamification logic defined
[ ] Testing strategy defined
[ ] CI/CD defined
[ ] Security requirements defined
[ ] Monitoring strategy defined
```

---

# 63. Next Step

After this document is reviewed and approved, create:

```text
PRD/
├── 01-product-definition.md
├── 02-ux-ui-specification.md
├── 03-technical-architecture.md
└── 04-database-design.md
```

**Step 4 — Database Design** should define:

* Complete Prisma schema
* Tables
* Columns
* Data types
* Primary keys
* Foreign keys
* Indexes
* Relationships
* Constraints
* Example records
* Learning-progress model
* Mistake model
* Review model
* XP model

Do not start implementing the database until Step 4 is reviewed.
