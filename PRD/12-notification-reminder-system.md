German Language Coach

Product Requirements Document — Step 12: Notification & Reminder System

Version: 0.1
Status: Draft

1. Purpose

The Notification & Reminder System helps learners return to the German Language Coach consistently without becoming annoying.

The MVP goal is:

Remind the learner at useful times, reinforce learning habits, and protect streaks while giving the user full control over notifications.

The system should support multiple notification channels in the future, while keeping the MVP simple.

2. Core Principle

Notifications should support learning, not distract from it.

Use notifications for:

Daily learning reminder
Streak protection
Goal completion
Achievement
Weekly progress

Avoid unnecessary notifications.

3. MVP Scope

Must have:

[ ] Notification preferences
[ ] Daily learning reminder
[ ] Streak reminder
[ ] Goal completion notification
[ ] Basic notification history
[ ] Quiet hours
[ ] Timezone-aware scheduling
[ ] Disable all notifications

Future:

[ ] Push notifications
[ ] Email notifications
[ ] WhatsApp notifications
[ ] Personalized reminder timing
[ ] AI-generated reminder messages

4. Notification Channels

Design the system around channels:

IN_APP
EMAIL
PUSH

MVP should prioritize:

IN_APP

and implement the backend abstraction so email/push can be added later.

5. Notification Architecture

Learning Event
      ↓
Notification Service
      ↓
Preference Check
      ↓
Quiet Hours Check
      ↓
Deduplication
      ↓
Channel Selection
      ↓
Notification Provider
      ↓
User

6. Notification Service

Create a centralized:

NotificationService

Responsibilities:

Create notification
Check preferences
Check quiet hours
Schedule notification
Send notification
Record delivery
Prevent duplicates

Learning modules should not directly send notifications.

7. Notification Types

MVP:

DAILY_REMINDER
STREAK_REMINDER
GOAL_COMPLETED
ACHIEVEMENT_UNLOCKED
WEEKLY_PROGRESS

Future:

LESSON_RECOMMENDATION
REVIEW_DUE
LEVEL_UP
PERSONAL_MILESTONE
AI_COACH_FOLLOWUP

8. Daily Reminder

Purpose:

Remind the learner to complete today's learning goal.

Example:

Ready for today's German practice?

You have a 20-minute goal waiting.

The reminder should link directly to the appropriate learning screen.

9. Daily Reminder Timing

The learner should choose:

Preferred reminder time

Example:

07:30
18:00
20:30

Store the timezone separately.

Do not assume server timezone.

10. Default Reminder

Recommended default:

20:00 local time

The user must be able to change or disable it.

11. Streak Reminder

Purpose:

Help the learner protect an active streak.

Example:

Your German streak is at risk 🔥

Complete today's lesson to keep it going.

Only send this when:

User has an active streak
AND
Today's learning goal is incomplete
AND
The reminder is within the user's allowed notification window

12. Avoid Notification Spam

Never send multiple reminders for the same event unnecessarily.

Example:

Daily reminder sent
      ↓
User still hasn't learned
      ↓
Optional streak reminder

Do not repeatedly remind every hour.

13. Goal Completion

When the learner completes their daily goal:

Daily goal complete 🎉

You completed 20 minutes of German today.

This should be optional.

Some users may prefer fewer notifications.

14. Achievement Notification

When an achievement is unlocked:

Achievement unlocked!

You completed your first 7-day German streak.

Only send when the achievement is newly unlocked.

15. Weekly Progress

Future/optional MVP notification:

Your German week

5 days practiced
112 minutes learned
34 vocabulary items reviewed

This should be sent at a configurable weekly time.

16. Notification Preferences

Recommended settings:

dailyReminderEnabled
streakReminderEnabled
goalCompletionEnabled
achievementEnabled
weeklyProgressEnabled

Global:

notificationsEnabled

If global notifications are disabled, no optional notifications should be sent.

17. Quiet Hours

Users should be able to define:

Quiet hours start
Quiet hours end

Example:

22:00 → 07:00

No non-critical notifications should be delivered during quiet hours.

18. Critical vs Optional

Notification priority:

NORMAL
HIGH

MVP learning notifications should normally be:

NORMAL

Do not create unnecessary "critical" notifications.

19. Timezone Handling

Store:

timezone = Asia/Kolkata

Scheduling should calculate local user time.

Example:

User timezone:
Asia/Kolkata

Reminder:
20:00 local time

The server should convert this appropriately.

20. Daylight Saving Time

Use IANA timezone identifiers:

Europe/Berlin
America/New_York
Asia/Kolkata

Do not store only:

UTC+5:30

IANA zones correctly represent daylight-saving changes where applicable.

21. Notification Lifecycle

CREATED
   ↓
SCHEDULED
   ↓
SENT
   ↓
DELIVERED
   ↓
READ

Possible failure:

FAILED

22. Notification Entity

Recommended:

Notification
------------
id
userId
type
title
body
priority
status
scheduledAt
sentAt
readAt
createdAt

23. Notification Preferences Entity

Example:

NotificationPreference
----------------------
id
userId
notificationsEnabled
dailyReminderEnabled
streakReminderEnabled
goalCompletionEnabled
achievementEnabled
weeklyProgressEnabled
preferredReminderTime
timezone
quietHoursStart
quietHoursEnd
createdAt
updatedAt

24. Notification Delivery

Future entity:

NotificationDelivery
--------------------
id
notificationId
channel
status
provider
sentAt
deliveredAt
failureReason

This allows multiple delivery channels.

25. In-App Notifications

MVP should support a simple notification center.

Example:

🔔 Notifications

Today
------------------
🔥 Your streak is at risk
   Complete today's lesson

🎉 Daily goal completed
   Great work!

Yesterday
------------------
🏆 Achievement unlocked

26. Unread Count

The frontend should show:

🔔 3

The backend should provide:

GET /api/notifications/unread-count

27. Notification APIs

Recommended:

GET  /api/notifications
GET  /api/notifications/unread-count
POST /api/notifications/:id/read
POST /api/notifications/read-all

Preferences:

GET /api/notification-preferences
PUT /api/notification-preferences

28. Notification Deep Links

Notifications should open the relevant screen.

Examples:

DAILY_REMINDER
→ /learn/today

STREAK_REMINDER
→ /learn/today

ACHIEVEMENT_UNLOCKED
→ /progress/achievements

WEEKLY_PROGRESS
→ /progress

Do not send users to a generic homepage when a specific destination exists.

29. Event-Driven Design

Learning systems should publish events.

Examples:

DAILY_GOAL_COMPLETED
STREAK_AT_RISK
ACHIEVEMENT_UNLOCKED
WEEK_COMPLETED

Notification service consumes these events.

Architecture:

Learning Engine
      ↓
Domain Event
      ↓
Notification Service
      ↓
Notification

30. Daily Reminder Job

A scheduled worker should periodically determine who needs a reminder.

Example:

Scheduler
   ↓
Find eligible users
   ↓
Check timezone
   ↓
Check local time
   ↓
Check learning progress
   ↓
Check preferences
   ↓
Create notification

Do not create duplicate reminders.

31. Streak Reminder Job

Example:

Scheduler
   ↓
Find active streak users
   ↓
Check today's progress
   ↓
Check reminder policy
   ↓
Create notification

32. Idempotency

Notification jobs must be safe to run more than once.

Example:

Job runs
 ↓
Notification already exists
 ↓
Do nothing

Use a deterministic business key where appropriate.

Example:

userId + notificationType + localDate

33. Duplicate Prevention

For daily reminders:

One DAILY_REMINDER per user per local day

For goal completion:

One GOAL_COMPLETED per completed goal

For achievements:

One notification per achievement unlock

34. Notification Cleanup

Old notifications should not grow forever.

Recommended policy:

Keep recent notifications
Archive/delete older notifications

Retention should be configurable.

35. Notification Preferences UI

Suggested settings screen:

Notifications

Daily learning reminder      [ ON ]
Streak reminders             [ ON ]
Goal completion              [ ON ]
Achievements                 [ ON ]
Weekly progress              [ ON ]

Reminder time
[ 20:00 ]

Quiet hours
[ 22:00 ] to [ 07:00 ]

36. Permission UX

If browser/mobile notification permission is required:

User enables notifications
       ↓
Explain benefit
       ↓
Request platform permission
       ↓
Permission granted/denied

Do not request permission immediately on first app load without context.

37. Permission States

Track:

NOT_REQUESTED
GRANTED
DENIED

The application should guide the user to settings if permission was previously denied.

38. Push Notifications

Future architecture:

NotificationService
       ↓
PushProvider
       ↓
FCM / APNs / Web Push

The application should not hard-code one platform.

39. Email Notifications

Future:

NotificationService
       ↓
EmailProvider
       ↓
Email service

Email templates should be versioned and separate from application logic.

40. Free-First Notification Strategy

For MVP, minimize external notification costs.

Recommended:

In-app notifications
+
Browser push where free

Email can be added later if a free/low-cost provider is selected.

Provider pricing and free quotas must be verified before production.

41. Notification Content

Notifications should be:

Short
Useful
Action-oriented
Positive
Non-judgmental

Avoid:

Guilt
Fear
Excessive urgency
Spam
Manipulative wording

42. Personalization

MVP can personalize using:

Name
Current streak
Daily goal
Learning progress

Example:

Karthik, your 7-day streak is waiting.
Complete today's German practice.

Only use personal data that is appropriate and available.

43. AI-Generated Notifications

Do not use AI to generate routine notifications in MVP.

Reasons:

Unnecessary AI cost
Unpredictable output
Latency
Potential inappropriate wording

Use predefined templates first.

44. Notification Templates

Example:

DAILY_REMINDER:

Title:
Zeit für Deutsch 🇩🇪

Body:
Your daily German practice is waiting.

Templates should support localization later.

45. Localization

MVP:

English UI
German learning content

Future notification languages:

English
German
Tamil
Hindi

Notification templates should be designed for localization from the beginning.

46. Notification Analytics

Track:

notificationsCreated
notificationsSent
notificationsDelivered
notificationsRead
notificationClickRate
disabledNotifications

For privacy, collect only the analytics necessary for product improvement.

47. Notification Effectiveness

Measure:

Reminder sent
       ↓
User opens app
       ↓
User completes learning

The most important metric is not notification volume.

It is:

Learning sessions successfully started after a reminder.

48. Anti-Spam Rules

Recommended initial rules:

Maximum 2 learning reminders/day
Minimum gap between reminders
No reminders during quiet hours
No reminder after daily goal completion
Respect disabled preferences

Exact values should be configurable.

49. Failure Handling

If notification delivery fails:

Record failure
Retry where appropriate
Do not repeatedly retry indefinitely

In-app notification creation should remain independent from an external push/email provider failure.

50. Testing — Preferences

Test:

Enable notification
Disable notification
Disable all
Enable individual types
Change reminder time
Change timezone
Configure quiet hours

51. Testing — Scheduling

Test:

Correct local time
Timezone conversion
DST timezone
Midnight boundary
Quiet hours
Daily duplicate prevention

52. Testing — Events

Test:

Goal completed → notification
Achievement unlocked → notification
Streak at risk → notification
Duplicate event → no duplicate notification

53. Testing — Delivery

Test:

In-app delivery
Provider success
Provider failure
Retry
Timeout
Permission denied

54. Testing — User Experience

Verify:

Notification opens correct screen
Unread count updates
Read status works
Read-all works
Disabled notifications remain disabled

55. Security

Protect notification APIs against:

Unauthorized access
Cross-user notification access
IDOR
Injection
Abuse

A user must only be able to read/update their own notifications and preferences.

56. Backend Architecture

Suggested:

backend/
└── src/
    └── notifications/
        ├── NotificationService
        ├── NotificationController
        ├── NotificationScheduler
        ├── NotificationPreferenceService
        ├── NotificationTemplateService
        ├── NotificationPolicyService
        ├── providers/
        │   ├── InAppProvider
        │   ├── PushProvider
        │   └── EmailProvider
        └── jobs/
            ├── DailyReminderJob
            ├── StreakReminderJob
            └── WeeklyProgressJob

Adapt naming to the actual backend stack.

57. Implementation Order

1. Notification database tables
2. Notification preferences
3. NotificationService
4. In-app notifications
5. Notification APIs
6. Notification center UI
7. Daily reminder scheduler
8. Streak reminder scheduler
9. Goal/achievement event integration
10. Quiet hours
11. Deduplication
12. Tests
13. Push/email providers later

58. MVP Scope

Must have:

[ ] Notification entity
[ ] Notification preferences
[ ] In-app notification center
[ ] Unread count
[ ] Daily reminder
[ ] Streak reminder
[ ] Goal completion
[ ] Achievement notification
[ ] Timezone support
[ ] Quiet hours
[ ] Duplicate prevention
[ ] Notification APIs
[ ] Basic analytics

Not required:

[ ] Email provider
[ ] Push provider
[ ] WhatsApp
[ ] AI-generated notifications
[ ] Advanced personalization

59. Definition of Done

Notification & Reminder System is complete when:

[ ] Users can enable/disable notifications
[ ] Users can configure reminder time
[ ] Users can configure quiet hours
[ ] Timezone is respected
[ ] Daily reminders work
[ ] Streak reminders work
[ ] Goal completion notifications work
[ ] Achievement notifications work
[ ] Notifications appear in-app
[ ] Unread count works
[ ] Notifications can be marked read
[ ] Duplicate notifications are prevented
[ ] User isolation is enforced
[ ] Scheduler is reliable
[ ] Core tests pass

60. Final Architecture

                  LEARNING ENGINE
                         │
                         ▼
                   DOMAIN EVENTS
                         │
                         ▼
                NOTIFICATION SERVICE
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
          Preferences  Policy    Scheduler
              │          │          │
              └──────────┼──────────┘
                         ▼
                  Notification
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
            In-App      Push       Email
              │          │          │
              └──────────┼──────────┘
                         ▼
                        USER

61. Key Architectural Rule

Learning modules create learning events; the Notification Service decides whether, when, and how the learner should be notified.

Do not embed notification logic inside:

Learning Engine
Gamification
AI Coach
Frontend components

62. Product Outcome

The ideal experience is:

User chooses reminder time
        ↓
App waits
        ↓
Reminder arrives at the right time
        ↓
User opens today's lesson
        ↓
Learning completed
        ↓
Streak/progress updated
        ↓
Optional positive feedback

The system should help learners build a habit without making the product feel like a spam notification app
