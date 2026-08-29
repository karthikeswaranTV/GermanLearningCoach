# German Language Coach

## Product Requirements Document — Step 5: API Specification

**Version:** 0.1
**Status:** Draft
**Depends on:**

* `01-product-definition.md`
* `02-ux-ui-specification.md`
* `03-technical-architecture.md`
* `04-database-design.md`

---

# 1. Purpose

This document defines the API contract between the frontend and backend of the German Language Coach.

The API must provide a consistent interface for:

* Authentication
* User profile
* Onboarding
* Assessment
* Dashboard
* Learning
* Lessons
* Exercises
* Vocabulary
* Grammar
* Speaking
* Conversation
* Review
* Progress
* Gamification
* AI coaching

The API must be designed so that the frontend does not need to know how business logic or database operations are implemented.

---

# 2. API Principles

The API must follow these principles:

1. Validate every request.
2. Authenticate every protected request.
3. Authorize every user-owned resource.
4. Keep business logic inside services.
5. Keep database access inside repositories.
6. Return predictable response structures.
7. Never expose database implementation details.
8. Never expose AI provider credentials.
9. Never expose internal exceptions or stack traces.
10. Use appropriate HTTP status codes.
11. Keep API naming consistent.
12. Design APIs so they can evolve without breaking existing clients.

---

# 3. Base URL

Development:

```text
http://localhost:3000/api
```

Production:

```text
/api
```

All application APIs should be grouped under:

```text
/api
```

---

# 4. API Versioning

Initial MVP:

```text
/api/v1
```

Example:

```text
GET /api/v1/dashboard
```

Versioning allows future API changes without immediately breaking existing clients.

---

# 5. Authentication

Authentication endpoints:

```text
POST /api/v1/auth/register
POST /api/v1/auth/login
POST /api/v1/auth/logout
POST /api/v1/auth/forgot-password
POST /api/v1/auth/reset-password
GET  /api/v1/auth/session
```

---

# 6. Register

## Endpoint

```text
POST /api/v1/auth/register
```

## Request

```json
{
  "email": "user@example.com",
  "password": "secure-password"
}
```

## Response

```json
{
  "success": true,
  "data": {
    "user": {
      "id": "user_123",
      "email": "user@example.com"
    }
  }
}
```

The server must create the user securely.

Passwords must never be stored as plaintext.

---

# 7. Login

## Endpoint

```text
POST /api/v1/auth/login
```

## Request

```json
{
  "email": "user@example.com",
  "password": "secure-password"
}
```

## Response

```json
{
  "success": true,
  "data": {
    "user": {
      "id": "user_123",
      "email": "user@example.com"
    }
  }
}
```

Authentication should preferably use secure HTTP-only cookies/session mechanisms.

Do not expose authentication tokens unnecessarily to frontend JavaScript.

---

# 8. Logout

## Endpoint

```text
POST /api/v1/auth/logout
```

## Response

```json
{
  "success": true,
  "data": null
}
```

---

# 9. Session

## Endpoint

```text
GET /api/v1/auth/session
```

## Response

```json
{
  "success": true,
  "data": {
    "authenticated": true,
    "user": {
      "id": "user_123",
      "email": "user@example.com"
    }
  }
}
```

---

# 10. User Profile

## Get Profile

```text
GET /api/v1/profile
```

Response:

```json
{
  "success": true,
  "data": {
    "id": "profile_123",
    "nativeLanguage": "en",
    "germanLevel": "A1",
    "dailyMinutes": 15,
    "goals": [
      "WORK",
      "LIVE_IN_GERMANY"
    ],
    "interests": [
      "IT",
      "SOFTWARE_ENGINEERING"
    ]
  }
}
```

---

# 11. Update Profile

```text
PATCH /api/v1/profile
```

Request:

```json
{
  "dailyMinutes": 30,
  "goals": [
    "WORK"
  ]
}
```

