Product Requirements Document — Step 04: Database Design

Version: 1.0
Status: Implementation Baseline
Database: PostgreSQL
ORM: Prisma

1. Purpose

This document defines the production database model for the German Language Coach MVP.

The database must support:

User accounts

Authentication/session data

Learner profiles

Learning goals

CEFR levels

Courses and curriculum

Modules and lessons

Exercises and questions

Vocabulary

Grammar concepts

Lesson progress

Exercise attempts

Mistake tracking

Spaced repetition/review

Daily activity

XP and levels

Streaks

Achievements

AI conversations

AI usage/quota tracking

Notifications

Admin/content management

Audit logging

The design should be simple enough for the MVP while allowing future expansion.

2. Database Principles

Use these principles:

PostgreSQL
     ↓
Prisma ORM
     ↓
Strong foreign keys
     ↓
Explicit relationships
     ↓
Indexes on frequent queries
     ↓
Transactions for multi-step updates

Additional rules:

Use UUIDs for externally visible identifiers.

Store timestamps in UTC.

Convert timestamps to the learner's timezone at the application layer.

Use database constraints for data integrity.

Do not store derived values when they can safely be calculated.

Add indexes based on actual query patterns.

Avoid hard deletion of important learning history.

Use status fields for content lifecycle where appropriate.

3. High-Level Entity Model

User
 ├── Profile
 ├── Learning Goals
 ├── Sessions
 ├── Lesson Progress
 ├── Exercise Attempts
 ├── Vocabulary Progress
 ├── Grammar Progress
 ├── Mistakes
 ├── Review Items
 ├── Daily Activity
 ├── XP Transactions
 ├── Streak
 ├── Achievements
 ├── Conversations
 ├── AI Usage
 ├── Notifications
 └── Audit Events

Course
 └── Level
      └── Module
           └── Lesson
                ├── Exercises
                ├── Vocabulary
                └── Grammar Concepts

4. User Model

users

Purpose: Core account record.

Fields:

id                  UUID PK
email               VARCHAR UNIQUE
password_hash       VARCHAR NULL
role                ENUM(USER, ADMIN)
status              ENUM(ACTIVE, SUSPENDED, DELETED)
email_verified_at   TIMESTAMP NULL
last_login_at       TIMESTAMP NULL
created_at          TIMESTAMP
updated_at          TIMESTAMP

Indexes:

email
status
created_at

Rules:

Email must be unique.

Email should be normalized to lowercase.

Passwords must only be stored as secure password hashes.

Never store plaintext passwords.

5. User Profile

user_profiles

Fields:

id                  UUID PK
user_id             UUID UNIQUE FK → users.id
display_name        VARCHAR
native_language     VARCHAR
target_language     VARCHAR DEFAULT 'de'
timezone            VARCHAR
avatar_url           VARCHAR NULL
created_at          TIMESTAMP
updated_at          TIMESTAMP

The profile is separated from authentication so account/security data and learner preferences remain independent.

6. Learning Goals

learning_goals

Fields:

id                  UUID PK
user_id             UUID FK → users.id
goal_type           ENUM
target_level        ENUM(A1, A2, B1, B2, C1, C2)
daily_minutes       INTEGER
target_date         DATE NULL
is_active           BOOLEAN
created_at          TIMESTAMP
updated_at          TIMESTAMP

Example goal types:

WORK
TRAVEL
EXAM
GENERAL
RELOCATION
STUDY

A user may have historical goals but normally only one active primary goal.

Index:

(user_id, is_active)

7. Authentication Sessions

sessions

Fields:

id                  UUID PK
user_id             UUID FK → users.id
token_hash          VARCHAR UNIQUE
expires_at          TIMESTAMP
created_at          TIMESTAMP
last_used_at        TIMESTAMP
revoked_at          TIMESTAMP NULL

Never store raw session tokens.

Indexes:

user_id
expires_at
token_hash

8. CEFR Levels

levels

Fields:

id                  UUID PK
code                ENUM(A1, A2, B1, B2, C1, C2) UNIQUE
name                VARCHAR
description         TEXT
sort_order          INTEGER
created_at          TIMESTAMP
updated_at          TIMESTAMP

For the MVP, focus on:

A1
A2

while keeping the schema ready for B1–C2.

9. Courses

courses

Fields:

id                  UUID PK
level_id            UUID FK → levels.id
title               VARCHAR
slug                VARCHAR UNIQUE
description         TEXT
status              ENUM(DRAFT, PUBLISHED, ARCHIVED)
sort_order          INTEGER
created_at          TIMESTAMP
updated_at          TIMESTAMP
published_at        TIMESTAMP NULL

