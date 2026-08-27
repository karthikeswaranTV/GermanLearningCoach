Product Requirements Document — Step 8: Gamification & Progress System

Version: 0.1
Status: Draft

1. Purpose

The Gamification & Progress System makes language learning motivating, measurable, and habit-forming.

It converts meaningful learning activity into:

XP

Levels

Streaks

Achievements

Challenges

Milestones

Progress indicators

Daily goals

The system must reward genuine learning rather than meaningless activity.

2. Core Principle

Gamification should encourage learning, not replace learning.

Architecture:

Learning Activity
↓
Learning Engine
↓
Learning Event
↓
Gamification Engine
↓
XP / Streak / Achievement / Progress

The Learning Engine remains the source of truth for learning.

The Gamification Engine consumes learning events.

3. MVP Gamification Features

The MVP should support:

XP

Levels

Daily goals

Daily streaks

Achievements

Learning milestones

Progress dashboard

Weekly progress

Skill progress

CEFR progress

Optional for later:

Leaderboards

Friends

Competitions

Seasonal challenges

Social sharing

4. XP System

XP represents meaningful learning effort.

XP should be awarded for activities such as:

Completing a lesson

Completing a review

Completing exercises

Completing a learning session

Completing a conversation

Mastering a concept

Completing a daily goal

5. XP Rules

Example initial values:

Activity

XP

Exercise completed

5

Review completed

10

Lesson completed

25

Conversation completed

20

Daily goal completed

30

Concept mastered

50

Assessment completed

40

These values must be configurable.

6. XP Anti-Abuse Rules

Do not award unlimited XP for meaningless repetition.

Examples:

Repeating the same exercise excessively should have reduced/no XP.

Opening a lesson should not award XP.

Viewing a page should not award XP.

Restarting an exercise should not repeatedly award full XP.

Duplicate events must not produce duplicate XP.

7. XP Event Model

Example:

{
  "eventId": "evt_123",
  "userId": "user_123",
  "eventType": "LESSON_COMPLETED",
  "xp": 25,
  "createdAt": "2026-08-28T10:00:00Z"
}

8. Levels

XP should translate into learner levels.

Example:

Level 1 → Beginner
Level 2 → Explorer
Level 3 → Learner
Level 4 → Builder
Level 5 → Communicator

The names can be changed later.

9. Level Formula

Use a simple configurable progression for MVP.

Example:

Level 1:
0 XP

Level 2:
100 XP

Level 3:
250 XP

Level 4:
500 XP

Level 5:
850 XP

Level 6:
1300 XP

Do not make progression excessively difficult.

10. Level vs CEFR

Gamification level and language level are different.

Example:

Gamification Level:
12

German Level:
A1

A user can gain XP without advancing their CEFR level.

CEFR advancement must continue to depend on the Learning Engine.

11. Daily Goal

The user should have a daily learning goal.

Examples:

10 minutes
20 minutes
30 minutes
45 minutes
60 minutes

or:

10 XP
25 XP
50 XP

The MVP should preferably use learning minutes + meaningful activities rather than XP alone.

12. Daily Goal Completion

Example:

Daily Goal:
20 minutes

Completed:
18 minutes

Progress:
90%

At:

20 minutes

the goal becomes:

COMPLETED

13. Streak

A streak represents consecutive days of meaningful learning.

Example:

Monday     ✓
Tuesday    ✓
Wednesday  ✓
Thursday   ✓
Friday     ✓

Streak = 5 days

14. Meaningful Learning

A streak should require meaningful activity.

Examples:

Complete a lesson

Complete a review

Complete a defined number of exercises

Complete a conversation

Complete a meaningful learning session

Simply opening the app should not maintain the streak.

15. Streak Rules

Example:

First learning day:
1 day

Next consecutive day:
2 days

Next:
3 days

If no meaningful learning occurs on a calendar day:

Streak ends

The learner's local timezone must determine the day boundary.

16. Streak Freeze

A streak freeze can be introduced later.

For MVP:

No streak freeze

or optionally:

1 free freeze per week

If implemented, it must be clearly explained to the learner.

17. Longest Streak

Store:

currentStreak
longestStreak
lastLearningDate

Example:

Current:
12 days

Longest:
37 days

18. Streak Milestones

Possible achievements:

3-day streak
7-day streak
14-day streak
30-day streak
60-day streak
100-day streak
365-day streak

19. Achievements

Achievements reward meaningful milestones.

Categories:

Learning
Consistency
Mastery
Vocabulary
Grammar
Speaking
Reading
Listening
Writing
CEFR

20. Example Achievements

First Step
Complete your first lesson.

Getting Started
Complete 5 lessons.

Week Warrior
Maintain a 7-day streak.

Grammar Builder
Master 10 grammar concepts.

Word Collector
Learn 100 vocabulary items.

Conversation Starter
Complete your first AI conversation.

A1 Explorer
Complete the A1 curriculum.

21. Achievement Structure

Example:

{
  "id": "achievement_7_day_streak",
  "name": "Week Warrior",
  "description": "Maintain a 7-day learning streak.",
  "category": "CONSISTENCY",
  "requirement": {
    "type": "STREAK",
    "value": 7
  },
  "xpReward": 50
}

22. Achievement Evaluation

Achievements should be evaluated from learning events.

Example:

REVIEW_COMPLETED
       ↓
Update progress
       ↓
Check achievements
       ↓
Unlock if requirement satisfied

23. Achievement Idempotency

An achievement must only be unlocked once.

If the same event is processed twice:

Do not:
- award XP twice
- create duplicate achievement
- create duplicate notification

24. Milestones

Milestones represent meaningful learner progress.

Examples:

First lesson
10 lessons
50 lessons
100 concepts
500 vocabulary words
A1 completed
A2 completed

Milestones should appear prominently in the progress dashboard.

25. Skill Progress

Show progress separately for:

Vocabulary
Grammar
Reading
Listening
Speaking
Writing

Example:

Grammar      68%
Vocabulary   76%
Reading      61%
Listening    54%
Speaking     48%
Writing      63%

26. CEFR Progress

The dashboard should show:

Current Level:
A1

Progress:
72%

Next:
A2

The percentage should be based on Learning Engine data, not XP.

27. Overall Progress

Overall progress may combine:

Concept mastery
Skill mastery
Curriculum completion

Example:

Concept mastery       65%
Skill mastery         61%
Curriculum completion 70%

Overall               65%

The exact formula should remain configurable.

28. Weekly Progress

Provide a weekly summary.

Example:

This Week

Learning:
3h 25m

Lessons:
8

Reviews:
24

XP:
420

New words:
87

Current streak:
6 days

29. Monthly Progress

Future support:

Total learning time
Lessons completed
Concepts mastered
XP earned
Best streak
Skill improvement
CEFR progress

30. Progress Dashboard

The dashboard should prioritize useful information.

Recommended layout:

--------------------------------
German Learning Coach
--------------------------------

🔥 12 Day Streak

A1 → 72%

Today's Goal
████████████░░ 80%

20 / 25 min

Next Activity
Review German Articles
5 min

--------------------------------
Skills

Grammar      68%
Vocabulary   76%
Speaking     48%
Listening    54%
--------------------------------

This Week
3h 25m | 8 lessons | 420 XP

Achievements
🏆 Week Warrior
🏆 First Conversation
--------------------------------

Avoid cluttering the dashboard with too many badges or statistics.

31. Daily Dashboard

The most important information should be:

Today's goal

Current streak

Next learning activity

Overall progress

Skill progress

Recent achievements

32. Gamification Events

Recommended events:

XP_EARNED
LEVEL_UP
STREAK_STARTED
STREAK_EXTENDED
STREAK_BROKEN
DAILY_GOAL_COMPLETED
ACHIEVEMENT_UNLOCKED
MILESTONE_REACHED

33. Event Flow

Example:

Lesson Completed
       ↓
Learning Engine
       ↓
LESSON_COMPLETED
       ↓
Gamification Engine
       ↓
+25 XP
       ↓
Check Level
       ↓
Check Achievements
       ↓
Update Dashboard

34. Level-Up Flow

XP Earned
    ↓
New XP Total
    ↓
Calculate Level
    ↓
Previous Level != New Level?
    ↓
LEVEL_UP
    ↓
Notification

35. Level-Up UX

When the learner levels up:

🎉 Level Up!

You reached Level 5

Keep going!

Keep the animation lightweight.

Do not interrupt learning for long.

36. Achievement UX

When an achievement is unlocked:

🏆 Achievement Unlocked

Week Warrior

7 days of consistent learning.

+50 XP

The user should be able to dismiss it quickly.

37. Progress Notifications

Useful notifications:

Daily goal completed
New achievement
Level up
Streak milestone
Concept mastered
Weekly summary

Avoid excessive notifications.

38. Notification Rules

Do not notify for every small action.

Bad:

+5 XP
+5 XP
+5 XP
+5 XP

Better:

Lesson completed!
+25 XP

and aggregate minor events where possible.

39. Leaderboards

Leaderboards should NOT be required for MVP.

Reasons:

Can discourage beginners

Requires additional privacy considerations

Adds backend complexity

Can encourage XP farming

Does not directly improve language learning

Future option:

Weekly friends leaderboard

40. Social Features

Future features may include:

Friends
Challenges
Study groups
Shared achievements
Friendly competitions

These are outside MVP scope.

41. Gamification Database

Recommended entities:

UserXP
UserLevel
UserStreak
DailyGoal
Achievement
UserAchievement
Milestone
GamificationEvent

42. UserXP

Example fields:

id
userId
totalXp
currentLevel
createdAt
updatedAt

43. UserStreak

Example:

id
userId
currentStreak
longestStreak
lastLearningDate
createdAt
updatedAt

44. DailyGoal

Example:

id
userId
goalType
target
completed
date
createdAt
updatedAt