Only supplied fields should be modified.

---

# 12. Onboarding

## Get Onboarding State

```text
GET /api/v1/onboarding
```

Response:

```json
{
  "success": true,
  "data": {
    "completed": false,
    "currentStep": 3,
    "totalSteps": 6
  }
}
```

---

# 13. Save Onboarding Step

```text
POST /api/v1/onboarding/steps
```

Request:

```json
{
  "step": 3,
  "data": {
    "germanLevel": "A1"
  }
}
```

The backend must validate the step-specific data.

---

# 14. Complete Onboarding

```text
POST /api/v1/onboarding/complete
```

Response:

```json
{
  "success": true,
  "data": {
    "completed": true
  }
}
```

---

# 15. Assessment

## Start Assessment

```text
POST /api/v1/assessments
```

Response:

```json
{
  "success": true,
  "data": {
    "assessmentId": "assessment_123",
    "status": "IN_PROGRESS",
    "totalQuestions": 20
  }
}
```

---

# 16. Get Assessment Question

```text
GET /api/v1/assessments/:assessmentId/questions/:questionId
```

Response:

```json
{
  "success": true,
  "data": {
    "id": "question_123",
    "type": "MULTIPLE_CHOICE",
    "skill": "GRAMMAR",
    "question": "Ich ___ Deutsch.",
    "options": [
      "lerne",
      "lernen",
      "lernst"
    ]
  }
}
```

Do not return the correct answer before submission.

---

# 17. Submit Assessment Answer

```text
POST /api/v1/assessments/:assessmentId/questions/:questionId/answer
```

Request:

```json
{
  "answer": "lerne"
}
```

Response:

```json
{
  "success": true,
  "data": {
    "correct": true,
    "nextQuestionId": "question_124"
  }
}
```

---

# 18. Complete Assessment

```text
POST /api/v1/assessments/:assessmentId/complete
```

Response:

```json
{
  "success": true,
  "data": {
    "level": "A1",
    "skills": {
      "grammar": 62,
      "vocabulary": 71,
      "reading": 65,
      "listening": 54
    },
    "focusAreas": [
      "ARTICLES",
      "LISTENING"
    ]
  }
}
```

---

# 19. Dashboard

## Endpoint

```text
GET /api/v1/dashboard
```

The dashboard endpoint should provide the information required for the main dashboard in one request.

Response:

```json
{
  "success": true,
  "data": {
    "greeting": "Guten Morgen!",
    "level": {
      "current": "A1",
      "progress": 68
    },
    "dailyGoal": {
      "minutes": 30,
      "completedMinutes": 18,
      "percentage": 60
    },
    "recommendation": {
      "type": "LESSON",
      "id": "lesson_123",
      "title": "German Articles",
      "reason": "You struggled with articles recently.",
      "estimatedMinutes": 12
    },
    "review": {
      "count": 8
    },
    "streak": 12,
    "xp": 1240,
    "focusAreas": [
      {
        "name": "Articles",
        "mastery": 62
      }
    ]
  }
}
```

---

# 20. Learning Roadmap

## Get Roadmap

```text
GET /api/v1/learning/roadmap
```

Response:

```json
{
  "success": true,
  "data": {
    "level": "A1",
    "modules": [
      {
        "id": "module_1",
        "title": "Introductions",
        "status": "COMPLETED"
      },
      {
        "id": "module_2",
        "title": "Numbers & Time",
        "status": "IN_PROGRESS"
      },
      {
        "id": "module_3",
        "title": "Daily Routine",
        "status": "LOCKED"
      }
    ]
  }
}
```

---

# 21. Lessons

## List Lessons

```text
GET /api/v1/lessons
```

Supported query parameters:

```text
level
module
skill
status
page
limit
```

Example:

```text
GET /api/v1/lessons?level=A1&skill=GRAMMAR
```

---

# 22. Get Lesson