Indexes:

level_id
status
(level_id, status)

10. Modules

modules

Fields:

id                  UUID PK
course_id           UUID FK → courses.id
title               VARCHAR
slug                VARCHAR
description         TEXT
sort_order          INTEGER
status              ENUM(DRAFT, PUBLISHED, ARCHIVED)
created_at          TIMESTAMP
updated_at          TIMESTAMP

Constraint:

UNIQUE(course_id, slug)

11. Lessons

lessons

Fields:

id                  UUID PK
module_id           UUID FK → modules.id
title               VARCHAR
slug                VARCHAR
description         TEXT
learning_objective  TEXT
estimated_minutes   INTEGER
difficulty           INTEGER
sort_order          INTEGER
status              ENUM(DRAFT, PUBLISHED, ARCHIVED)
created_at          TIMESTAMP
updated_at          TIMESTAMP
published_at        TIMESTAMP NULL

Constraints:

UNIQUE(module_id, slug)
difficulty: 1–5
estimated_minutes > 0

12. Exercises

exercises

Fields:

id                  UUID PK
lesson_id           UUID FK → lessons.id
type                ENUM
prompt              TEXT
instructions        TEXT NULL
difficulty           INTEGER
sort_order          INTEGER
points              INTEGER
metadata            JSONB NULL
status              ENUM(DRAFT, PUBLISHED, ARCHIVED)
created_at          TIMESTAMP
updated_at          TIMESTAMP

Exercise types:

MULTIPLE_CHOICE
FILL_BLANK
TRANSLATION
WORD_ORDER
LISTENING
READING
WRITING
SPEAKING
MATCHING

metadata can contain type-specific configuration.

Example:

{
  "options": ["bin", "bist", "ist"],
  "correctOption": "bin"
}

Do not put sensitive learner data into exercise metadata.

13. Exercise Answers

exercise_answers

Fields:

id                  UUID PK
exercise_id         UUID FK → exercises.id
answer_text         TEXT
is_correct          BOOLEAN
sort_order          INTEGER
created_at          TIMESTAMP

Useful primarily for deterministic exercises.

AI-evaluated exercises can instead store evaluation results in exercise_attempts.

14. Vocabulary

vocabulary_items

Fields:

id                  UUID PK
lesson_id           UUID NULL FK → lessons.id
word                VARCHAR
article              VARCHAR NULL
translation         VARCHAR
part_of_speech      VARCHAR NULL
plural              VARCHAR NULL
example_sentence    TEXT NULL
pronunciation       VARCHAR NULL
audio_url            VARCHAR NULL
level_code          ENUM(A1, A2, B1, B2, C1, C2)
created_at          TIMESTAMP
updated_at          TIMESTAMP

Indexes:

word
level_code
lesson_id

15. User Vocabulary Progress

user_vocabulary

Fields:

id                  UUID PK
user_id             UUID FK → users.id
vocabulary_id       UUID FK → vocabulary_items.id
status              ENUM(NEW, LEARNING, MASTERED)
confidence           INTEGER
correct_count       INTEGER
incorrect_count     INTEGER
last_reviewed_at    TIMESTAMP NULL
next_review_at      TIMESTAMP NULL
created_at          TIMESTAMP
updated_at          TIMESTAMP

Constraint:

UNIQUE(user_id, vocabulary_id)

16. Grammar Concepts

grammar_concepts

Fields:

id                  UUID PK
lesson_id            UUID NULL FK → lessons.id
title                VARCHAR
explanation          TEXT
example_german       TEXT
example_translation  TEXT NULL
level_code           ENUM(A1, A2, B1, B2, C1, C2)
created_at           TIMESTAMP
updated_at           TIMESTAMP

17. User Grammar Progress

user_grammar_progress

Fields:

id                  UUID PK
user_id             UUID FK → users.id
grammar_concept_id  UUID FK → grammar_concepts.id
status              ENUM(NEW, LEARNING, MASTERED)
confidence           INTEGER
correct_count       INTEGER
incorrect_count     INTEGER
last_practiced_at   TIMESTAMP NULL
created_at          TIMESTAMP
updated_at          TIMESTAMP

Constraint:

UNIQUE(user_id, grammar_concept_id)

18. Lesson Progress

lesson_progress

Purpose: Stores a learner's progress through a lesson.

Fields:

id                  UUID PK
user_id             UUID FK → users.id
lesson_id           UUID FK → lessons.id
status              ENUM(NOT_STARTED, IN_PROGRESS, COMPLETED)
progress_percent    INTEGER
started_at          TIMESTAMP NULL
completed_at        TIMESTAMP NULL
last_activity_at    TIMESTAMP
created_at          TIMESTAMP
updated_at          TIMESTAMP

Constraint:

UNIQUE(user_id, lesson_id)

Indexes:

user_id
(user_id, status)
lesson_id

19. Exercise Attempts

exercise_attempts

Fields:

id                  UUID PK
user_id             UUID FK → users.id
exercise_id         UUID FK → exercises.id
answer              TEXT NULL
is_correct          BOOLEAN NULL
score               DECIMAL NULL
feedback            TEXT NULL
evaluation_type     ENUM(RULE_BASED, AI, MANUAL)
attempt_number      INTEGER
response_metadata   JSONB NULL
created_at          TIMESTAMP

Indexes:

(user_id, exercise_id)
(user_id, created_at)
exercise_id

Do not overwrite previous attempts.

Attempts form the learner's learning history.

20. Mistakes

mistakes

Fields:

id                  UUID PK
user_id             UUID FK → users.id
exercise_id         UUID NULL FK → exercises.id
category            ENUM
source_text         TEXT
corrected_text      TEXT NULL
explanation         TEXT NULL
resolved             BOOLEAN
occurrence_count    INTEGER
last_occurred_at    TIMESTAMP
created_at          TIMESTAMP
updated_at          TIMESTAMP

Mistake categories:

GRAMMAR
VOCABULARY
SPELLING
WORD_ORDER
PRONUNCIATION
ARTICLE
PREPOSITION
OTHER

Indexes:

(user_id, resolved)
(user_id, category)
(user_id, last_occurred_at)

21. Review Items

review_items

Used for spaced repetition.

Fields:

id                  UUID PK
user_id             UUID FK → users.id
item_type           ENUM(VOCABULARY, GRAMMAR, EXERCISE, MISTAKE)
item_id             UUID
interval_days       INTEGER
ease_factor         DECIMAL
repetitions         INTEGER
next_review_at      TIMESTAMP
last_reviewed_at    TIMESTAMP NULL
created_at          TIMESTAMP
updated_at          TIMESTAMP

Indexes:

(user_id, next_review_at)
(user_id, item_type)

The application owns the scheduling algorithm.

22. Daily Activity

daily_activity

Purpose: One record per user per local calendar day.

Fields:

id                  UUID PK
user_id             UUID FK → users.id
activity_date       DATE
minutes_learned     INTEGER
lessons_completed   INTEGER
exercises_completed INTEGER
xp_earned           INTEGER
ai_messages         INTEGER
created_at          TIMESTAMP
updated_at          TIMESTAMP

Constraint:

UNIQUE(user_id, activity_date)

Important:

activity_date represents the learner's local date, not UTC date.

23. XP Transactions

xp_transactions

Do not store only a single XP total.

Keep an immutable-ish transaction history.

Fields:

id                  UUID PK
user_id             UUID FK → users.id
amount              INTEGER
source              ENUM
reference_id        UUID NULL
description         VARCHAR NULL
created_at          TIMESTAMP

XP sources:

LESSON_COMPLETION
EXERCISE_COMPLETION
DAILY_GOAL
STREAK
ACHIEVEMENT
REVIEW
BONUS

Indexes:

(user_id, created_at)

The current XP can be calculated from transactions or maintained as a cached value on the profile if performance later requires it.

24. User Gamification Profile

user_gamification

Fields:

id                  UUID PK
user_id             UUID UNIQUE FK → users.id
current_xp          INTEGER
current_level       INTEGER
current_streak      INTEGER
longest_streak      INTEGER
last_learning_date  DATE NULL
created_at          TIMESTAMP
updated_at          TIMESTAMP

This table stores frequently accessed aggregates.

The source of truth for XP history remains xp_transactions.

25. Achievements

achievements

Fields:

id                  UUID PK
code                VARCHAR UNIQUE
name                VARCHAR
description         TEXT
icon_url             VARCHAR NULL
criteria             JSONB
xp_reward           INTEGER
status              ENUM(ACTIVE, INACTIVE)
created_at          TIMESTAMP
updated_at          TIMESTAMP

Example criteria:

{
  "type": "STREAK",
  "days": 7
}

26. User Achievements

user_achievements

Fields:

id                  UUID PK
user_id             UUID FK → users.id
achievement_id      UUID FK → achievements.id
earned_at           TIMESTAMP

Constraint:

UNIQUE(user_id, achievement_id)

27. AI Conversations

conversations

Fields:

id                  UUID PK
user_id             UUID FK → users.id
title               VARCHAR NULL
mode                ENUM(
  GENERAL,
  GRAMMAR,
  VOCABULARY,
  TRANSLATION,
  SPEAKING,
  LESSON_HELP
)
level_code          ENUM(A1, A2, B1, B2, C1, C2)
created_at          TIMESTAMP
updated_at          TIMESTAMP

Indexes:

(user_id, updated_at)

28. AI Conversation Messages

conversation_messages

Fields:

id                  UUID PK
conversation_id    UUID FK → conversations.id
role                ENUM(USER, ASSISTANT, SYSTEM)
content             TEXT
provider             VARCHAR NULL
model                VARCHAR NULL
input_tokens        INTEGER NULL
output_tokens       INTEGER NULL
latency_ms          INTEGER NULL
created_at          TIMESTAMP

Indexes:

(conversation_id, created_at)

Privacy:

Do not expose system prompts to learners.

Do not store provider secrets.

Apply retention rules where required.

29. AI Usage

ai_usage

Purpose: Track usage and enforce quotas.

Fields:

id                  UUID PK
user_id             UUID FK → users.id
provider             VARCHAR
model                VARCHAR
request_type        VARCHAR
input_tokens        INTEGER NULL
output_tokens       INTEGER NULL
total_tokens        INTEGER NULL
success             BOOLEAN
latency_ms          INTEGER NULL
error_code          VARCHAR NULL
created_at          TIMESTAMP

Indexes:

(user_id, created_at)
(provider, created_at)

This supports:

Daily limits
Provider limits
Usage analytics
Cost estimation
Abuse detection

30. Notifications

notifications

Fields:

id                  UUID PK
user_id             UUID FK → users.id
type                ENUM
title               VARCHAR
message             TEXT
scheduled_at        TIMESTAMP NULL
sent_at             TIMESTAMP NULL
read_at             TIMESTAMP NULL
status              ENUM(PENDING, SENT, READ, FAILED, CANCELLED)
created_at          TIMESTAMP

Indexes:

(user_id, status)
(user_id, scheduled_at)

31. Notification Preferences

notification_preferences

Fields:

id                  UUID PK
user_id             UUID UNIQUE FK → users.id
daily_reminder      BOOLEAN
streak_reminder     BOOLEAN
achievement_alerts  BOOLEAN
weekly_summary      BOOLEAN
quiet_hours_start   TIME NULL
quiet_hours_end     TIME NULL
created_at          TIMESTAMP
updated_at          TIMESTAMP

32. Admin Audit Logs

audit_logs

Fields:

id                  UUID PK
actor_user_id       UUID NULL FK → users.id
action              VARCHAR
entity_type         VARCHAR
entity_id           UUID NULL
metadata            JSONB NULL
created_at          TIMESTAMP

Examples:

LESSON_CREATED
LESSON_UPDATED
LESSON_PUBLISHED
LESSON_ARCHIVED
USER_SUSPENDED
USER_REACTIVATED

Indexes:

(actor_user_id, created_at)
(entity_type, entity_id)
(created_at)

33. Content Versioning

For MVP, content can initially use:

status
created_at
updated_at
published_at

Full revision history can be introduced later if required.

If content editing becomes collaborative, add:

content_versions

rather than overcomplicating the initial schema.

34. Enumerations

Recommended Prisma enums:

UserRole
UserStatus
GoalType
LevelCode
ContentStatus
ExerciseType
LessonProgressStatus
EvaluationType
MistakeCategory
ReviewItemType
XPSource
ConversationMode
MessageRole
NotificationType
NotificationStatus
AchievementStatus

Keep enum names stable because they may become part of the API contract.

35. Relationships Summary

users
 ├── 1:1 user_profiles
 ├── 1:N learning_goals
 ├── 1:N sessions
 ├── 1:N lesson_progress
 ├── 1:N exercise_attempts
 ├── 1:N mistakes
 ├── 1:N review_items
 ├── 1:N daily_activity
 ├── 1:N xp_transactions
 ├── 1:1 user_gamification
 ├── 1:N user_achievements
 ├── 1:N conversations
 ├── 1:N ai_usage
 ├── 1:N notifications
 ├── 1:1 notification_preferences
 └── 1:N audit_logs

levels
 └── 1:N courses

