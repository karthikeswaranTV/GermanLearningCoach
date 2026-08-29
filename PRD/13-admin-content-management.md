German Language Coach

Product Requirements Document — Step 13: Admin & Content Management

Version: 0.1
Status: Draft

1. Purpose

The Admin & Content Management system allows authorized administrators to manage the German learning content and monitor the health of the application.

The MVP goal is:

Give administrators a simple, secure workspace to manage curriculum, lessons, exercises, vocabulary, grammar content, users, and operational issues without requiring direct database changes.

2. Core Principle

Admin functionality must be:

Secure
Role-based
Auditable
Simple
Separated from learner UI

Only authorized users may access admin functionality.

3. Admin Scope

MVP:

[ ] Admin authentication
[ ] Admin dashboard
[ ] User management
[ ] Curriculum management
[ ] Lesson management
[ ] Exercise management
[ ] Vocabulary management
[ ] Grammar content management
[ ] Content publishing
[ ] Content version/status
[ ] Basic reports
[ ] Audit logging

Future:

[ ] Content workflow
[ ] Multiple content editors
[ ] Review/approval workflow
[ ] AI-assisted content generation
[ ] Advanced analytics
[ ] A/B testing

4. Admin Roles

MVP:

ADMIN

Future:

CONTENT_EDITOR
CONTENT_REVIEWER
MODERATOR
SUPPORT
SUPER_ADMIN

Do not add roles unless they provide a real permission boundary.

5. Authorization

Admin access must be enforced on the backend.

Architecture:

Admin User
    ↓
Authentication
    ↓
Role Check
    ↓
Permission Check
    ↓
Admin API

Frontend route protection alone is insufficient.

6. Admin Dashboard

The dashboard should provide a concise operational overview.

Example:

Admin Dashboard

Users
12,420

Active Today
1,842

Lessons
128

Exercises
1,450

AI Requests Today
8,230

Content Drafts
14

System Alerts
2

The dashboard should prioritize actionable information rather than decorative metrics.

7. Admin Navigation

Recommended:

Dashboard
Users
Curriculum
Lessons
Exercises
Vocabulary
Grammar
Achievements
Reports
Audit Log
Settings

Keep navigation modular so future sections can be added without redesigning the whole application.

8. User Management

Admins should be able to:

Search users
View user
View account status
View registration date
View last login
Suspend user
Reactivate user

MVP should avoid exposing unnecessary private learner information.

9. User Search

Support:

Email
Display name
User ID
Status

Optional filters:

Active
Suspended
Unverified
Recently registered

Use pagination.

Do not load the entire user database into the browser.

10. User Detail

Example:

User
-------------------------
Name
Email
Status
Created
Last login
Email verified

Learning
-------------------------
Level
Goal
Daily goal
Current streak

Usage
-------------------------
AI requests
Lessons completed

Only show data necessary for administration.

11. Suspend User

Flow:

Admin
 ↓
User profile
 ↓
Suspend
 ↓
Confirmation
 ↓
Reason
 ↓
Account suspended

Suspension should invalidate active sessions where appropriate.

12. Reactivate User

Flow:

Suspended user
      ↓
Admin selects Reactivate
      ↓
Confirmation
      ↓
Account becomes ACTIVE

13. Admin User Safety

Never allow an ordinary USER to call admin APIs.

Examples:

GET /api/admin/users
POST /api/admin/users/:id/suspend
POST /api/admin/content

must enforce authorization server-side.

14. Curriculum Management

Admin should manage:

CEFR level
Course
Module
Lesson
Exercise

Conceptually:

German Course
    ↓
A1
    ↓
Module 1
    ↓
Lesson 1
    ↓
Exercises

15. Curriculum Hierarchy

Recommended:

Course
 └── Level
      └── Module
           └── Lesson
                ├── Vocabulary
                ├── Grammar
                └── Exercises

This should align with the Learning Engine defined in Step 7.

16. Lesson Management

Admin should be able to:

Create lesson
Edit lesson
Preview lesson
Save draft
Publish lesson
Unpublish lesson
Archive lesson

A lesson should have:

title
description
CEFR level
module
learning objectives
estimated duration
content
status
version

17. Lesson Status

Recommended:

DRAFT
IN_REVIEW
PUBLISHED
ARCHIVED

MVP may use:

DRAFT
PUBLISHED
ARCHIVED