```text
GET /api/v1/lessons/:lessonId
```

Response:

```json
{
  "success": true,
  "data": {
    "id": "lesson_123",
    "title": "German Word Order",
    "level": "A1",
    "estimatedMinutes": 8,
    "sections": []
  }
}
```

---

# 23. Start Lesson

```text
POST /api/v1/lessons/:lessonId/start
```

Response:

```json
{
  "success": true,
  "data": {
    "lessonProgressId": "progress_123",
    "startedAt": "2026-08-28T10:00:00Z"
  }
}
```

Starting a lesson should not award completion XP.

---

# 24. Save Lesson Progress

```text
PATCH /api/v1/lessons/:lessonId/progress
```

Request:

```json
{
  "currentSection": 4,
  "completedSections": [
    "section_1",
    "section_2",
    "section_3"
  ]
}
```

---

# 25. Submit Exercise

```text
POST /api/v1/lessons/:lessonId/exercises/:exerciseId/answer
```

Request:

```json
{
  "answer": "eine"
}
```

Response:

```json
{
  "success": true,
  "data": {
    "correct": true,
    "feedback": "Correct!",
    "mastery": {
      "before": 62,
      "after": 65
    }
  }
}
```

---

# 26. Complete Lesson

```text
POST /api/v1/lessons/:lessonId/complete
```

Response:

```json
{
  "success": true,
  "data": {
    "completed": true,
    "xpAwarded": 50,
    "skillsUpdated": [
      "GRAMMAR",
      "LISTENING"
    ],
    "reviewItemsCreated": 2
  }
}
```

Lesson completion must be idempotent.

Calling the endpoint multiple times must not repeatedly award XP.

---

# 27. Vocabulary

## List Vocabulary

```text
GET /api/v1/vocabulary
```

Query parameters:

```text
search
level
topic
mastery
reviewDue
page
limit
```

Example:

```text
GET /api/v1/vocabulary?level=A1&mastery=WEAK
```

---

# 28. Get Vocabulary Item

```text
GET /api/v1/vocabulary/:vocabularyId
```

Response:

```json
{
  "success": true,
  "data": {
    "id": "word_123",
    "word": "Wohnung",
    "article": "die",
    "plural": "Wohnungen",
    "meaning": "apartment",
    "example": "Ich suche eine Wohnung.",
    "audioUrl": "/audio/wohnung.mp3",
    "mastery": 72
  }
}
```

---

# 29. Grammar

## List Grammar Concepts

```text
GET /api/v1/grammar
```

Query parameters:

```text
level
status
search
```

---

# 30. Get Grammar Concept

```text
GET /api/v1/grammar/:conceptId
```

Response:

```json
{
  "success": true,
  "data": {
    "id": "grammar_123",
    "name": "German Articles",
    "level": "A1",
    "mastery": 62,
    "examples": [],
    "relatedMistakes": []
  }
}
```

---

# 31. Practice

## Get Recommended Practice

```text
GET /api/v1/practice/recommended
```

Response:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "type": "GRAMMAR",
        "conceptId": "grammar_123",
        "title": "German Articles",
        "reason": "Low mastery"
      }
    ]
  }
}
```

---

# 32. Speaking

## Submit Speaking Attempt

```text
POST /api/v1/speaking/attempts
```

The endpoint accepts an audio file.

Request:

```text
multipart/form-data
```

Fields:

```text
audio
promptId
```

Response:

```json
{
  "success": true,
  "data": {
    "attemptId": "attempt_123",
    "transcript": "Ich lerne Deutsch.",
    "score": 78,
    "feedback": {
      "pronunciation": 75,
      "grammar": 90,
      "fluency": 70,
      "comprehensibility": 85
    },
    "mistakes": []
  }
}
```

---

# 33. Speaking Evaluation Flow

Backend flow:

```text
Audio
 ↓
Validate file
 ↓
Speech-to-text
 ↓