courses
 └── 1:N modules

modules
 └── 1:N lessons

lessons
 ├── 1:N exercises
 ├── 1:N vocabulary_items
 └── 1:N grammar_concepts

exercises
 └── 1:N exercise_answers

conversations
 └── 1:N conversation_messages

36. Foreign Key Delete Strategy

Recommended:

User → Progress
RESTRICT / SOFT DELETE

User → Sessions
CASCADE

User → Notifications
CASCADE

User → AI Usage
RETAIN or controlled deletion

Course → Module
RESTRICT

Module → Lesson
RESTRICT

Lesson → Exercise
RESTRICT

Exercise → Attempts
RETAIN

Important learner history should not disappear merely because content is archived.

37. Soft Delete

Use status instead of physical deletion for:

Users
Courses
Modules
Lessons
Exercises
Achievements

Example:

ACTIVE
SUSPENDED
ARCHIVED
DELETED

Hard deletion should be restricted to data where it is safe and appropriate.

38. Timestamp Standard

All server timestamps:

UTC

Example:

created_at
updated_at
completed_at
last_activity_at

User-facing dates are converted using:

user_profiles.timezone

39. Indexing Strategy

Initial high-value indexes:

users.email

lesson_progress(user_id, status)
lesson_progress(user_id, lesson_id)

exercise_attempts(user_id, created_at)

user_vocabulary(user_id, vocabulary_id)
user_grammar_progress(user_id, grammar_concept_id)

daily_activity(user_id, activity_date)

xp_transactions(user_id, created_at)

conversations(user_id, updated_at)

conversation_messages(conversation_id, created_at)

ai_usage(user_id, created_at)

notifications(user_id, status)
notifications(user_id, scheduled_at)

Do not create excessive indexes before query patterns are known.

40. Transactions

Use database transactions for operations such as:

Complete lesson
  ├── update lesson progress
  ├── record activity
  ├── award XP
  └── update streak

These changes should either all succeed or be safely recoverable.

41. Idempotency

Important operations must be idempotent.

Examples:

Lesson completion
XP award
Achievement award
Notification creation

A repeated request must not create duplicate rewards.

For example:

Complete Lesson
      ↓
XP transaction reference
      ↓
Check existing transaction
      ↓
Already exists?
   YES → do not award again
   NO  → award XP

42. Data Integrity Constraints

Database should enforce:

Unique email
Unique user/profile
Unique user/lesson progress
Unique user/vocabulary
Unique user/grammar
Unique user/day
Unique user/achievement
Unique course/module slug
Unique module/lesson slug

Also validate numeric fields:

progress: 0–100
confidence: 0–100
difficulty: 1–5
XP: appropriate integer

43. JSONB Usage

Use JSONB only for flexible data:

exercise metadata
achievement criteria
AI response metadata
audit metadata

Do not use JSONB for core relational data that needs:

Foreign keys
Indexes
Frequent querying
Strong constraints

44. Privacy

User-specific data includes:

Learning progress
Mistakes
AI conversations
AI usage
Profile information

Application authorization must always scope queries by authenticated user.

Never trust a user-supplied user_id alone.

Use:

authenticated_user_id
+
resource ownership check

45. AI Conversation Retention

The application should eventually support configurable retention.

For MVP:

Keep conversations while account is active

Future:

User deletes conversation
User exports conversation
Account deletion removes/anonymizes conversation data

Retention requirements must be reviewed before production launch in each target jurisdiction.

46. Seed Data

The development database should include:

A1 level
A2 level

German A1 course

Modules:
- Greetings & Introductions
- Numbers & Time
- Family
- Daily Routine
- Food & Shopping
- Travel Basics

Example lessons
Example exercises
Example vocabulary
Example grammar
Example achievements

Production seed data should be explicitly controlled.

47. Initial A1 Content Example

Example:

Course:
Deutsch A1

Module:
Greetings & Introductions

Lesson:
Introducing Yourself

Vocabulary:
ich
du
heißen
kommen
wohnen

Grammar:
Personal pronouns
Verb "sein"
Verb conjugation basics

Exercises:
Multiple choice
Fill blank
Translation
Word order

48. Prisma Schema Organization

Start with one Prisma schema:

prisma/
└── schema.prisma

For larger scale, Prisma models can be organized through generation tooling if required.

Do not prematurely split the database into multiple services/databases.

49. Migration Strategy

Use Prisma migrations:

schema change
    ↓
prisma migrate dev
    ↓
review migration
    ↓
test
    ↓