if a review workflow is not yet required.

18. Content Publishing

Never publish unfinished content accidentally.

Flow:

Draft
  ↓
Preview
  ↓
Validate
  ↓
Publish
  ↓
Visible to learners

Published content should have a clear version.

19. Unpublishing

Admins should be able to remove content from new learner recommendations without deleting historical records.

Prefer:

UNPUBLISH

or:

ARCHIVE

rather than destructive deletion.

20. Content Versioning

Content should support versions.

Example:

Lesson 12
Version 1
Version 2
Version 3

When content is changed, preserve enough history to understand what was previously published.

21. Exercise Management

Admin should be able to create:

Multiple choice
Fill in the blank
Translation
Sentence ordering
Vocabulary recall
Grammar correction
Listening
Speaking prompt
Conversation

The exercise type should align with the Learning Engine.

22. Exercise Fields

Recommended:

id
lessonId
type
prompt
instructions
difficulty
correctAnswer
options
explanation
xpReward
status
version
createdAt
updatedAt

Do not store sensitive information in exercise content.

23. Exercise Validation

Before publishing:

Prompt exists
Answer exists
Options valid where applicable
Correct answer matches available options
CEFR level valid
XP reward valid
Referenced lesson exists

24. Multiple Choice Validation

Example:

Question:
Ich ___ Deutsch.

Options:
A. lerne
B. lernen
C. lernst

Correct:
A

Validation must ensure the correct answer exists in the option list.

25. Fill-in-the-Blank Validation

Example:

Ich ___ in Chennai.

Answer:
wohne

Support:

Exact answer
Accepted alternatives
Case sensitivity policy

26. Translation Exercise

Example:

English:
I work in Germany.

Expected:
Ich arbeite in Deutschland.

The system should support acceptable alternative answers where practical.

AI-based evaluation may be used later.

27. Vocabulary Management

Admin should manage:

German word
Translation
Part of speech
Article
Plural
Example sentence
CEFR level
Topic
Pronunciation

Example:

der Beruf
job / profession
Noun
Plural: die Berufe
A1
Topic: Work

28. Grammar Management

Admin should manage:

Grammar topic
CEFR level
Explanation
Examples
Common mistakes
Exercises

Examples:

Articles
Cases
Verb conjugation
Word order
Modal verbs
Separable verbs

29. Content Tags

Use structured tags.

Examples:

A1
WORK
TRAVEL
GRAMMAR
VOCABULARY
LISTENING
SPEAKING

Tags support search and personalization.

30. Search

Admin content search should support:

Title
Keyword
CEFR level
Content type
Status
Tag

Results should be paginated.

31. Bulk Operations

Future but useful:

Bulk publish
Bulk archive
Bulk tag
Bulk export
Bulk import

Do not implement bulk destructive operations without confirmation.

32. Content Import

Future:

CSV
JSON
Markdown

Potential use:

Existing curriculum
Vocabulary lists
Exercise banks

All imported content must pass validation before publishing.

33. Content Export

Future:

Export curriculum
Export vocabulary
Export exercises

Useful for:

Backup
Migration
Review
Version control

34. Preview Mode

Admin should be able to preview content exactly as a learner would see it.

Example:

Edit Lesson
    ↓
Preview
    ↓
Learner View

This reduces publishing errors.

35. Content Quality Checks

Before publishing, validate:

Required fields
Broken references
Invalid CEFR level
Missing answers
Duplicate identifiers
Malformed structured data

36. AI-Assisted Content

Future feature:

Admin enters topic
      ↓
AI suggests vocabulary
      ↓
AI suggests exercises
      ↓
Admin reviews
      ↓
Admin edits
      ↓
Admin publishes

Important:

AI-generated content must never automatically become published learning content.

Human review is required.

37. Content Safety

Admin content must be reviewed for:

Incorrect German
Incorrect translations
Offensive content
Cultural inaccuracies
Ambiguous questions
Invalid answer keys

The product should favor correctness over content volume.

38. Achievements Management

Admin should eventually manage:

Achievement name
Description
Icon
Trigger
XP reward
Status

Example:

7 Day Streak
Complete learning for 7 consecutive days.

Achievement definitions should align with Step 8.

39. Reports

MVP reports:

New users
Active users
Lessons completed
Exercise completion
Content usage
AI usage