Transcript
 ↓
AI evaluation
 ↓
Validate AI result
 ↓
Store attempt
 ↓
Update skill
 ↓
Record mistakes
 ↓
Schedule review
 ↓
Return result
```

---

# 34. Conversation

## Create Conversation

```text
POST /api/v1/conversations
```

Request:

```json
{
  "scenario": "CAFE",
  "mode": "GUIDED"
}
```

Response:

```json
{
  "success": true,
  "data": {
    "conversationId": "conversation_123",
    "scenario": "CAFE",
    "mode": "GUIDED"
  }
}
```

---

# 35. Send Conversation Message

```text
POST /api/v1/conversations/:conversationId/messages
```

Request:

```json
{
  "message": "Ich möchte eine Pizza."
}
```

Response:

```json
{
  "success": true,
  "data": {
    "message": {
      "role": "ASSISTANT",
      "content": "Natürlich. Möchten Sie etwas trinken?"
    }
  }
}
```

---

# 36. Conversation Feedback

```text
POST /api/v1/conversations/:conversationId/feedback
```

The system may periodically analyze the conversation rather than correcting every message.

Response:

```json
{
  "success": true,
  "data": {
    "feedback": [
      {
        "category": "ARTICLE",
        "original": "ein Pizza",
        "correction": "eine Pizza",
        "explanation": "Pizza is feminine."
      }
    ]
  }
}
```

---

# 37. End Conversation

```text
POST /api/v1/conversations/:conversationId/complete
```

Response:

```json
{
  "success": true,
  "data": {
    "completed": true,
    "xpAwarded": 40,
    "summary": {
      "turns": 12,
      "strengths": [
        "Vocabulary"
      ],
      "focusAreas": [
        "Articles"
      ]
    }
  }
}
```

---

# 38. Review

## Get Due Reviews

```text
GET /api/v1/reviews
```

Response:

```json
{
  "success": true,
  "data": {
    "total": 8,
    "items": [
      {
        "id": "review_123",
        "type": "GRAMMAR",
        "conceptId": "grammar_123",
        "priority": "HIGH"
      }
    ]
  }
}
```

---

# 39. Submit Review

```text
POST /api/v1/reviews/:reviewId/answer
```

Request:

```json
{
  "answer": "eine"
}
```

Response:

```json
{
  "success": true,
  "data": {
    "correct": true,
    "mastery": 68,
    "nextReviewAt": "2026-08-30T10:00:00Z"
  }
}
```

---

# 40. Progress

## Get Progress

```text
GET /api/v1/progress
```

Response:

```json
{
  "success": true,
  "data": {
    "level": "A1",
    "overall": 68,
    "skills": {
      "speaking": 42,
      "listening": 54,
      "reading": 65,
      "writing": 55,
      "grammar": 62,
      "vocabulary": 71,
      "pronunciation": 58
    }
  }
}
```

---

# 41. Progress History

```text
GET /api/v1/progress/history
```

Query parameters:

```text
period=7d
period=30d
period=90d
```

Response:

```json
{
  "success": true,
  "data": {
    "points": [
      {
        "date": "2026-08-20",
        "overall": 61
      },
      {
        "date": "2026-08-27",
        "overall": 68
      }
    ]
  }
}
```

---

# 42. Mistakes

## Get Mistakes

```text
GET /api/v1/mistakes
```

Query parameters:

```text
category
conceptId
status
severity
page
limit
```

---

# 43. Get Mistake

```text
GET /api/v1/mistakes/:mistakeId
```

Response:

```json
{
  "success": true,
  "data": {
    "id": "mistake_123",
    "category": "ARTICLE",
    "original": "ein Pizza",
    "correction": "eine Pizza",
    "frequency": 3,
    "mastery": 42,
    "reviewPriority": "HIGH"
  }
}
```

---

# 44. Gamification

## Get Gamification Summary

```text
GET /api/v1/gamification
```

Response:

```json
{
  "success": true,
  "data": {
    "xp": 1240,
    "level": 6,
    "streak": 12,
    "achievements": []
  }
}
```

---

# 45. XP Transactions

```text
GET /api/v1/gamification/xp
```

Query:

```text
page
limit
```

Response:

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "amount": 50,
        "reason": "LESSON_COMPLETED",
        "createdAt": "2026-08-28T10:00:00Z"
      }
    ]
  }
}
```