45. UserAchievement

Example:

id
userId
achievementId
unlockedAt
xpAwarded

46. Gamification Event

Example:

id
userId
eventId
eventType
source
xp
processedAt
createdAt

eventId must be unique for idempotency.

47. API Endpoints

Possible endpoints:

GET /api/gamification/profile

GET /api/gamification/xp

GET /api/gamification/level

GET /api/gamification/streak

GET /api/gamification/daily-goal

GET /api/gamification/achievements

GET /api/gamification/milestones

GET /api/gamification/weekly-progress

GET /api/gamification/dashboard

48. Dashboard API

Prefer a single optimized endpoint:

GET /api/gamification/dashboard

Example:

{
  "xp": 1250,
  "level": 6,
  "xpToNextLevel": 250,
  "currentStreak": 12,
  "longestStreak": 37,
  "dailyGoal": {
    "targetMinutes": 25,
    "completedMinutes": 20,
    "percentage": 80
  },
  "achievementsUnlocked": 8
}

49. Security

The client must not be able to submit:

xp = 100000
level = 50
streak = 500
achievement = unlocked

The backend must calculate and validate all gamification values.

50. XP Calculation Ownership

The client sends:

"I completed this activity."

The backend determines:

Is it valid?
How much XP?
Has it already been processed?
Does it trigger an achievement?
Does it cause a level-up?

51. Anti-Cheating

The backend should detect:

Duplicate events
Impossible completion rates
Repeated exercise farming
Abnormally high activity
Client-generated XP

For MVP, basic server-side validation is sufficient.

52. Testing

Test:

XP calculation
Level progression
Streak calculation
Daily goal calculation
Achievement unlocking
Milestones
Duplicate events
Timezone boundaries
Dashboard aggregation

53. XP Tests

Example:

Lesson completed
→ +25 XP

Duplicate completion:

Same event submitted twice
→ +25 XP only once

54. Streak Tests

Example:

Monday learning
Tuesday learning
Wednesday learning

Expected:
3-day streak

Missed day:

Monday
Tuesday
Thursday

Expected:
Streak reset on Thursday

Test timezone boundaries carefully.

55. Achievement Tests

Example:

7 consecutive learning days
→ Week Warrior unlocked

Submitting the same event again:

Achievement remains unlocked
No duplicate reward

56. Dashboard Tests

Verify:

XP is correct
Level is correct
Streak is correct
Daily goal is correct
Achievements are correct
Skill progress comes from Learning Engine
CEFR progress comes from Learning Engine

57. MVP Scope

Must have:

[ ] XP
[ ] Levels
[ ] Daily goals
[ ] Streaks
[ ] Achievements
[ ] Milestones
[ ] Skill progress
[ ] CEFR progress
[ ] Weekly progress
[ ] Dashboard
[ ] Gamification events
[ ] Idempotency
[ ] Server-side validation
[ ] Unit tests

Not required for MVP:

[ ] Global leaderboard
[ ] Friends
[ ] Social feed
[ ] Competitions
[ ] Seasonal events
[ ] Paid rewards

58. Definition of Done

The Gamification System is complete when:

[ ] Learners earn XP from meaningful activities
[ ] XP cannot be manipulated from the client
[ ] Levels are calculated automatically
[ ] Daily goals are tracked
[ ] Streaks are tracked correctly
[ ] Local timezone is respected
[ ] Achievements unlock automatically
[ ] Achievements cannot be duplicated
[ ] Milestones are tracked
[ ] Skill progress is displayed
[ ] CEFR progress is displayed
[ ] Weekly progress is available
[ ] Dashboard aggregates the required information
[ ] Duplicate events are handled safely
[ ] Core gamification logic has unit tests

59. Final Architecture

                         USER
                           │
                           ▼
                       FRONTEND
                           │
                           ▼
                       API LAYER
                           │
              ┌────────────┴────────────┐
              ▼                         ▼
       LEARNING ENGINE          GAMIFICATION ENGINE
              │                         │
              │                         ├── XP
              │                         ├── Levels
              │                         ├── Streaks
              │                         ├── Goals
              │                         ├── Achievements
              │                         └── Milestones
              │
              ▼
       LEARNING EVENTS
              │
              └─────────────────────────►
                           │
                           ▼
                        DATABASE

60. Key Architectural Rule

Learning creates the evidence. Gamification rewards the evidence.

The Gamification Engine must never become the source of truth for language proficiency.

For example:

1000 XP
≠
B1 German

Instead:

Learning Engine
→ mastery
→ skills
→ CEFR readiness

Gamification Engine
→ XP
→ levels
→ streaks
→ achievements

61. Product Outcome

The learner should feel:

I know what I need to learn.
        ↓
I complete meaningful activities.
        ↓
I can see my progress.
        ↓
I earn rewards for real learning.
        ↓
I maintain my learning habit.
        ↓
I become better at German.

The ultimate goal is:

Make consistent German learning rewarding without allowing gamification to replace real learning