Future:

Retention
Learning effectiveness
Weak curriculum areas
Drop-off points

40. Content Analytics

Useful metrics:

Lesson views
Lesson starts
Lesson completions
Exercise attempts
Exercise success rate
Average time
Drop-off rate

This can identify poorly designed lessons.

41. Exercise Analytics

For each exercise:

Attempts
Correct rate
Incorrect rate
Average attempts
Average time

Example:

Exercise: German articles

Attempts: 2,450
Correct: 61%
Incorrect: 39%

Low-performing exercises should be reviewed.

42. Audit Log

Record sensitive admin actions.

Examples:

ADMIN_LOGIN
USER_SUSPENDED
USER_REACTIVATED
LESSON_CREATED
LESSON_UPDATED
LESSON_PUBLISHED
LESSON_ARCHIVED
EXERCISE_UPDATED
ROLE_CHANGED

43. Audit Log Fields

id
adminUserId
action
resourceType
resourceId
metadata
createdAt

Do not store secrets or unnecessary personal data in audit metadata.

44. Audit Log Immutability

Admins should not normally be able to modify historical audit records.

Use append-only behavior where practical.

45. Admin APIs

Recommended:

GET  /api/admin/dashboard

GET  /api/admin/users
GET  /api/admin/users/:id
POST /api/admin/users/:id/suspend
POST /api/admin/users/:id/reactivate

GET  /api/admin/courses
POST /api/admin/courses
PUT  /api/admin/courses/:id

GET  /api/admin/lessons
POST /api/admin/lessons
GET  /api/admin/lessons/:id
PUT  /api/admin/lessons/:id
POST /api/admin/lessons/:id/publish
POST /api/admin/lessons/:id/archive

GET  /api/admin/exercises
POST /api/admin/exercises
PUT  /api/admin/exercises/:id

GET  /api/admin/vocabulary
POST /api/admin/vocabulary
PUT  /api/admin/vocabulary/:id

GET  /api/admin/grammar
POST /api/admin/grammar
PUT  /api/admin/grammar/:id

GET /api/admin/audit-log

Adapt endpoints to the actual API architecture from Step 5.

46. Admin Frontend Structure

Suggested:

/admin
/admin/dashboard
/admin/users
/admin/curriculum
/admin/curriculum/lessons
/admin/curriculum/exercises
/admin/content/vocabulary
/admin/content/grammar
/admin/reports
/admin/audit-log
/admin/settings

47. Admin UI Principles

The admin UI should prioritize:

Clarity
Density
Search
Tables
Filters
Fast editing
Safe actions
Clear status

Do not use the learner's gamified UI for the admin panel.

48. Tables

User/content tables should support:

Search
Sort
Filter
Pagination
Row actions
Bulk selection where appropriate

Example:

Lesson       Level   Status     Updated
Greetings    A1      Published  Today
Articles     A1      Draft      Yesterday
Workplace    A2      Published  Aug 20

49. Forms

Admin forms should:

Validate immediately
Show required fields
Preserve unsaved input
Warn before destructive actions
Support preview

50. Draft Autosave

Future enhancement:

Admin editing
     ↓
Autosave draft

This reduces accidental content loss.

Not required for MVP.

51. Concurrency

If two admins edit the same content:

Admin A opens lesson
Admin B updates lesson
Admin A tries to save

The system should detect stale versions where practical.

Possible solution:

version number
updatedAt check
optimistic locking

52. Content IDs

Use stable IDs.

Do not use titles as identifiers.

Example:

lessonId = lesson_01H...

A title may change without breaking references.

53. Content References

When one item references another:

Lesson → Exercise
Lesson → Vocabulary
Lesson → Grammar

Validate that referenced objects exist.

Avoid dangling references.

54. Deletion Strategy

Prefer:

Archive

over permanent deletion for published educational content.

Reasons:

Historical progress
Analytics
Existing user sessions
Content references
Audit history

55. Admin Security

Required:

Strong authentication
Role-based authorization
Secure sessions
Rate limiting
Audit logs
CSRF protection where applicable
Input validation
Output encoding

Sensitive admin actions may require re-authentication.

56. Admin Account Protection

Future:

MFA
Passkeys
IP restrictions
Admin session management
Login alerts

At minimum, protect admin credentials strongly.

57. Admin API Rate Limits

Rate-limit sensitive operations:

Login
Bulk operations
User actions
Content publishing
Import
Export

Do not allow unlimited automated requests.

58. Testing — Authorization

Verify:

USER → denied
ADMIN → allowed
Suspended admin → denied
Unauthenticated → denied

59. Testing — User Management

Test:

Search user
View user
Suspend user
Reactivate user
Invalid user ID
Unauthorized access
Pagination

60. Testing — Content

Test:

Create lesson
Edit lesson
Save draft
Preview
Publish
Archive
Invalid content
Broken reference
Version conflict

61. Testing — Exercises

Test:

Create exercise
Validate answer
Edit exercise
Publish
Archive
Invalid options
Missing correct answer

62. Testing — Audit Logs

Verify:

Sensitive action creates audit event
Correct admin ID recorded
Correct resource recorded
Audit event cannot be silently modified
Secrets are not recorded

63. MVP Data Entities

Recommended:

Course
Level
Module
Lesson
Exercise
VocabularyItem
GrammarTopic
Achievement
ContentVersion
AuditLog

Existing entities from previous PRDs should be reused rather than duplicated.

64. Suggested Project Structure

backend/
└── src/
    └── admin/
        ├── AdminController
        ├── AdminService
        ├── UserAdminService
        ├── CurriculumAdminService
        ├── ContentAdminService
        ├── ReportService
        ├── AuditLogService
        └── validators/

Frontend:

frontend/
└── admin/
    ├── dashboard/
    ├── users/
    ├── curriculum/
    ├── exercises/
    ├── vocabulary/
    ├── grammar/
    ├── reports/
    └── audit-log/

Adapt structure to the selected framework.

65. Implementation Order

1. Admin role/authorization
2. Admin route protection
3. Admin dashboard
4. User management
5. Curriculum management
6. Lesson management
7. Exercise management
8. Vocabulary management
9. Grammar management
10. Publishing/versioning
11. Audit logs
12. Reports
13. Testing

66. MVP Scope

Must have:

[ ] ADMIN role
[ ] Admin authorization
[ ] Admin dashboard
[ ] User search
[ ] User detail
[ ] Suspend/reactivate
[ ] Course management
[ ] Lesson management
[ ] Exercise management
[ ] Vocabulary management
[ ] Grammar management
[ ] Draft/publish/archive
[ ] Basic versioning
[ ] Audit logging
[ ] Basic reports

Not required:

[ ] Multiple editor roles
[ ] AI content generation
[ ] Advanced analytics
[ ] Bulk import/export
[ ] Autosave
[ ] A/B testing
[ ] MFA

67. Definition of Done

Admin & Content Management is complete when:

[ ] Only authorized admins can access admin APIs
[ ] Admin dashboard works
[ ] Users can be searched
[ ] Users can be suspended/reactivated
[ ] Curriculum can be managed
[ ] Lessons can be created/edited
[ ] Exercises can be created/edited
[ ] Vocabulary can be managed
[ ] Grammar topics can be managed
[ ] Content can be drafted
[ ] Content can be published
[ ] Content can be archived
[ ] Content versions are traceable
[ ] Admin actions are audited
[ ] Basic reports work
[ ] Security tests pass

68. Final Architecture

                         ADMIN
                           │
                           ▼
                    ADMIN FRONTEND
                           │
                           ▼
                  AUTHENTICATION
                           │
                           ▼
                  ROLE / PERMISSION
                           │
                           ▼
                    ADMIN API
                           │
          ┌────────────────┼────────────────┐
          ▼                ▼                ▼
       USERS          CONTENT          REPORTS
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
           Lessons      Exercises   Vocabulary/
                                      Grammar
              │            │            │
              └────────────┼────────────┘
                           ▼
                     PUBLISHING
                           │
                           ▼
                    LEARNING ENGINE
                           │
                           ▼
                         USERS

69. Key Architectural Rule

Admins manage the content and system; learners consume the learning experience.

Do not allow admin functionality to become tightly coupled to learner-facing UI components.

70. Product Outcome

The administrator should be able to:

Open Admin
    ↓
See system status
    ↓
Find a lesson
    ↓
Edit content
    ↓
Preview
    ↓
Publish
    ↓
Monitor learner usage
    ↓
Improve curriculum

The ultimate goal is:

Make curriculum and product operations manageable without developers having to edit the database manually