---

# 46. AI Coach

AI endpoints should generally be called through domain-specific APIs rather than exposing a generic AI endpoint.

Avoid:

```text
POST /api/ai
```

Prefer:

```text
POST /api/v1/speaking/attempts
POST /api/v1/conversations/:id/messages
POST /api/v1/lessons/:id/exercises/:id/answer
```

This keeps business context inside the appropriate module.

---

# 47. AI Service Internal Interface

Internally define:

```typescript
interface AIService {
  evaluateAnswer(input: EvaluateAnswerInput): Promise<EvaluateAnswerResult>;

  evaluateWriting(input: EvaluateWritingInput): Promise<EvaluateWritingResult>;

  evaluateSpeaking(input: EvaluateSpeakingInput): Promise<EvaluateSpeakingResult>;

  generateConversationResponse(
    input: ConversationInput
  ): Promise<ConversationResponse>;

  explainGrammar(
    input: GrammarExplanationInput
  ): Promise<GrammarExplanation>;

  generateReview(
    input: ReviewGenerationInput
  ): Promise<ReviewResult>;
}
```

The frontend should never call this interface directly.

---

# 48. API Response Standard

All APIs should use:

```json
{
  "success": true,
  "data": {}
}
```

or:

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable message."
  }
}
```

---

# 49. HTTP Status Codes

Use standard HTTP status codes.

```text
200 OK
201 Created
204 No Content
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
422 Unprocessable Entity
429 Too Many Requests
500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
```

---

# 50. Error Codes

Define application-level error codes.

Examples:

```text
AUTH_INVALID_CREDENTIALS
AUTH_SESSION_EXPIRED
AUTH_UNAUTHORIZED

VALIDATION_FAILED
RESOURCE_NOT_FOUND
RESOURCE_FORBIDDEN

LESSON_NOT_FOUND
LESSON_ALREADY_COMPLETED

ASSESSMENT_NOT_FOUND
ASSESSMENT_ALREADY_COMPLETED

REVIEW_NOT_FOUND
REVIEW_ALREADY_COMPLETED

AI_UNAVAILABLE
AI_INVALID_RESPONSE
AI_RATE_LIMITED

AUDIO_INVALID
AUDIO_PROCESSING_FAILED

DATABASE_ERROR
INTERNAL_ERROR
```

---

# 51. Validation

Use Zod or equivalent validation.

Example:

```typescript
const SubmitAnswerSchema = z.object({
  answer: z.string().min(1).max(1000)
});
```

Reject invalid requests before executing business logic.

---

# 52. Authorization

Every user-owned endpoint must verify:

```text
Authenticated user
        ↓
Resource belongs to user
        ↓