commit migration
    ↓
staging
    ↓
production

Never manually modify production schema.

50. Database Backup

Production PostgreSQL must have:

Automated backups
Restore capability
Backup monitoring

A backup is only considered valid after restore testing.

51. MVP Database Scope

For the first implementation, prioritize:

users
user_profiles
sessions
levels
courses
modules
lessons
exercises
exercise_answers
vocabulary_items
grammar_concepts
lesson_progress
exercise_attempts
daily_activity
xp_transactions
user_gamification
achievements
user_achievements
conversations
conversation_messages
ai_usage

Implement later if needed:

mistakes
review_items
notifications
notification_preferences
audit_logs

However, their schema should remain compatible with the full model above.

52. First Vertical Slice Database

The absolute minimum working flow is:

User
 ↓
Profile
 ↓
Course
 ↓
Level
 ↓
Module
 ↓
Lesson
 ↓
Exercise
 ↓
Attempt
 ↓
Lesson Progress
 ↓
Daily Activity
 ↓
XP Transaction
 ↓
Gamification Profile

This should be implemented before building advanced analytics.

53. Database Acceptance Criteria

The database design is ready for implementation when:

[ ] All core entities defined
[ ] Primary keys defined
[ ] Foreign keys defined
[ ] Unique constraints defined
[ ] Important indexes defined
[ ] Enums defined
[ ] Delete strategy defined
[ ] Timestamp strategy defined
[ ] User ownership strategy defined
[ ] Transaction boundaries identified
[ ] Idempotency strategy identified
[ ] Seed data defined
[ ] Migration strategy defined
[ ] Backup strategy defined

54. Recommended Initial Implementation Order

Implement Prisma models in this order:

1. User / Profile / Session
2. Level / Course / Module / Lesson
3. Exercise / Exercise Answer
4. Lesson Progress
5. Exercise Attempts
6. Daily Activity
7. XP Transactions
8. User Gamification
9. Vocabulary
10. Grammar
11. AI Conversations
12. AI Usage
13. Achievements
14. Notifications
15. Mistakes / Review Items
16. Audit Logs

55. Database Design Rule

The database stores facts and history; application services decide behavior.

Examples:

Database:
"User completed exercise."

Service:
"Therefore award 10 XP."

Database:
"User has 7-day streak."

Service:
"Therefore unlock the 7-day achievement."

Do not put complex learning/business rules into PostgreSQL triggers unless there is a strong reason.

56. Final MVP Data Flow

                  USER
                   │
                   ▼
              AUTHENTICATION
                   │
                   ▼
                PROFILE
                   │
                   ▼
              LEARNING GOAL
                   │
                   ▼
                 COURSE
                   │
                   ▼
                 MODULE
                   │
                   ▼
                 LESSON
                   │
          ┌────────┴────────┐
          ▼                 ▼
      EXERCISES          CONTENT
          │            ┌────┴────┐
          ▼            ▼         ▼
       ATTEMPT      VOCABULARY  GRAMMAR
          │
          ▼
       PROGRESS
          │
     ┌────┼─────┐
     ▼    ▼     ▼
    XP  STREAK  DAILY ACTIVITY
     │
     ▼
 ACHIEVEMENTS

USER
 │
 ▼
AI COACH
 │
 ▼
CONVERSATION
 │
 ▼
AI USAGE

57. Next Implementation Step

After this database design is accepted, do not immediately implement every table.

Start with:

Phase 1A
───────
Create Next.js/TypeScript application
        ↓
Install Prisma
        ↓
Configure PostgreSQL
        ↓
Create initial migration
        ↓
Implement:
  users
  user_profiles
  sessions
  levels
  courses
  modules
  lessons
  exercises
        ↓
Seed A1 content
        ↓
Run application
        ↓
Verify database

Then build authentication and the first learning vertical slice.

58. Definition of Done

The database foundation is complete when:

[ ] PostgreSQL running
[ ] Prisma configured
[ ] Schema created
[ ] Migration succeeds from empty database
[ ] Seed script succeeds
[ ] Foreign keys work
[ ] Unique constraints work
[ ] Basic indexes created
[ ] Local database can be reset
[ ] Test database can be created
[ ] Application can read A1 course data
[ ] Application can create a learner
[ ] Application can save lesson progress
[ ] Application can record an exercise attempt
[ ] Application can record XP

59. Final Principle

Start with a stable relational core, keep business logic in application services, preserve learner history, and design the schema so the MVP can grow to a full German learning platform without requiring a database rewrite