Action allowed
```

Example:

A user must never be able to retrieve another user's:

* Lesson progress
* Mistakes
* Vocabulary mastery
* Conversations
* Speaking attempts
* Assessment results
* XP history

by changing an ID in the URL.

---

# 53. Pagination

List APIs should support pagination.

Example:

```text
GET /api/v1/vocabulary?page=1&limit=20
```

Response:

```json
{
  "success": true,
  "data": {
    "items": [],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 120,
      "totalPages": 6
    }
  }
}
```

Default:

```text
limit = 20
```

Maximum:

```text
limit = 100
```

---

# 54. Filtering and Sorting

List endpoints should use predictable query parameters.

Example:

```text
GET /api/v1/mistakes?category=ARTICLE&sort=priority&order=desc
```

Validate allowed filter values.

Do not directly insert query parameters into database queries.

---

# 55. Idempotency

Important mutation endpoints should be idempotent where appropriate.

Especially:

```text
POST /lessons/:id/complete
POST /conversations/:id/complete
POST /assessments/:id/complete
```

A retry caused by a network failure must not:

* Award XP twice
* Create duplicate completion records
* Create duplicate reviews

---

# 56. Rate Limiting

Apply rate limits to:

### Authentication

```text
register
login
forgot-password
```

### AI

```text
speaking
conversation
AI evaluation
```

### Expensive operations

```text
lesson generation
assessment generation
```

Return:

```text
429 Too Many Requests
```

when the limit is exceeded.

---

# 57. File Uploads

Speaking/audio APIs must validate:

* File type
* File size
* Duration
* Content type

Reject unsupported files.

Never trust the filename or MIME type alone.

---

# 58. API Security

The API must:

* Use HTTPS in production
* Use secure cookies
* Validate all input
* Authorize all resources
* Rate-limit sensitive endpoints
* Sanitize output where required
* Protect against injection
* Protect authentication endpoints
* Never expose secrets

---

# 59. API Logging

Log:

* Request ID
* Endpoint
* HTTP method
* Status code
* Response time
* Error code

Do NOT log:

* Passwords
* Session tokens
* API keys
* Raw sensitive audio
* Sensitive personal information

---

# 60. Request IDs

Every request should have a request ID.

Example:

```text
X-Request-ID: req_123456
```

This allows frontend, backend and infrastructure logs to be correlated.

---

# 61. API Performance

Initial targets:

```text
Normal API response:
< 500ms

Dashboard:
< 1000ms

Database operations:
< 300ms where practical

AI operations:
stream or provide meaningful loading state
```

AI requests are expected to take longer than normal APIs.

---

# 62. AI Streaming

Conversation responses should support streaming where technically appropriate.

Example:

```text
User sends message
       ↓
AI processing
       ↓
Response stream
       ↓
UI displays response progressively
```

This makes conversation feel significantly more responsive.

---

# 63. Transaction Boundaries

Operations affecting multiple pieces of learner state should use database transactions where appropriate.

Example:

```text
Complete lesson
 ↓
Update lesson progress
 ↓
Update mastery
 ↓
Record mistakes
 ↓
Create reviews
 ↓
Award XP
 ↓
Update daily activity
```

These operations should be coordinated so that partial updates do not leave inconsistent learner data.

---

# 64. Dashboard Aggregation

The dashboard API should aggregate the most important information.

The frontend should NOT make ten independent API calls just to render the dashboard.

Prefer:

```text
GET /api/v1/dashboard
```

over:

```text
GET /profile
GET /progress
GET /reviews
GET /gamification
GET /recommendation
GET /streak
...
```

The backend can perform the required queries internally.

---

# 65. Caching

Cache appropriate data:

```text
Static lessons
Vocabulary
Grammar concepts
Audio
Published content
```

Do not incorrectly cache personalized information.

Personalized dashboard data should be user-specific.

---

# 66. API Testing

Every API must have tests for:

### Success

```text
Valid request
Authenticated user
Correct resource
```

### Validation

```text
Missing fields
Invalid types
Invalid enum values
Oversized input
```

### Authorization

```text
Unauthenticated
Wrong user
Forbidden resource
```

### Not Found

```text
Invalid resource ID
```

### Business Rules

```text
Already completed
Duplicate action
Invalid learning state
```

### Failure

```text
AI unavailable
Database failure
Audio processing failure
```

---

# 67. Critical API Integration Tests

The following workflows must be tested end-to-end at the API level.

## Lesson Completion

```text
Start lesson
 ↓
Submit exercises
 ↓
Complete lesson
 ↓
Progress updated
 ↓
Mastery updated
 ↓
Mistake recorded
 ↓
Review scheduled
 ↓
XP awarded
```

## Speaking

```text
Upload audio
 ↓
Transcribe
 ↓
Evaluate
 ↓
Store attempt
 ↓
Update skill
 ↓
Record mistake
```

## Conversation

```text
Create conversation
 ↓
Send message
 ↓
AI response
 ↓
Store messages
 ↓
Complete conversation
 ↓
Update progress
 ↓
Award XP
```

---

# 68. API Documentation

Generate API documentation using OpenAPI where practical.

The API documentation should contain:

* Endpoint
* Method
* Authentication requirement
* Request schema
* Response schema
* Error responses
* Example requests
* Example responses

The OpenAPI specification should become the single source of truth for API contracts.

---

# 69. Frontend API Client

Do not call fetch directly throughout the application.

Create a central API client.

Example:

```typescript
api.dashboard.get()

api.lessons.get(id)

api.lessons.start(id)

api.lessons.submitAnswer(id, exerciseId, answer)

api.conversations.create(input)

api.conversations.sendMessage(id, message)

api.reviews.getDue()
```

This keeps frontend code consistent.

---

# 70. Service Boundary

The request flow should be:

```text
Frontend
   ↓
API Route
   ↓
Validation
   ↓
Authentication
   ↓
Authorization
   ↓
Service
   ↓
Repository
   ↓
Database
```

For AI:

```text
Service
   ↓
AI Service
   ↓
AI Provider
```

---

# 71. API Definition of Done

Before implementation begins:

```text
[ ] Authentication APIs defined
[ ] Profile APIs defined
[ ] Onboarding APIs defined
[ ] Assessment APIs defined
[ ] Dashboard API defined
[ ] Learning APIs defined
[ ] Lesson APIs defined
[ ] Exercise APIs defined
[ ] Vocabulary APIs defined
[ ] Grammar APIs defined
[ ] Speaking APIs defined
[ ] Conversation APIs defined
[ ] Review APIs defined
[ ] Progress APIs defined
[ ] Mistake APIs defined
[ ] Gamification APIs defined
[ ] AI service interface defined
[ ] Error model defined
[ ] Validation defined
[ ] Authorization defined
[ ] Pagination defined
[ ] Rate limiting defined
[ ] Idempotency defined
[ ] API testing strategy defined
[ ] OpenAPI documentation planned
```

---

# 72. API Golden Path

The complete MVP API flow should support:

```text
REGISTER
   ↓
LOGIN
   ↓
ONBOARDING
   ↓
ASSESSMENT
   ↓
DASHBOARD
   ↓
RECOMMENDATION
   ↓
START LESSON
   ↓
SUBMIT EXERCISES
   ↓
SPEAKING ATTEMPT
   ↓
AI EVALUATION
   ↓
COMPLETE LESSON
   ↓
UPDATE MASTERY
   ↓
RECORD MISTAKE
   ↓
CREATE REVIEW
   ↓
AWARD XP
   ↓
UPDATE STREAK
   ↓
DASHBOARD REFRESH
```

This is the primary API integration flow for the MVP.

---

# 73. Next Step

After this document is reviewed and approved, create:

```text
PRD/
├── 01-product-definition.md
├── 02-ux-ui-specification.md
├── 03-technical-architecture.md
├── 04-database-design.md
└── 05-api-specification.md
```

Next:

> **Step 6 — AI Coach & Prompt Architecture**

Step 6 will define the actual intelligence layer of the product:

* AI Coach architecture
* System prompts
* Lesson-generation prompts
* Grammar correction
* Speaking evaluation
* Conversation behavior
* Mistake extraction
* Learner context
* AI memory
* Structured outputs
* Prompt versioning
* AI evaluation
* Cost control
* Fallback behavior
* Hallucination control
* CEFR-aware responses

The AI layer is the **core differentiator** of this product, so we should design it carefully before implementing it.
