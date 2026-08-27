Product Requirements Document — Step 7: Learning Engine

Version: 0.1
Status: Draft
Depends on:

01-product-definition.md

02-ux-ui-specification.md

03-technical-architecture.md

04-database-design.md

05-api-specification.md

06-ai-coach-prompt-architecture.md

1. Purpose

The Learning Engine is the core decision-making system of the German Language Coach.

Its responsibility is to determine:

What should this learner learn, practice, review, or improve next?

The Learning Engine combines:

Learner level

Skill mastery

Lesson progress

Exercise performance

Mistakes

Review history

Spaced repetition

Difficulty

Prerequisites

Learning goals

Daily availability

Recent activity

The Learning Engine must provide a personalized learning path while remaining predictable, explainable, and testable.

2. Core Principle

The Learning Engine is the source of truth for learner progress.

AI may provide:

Suggestions

Corrections

Explanations

Content

Insights

But the Learning Engine decides:

Mastery

Progress

Review priority

Lesson completion

Skill status

Next activity

CEFR progression

Architecture:

                 AI COACH
                    │
                    │ suggestions/evidence
                    ▼
             LEARNING ENGINE
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
     Mastery      Review      Progress
        │           │           │
        └───────────┼───────────┘
                    ▼
                 DATABASE

3. Learning Philosophy

The system should follow:

Learn
 ↓
Practice
 ↓
Recall
 ↓
Apply
 ↓
Review
 ↓
Master

The system should not assume that completing a lesson means mastering the concept.

Completion and mastery are separate concepts.

4. Learning Unit

The smallest meaningful learning unit is a:

Concept

Examples:

German greetings
Personal pronouns
sein
haben
German articles
der
die
das
Accusative case
Dative case
Present tense
Perfekt
Word order

A lesson can contain multiple concepts.

5. Skill Model

The initial skill model should contain:

Vocabulary
Grammar
Reading
Listening
Speaking
Writing

Example:

Learner
│
├── Vocabulary
├── Grammar
├── Reading
├── Listening
├── Speaking
└── Writing

6. Skill Weighting

For the MVP, all major skills may initially have equal importance.

Example:

Vocabulary  → 20%
Grammar     → 20%
Reading     → 15%
Listening   → 15%
Speaking    → 15%
Writing     → 15%

The exact weights should be configurable.

The system should later support goal-specific weighting.

Example:

WORK_IN_GERMANY

may increase the importance of:

Speaking
Writing
Workplace Vocabulary

7. CEFR Levels

The learning system should support:

A1
A2
B1
B2

Future support may include:

C1
C2

The MVP should focus primarily on:

A1
A2

8. CEFR Progression

Learners should progress through concepts rather than simply accumulating XP.

Example:

A1
 │
 ├── Greetings
 ├── Introductions
 ├── Numbers
 ├── Family
 ├── Food
 ├── Basic verbs
 ├── Articles
 └── Basic sentence structure
       │
       ▼
A2

9. Learning States

Every concept should have a learning state.

Recommended states:

NEW
LEARNING
FAMILIAR
MASTERED

Optional:

REVIEW_DUE

However, REVIEW_DUE may be represented as a scheduling condition rather than a permanent state.

10. State Definitions

NEW

The learner has not meaningfully practiced the concept.

Mastery: 0–20

LEARNING

The learner has encountered and practiced the concept.

Mastery: 21–50

FAMILIAR

The learner demonstrates reasonable understanding.

Mastery: 51–79

MASTERED

The learner consistently demonstrates the concept over time.

Mastery: 80–100

The thresholds must be configurable.

11. Mastery

Mastery represents how well the learner understands and can apply a concept.

Mastery must not be based solely on lesson completion.

Possible inputs:

Correct answers
Incorrect answers
Recent performance
Historical performance
Review performance
Difficulty
Time since last practice
Repeated mistakes
Application performance

12. Initial Mastery Formula

For the MVP, use a simple weighted model.

Example:

Mastery =
    40% recent performance
  + 30% historical performance
  + 20% review performance
  + 10% consistency

All components should be normalized to:

0–100

13. Recency Weight

Recent performance should matter more than very old performance.

Example:

Recent correct answer
→ stronger positive signal

Old correct answer
→ weaker positive signal

This prevents a learner from being considered mastered forever based on old performance.

14. Incorrect Answer Impact

Incorrect answers should reduce mastery, but not excessively.

Example:

Mastery = 75

One mistake
→ 70

Repeated mistakes
→ stronger reduction

The exact algorithm should be tuned through testing.

15. Consecutive Success

Repeated correct answers should increase confidence.

Example:

Correct
Correct
Correct
Correct

should produce stronger evidence of mastery than:

Correct
Wrong
Correct
Wrong

16. Consistency

A learner should not become "mastered" based on one excellent session.

Mastery should require performance across multiple attempts and preferably multiple sessions.

Example:

Session 1 → 90%
Session 2 → 85%
Session 3 → 92%

is stronger evidence than:

One session → 100%

17. Mastery Decay

Mastery should gradually lose confidence when a concept has not been practiced for a long time.

This does not mean the learner literally forgets the concept instantly.

Instead:

The system becomes less confident that the learner can reliably recall it.

Example:

Mastery: 90
No practice for a long period
↓
Effective mastery: 82

This makes old concepts eligible for review.

18. Spaced Repetition

The system should use spaced repetition for review.

Basic progression:

First successful review
→ Short interval

Second successful review
→ Longer interval

Third successful review
→ Longer interval

Repeated success
→ Increasing interval

19. Review Intervals

Initial MVP intervals can be:

New:
same day

After successful review:
1 day

Second successful review:
3 days

Third successful review:
7 days

Fourth successful review:
14 days

Fifth successful review:
30 days

Advanced:
60+ days

These values must be configurable.

20. Failed Review

If the learner fails a review:

Reduce confidence
 ↓
Reset or shorten interval
 ↓
Schedule earlier review

Example:

Current interval:
14 days

Failed review
 ↓
Next review:
1–3 days

21. Review Quality

Not all answers are equal.

The system should distinguish:

Correct
Correct with hint
Partially correct
Incorrect
Skipped

Suggested quality scale:

0 = Incorrect
1 = Very difficult
2 = Difficult
3 = Correct
4 = Easy

The scale can later be expanded.

22. Hint Usage

Using a hint should reduce the strength of the success signal.

Example:

Correct without hint
→ strong positive signal

Correct after hint
→ moderate positive signal

This prevents users from becoming "mastered" while heavily relying on hints.

23. Answer Attempts

Each exercise attempt should record:

User
Concept
Exercise
Answer
Correctness
Score
Hint used
Time taken
Timestamp

24. Mistake Model

Mistakes are important learning signals.

A mistake should be associated with a concept whenever possible.

Example:

"I want one Pizza."

Mistake:
ein Pizza

Concept:
FEMININE_ARTICLE

25. Mistake Frequency

Track how often the learner makes the same conceptual mistake.

Example:

FEMININE_ARTICLE
Occurrences: 5

Repeated mistakes increase review priority.

26. Mistake Recency

Recent mistakes should have greater weight.

Example:

Mistake yesterday
→ high priority

Mistake six months ago
→ lower priority

unless the mistake continues to recur.

27. Weak Area Detection

A weak area can be detected using:

Low mastery
+
High mistake frequency
+
Recent mistakes
+
Low review performance

Example:

Articles
Mastery: 42
Recent mistakes: 6
Review success: 55%

→ Weak Area

28. Strong Area Detection

A strong area should have:

High mastery
+
Repeated successful reviews
+
Low recent mistake frequency
+
Good consistency

Example:

Greetings
Mastery: 94
Recent mistakes: 0
Reviews: 5 successful

→ Strong Area

29. Prerequisites

Some concepts depend on earlier concepts.

Example:

German Articles
      ↓
Accusative Articles
      ↓
Dative Articles

Another:

Present Tense
      ↓
Past Tense
      ↓
Perfekt

The system should avoid introducing advanced concepts before important prerequisites are sufficiently understood.

30. Prerequisite Completion

A prerequisite should be considered ready when:

Prerequisite mastery >= configured threshold

Example:

Threshold = 70

If:

Articles mastery = 45

then:

Accusative Articles

should not become the primary learning recommendation.

31. Concept Dependency Graph

Concepts can be represented as:

Concept A
   ↓
Concept B
   ↓
Concept C

Example:

Nouns
 ↓
Gender
 ↓
Articles
 ↓
Accusative
 ↓
Accusative Articles

32. Lesson Model

A lesson should contain:

Lesson
 ├── Concept 1
 ├── Concept 2
 ├── Concept 3
 └── Practice

Lesson completion does not automatically mean mastery.

33. Lesson Completion

A lesson may be marked complete when:

Required activities completed
+
Minimum score achieved

Example:

Lesson progress >= 80%

The exact threshold should be configurable.

34. Mastery vs Completion

Example:

Lesson:
German Articles

Completed:
YES

Mastery:
58

This means:

The learner completed the lesson but still needs practice.

The UI should clearly distinguish these.

35. Daily Learning Plan

The system should generate a daily learning plan.

Example:

Today's Plan

1. Review: Articles        5 min
2. New: Food Vocabulary    8 min
3. Practice: Word Order    7 min
4. Conversation            5 min

Total:
25 minutes

36. Daily Plan Priorities

Recommended priority order:

1. Overdue reviews
2. High-risk weak areas
3. Current lesson
4. New concepts
5. Optional enrichment

37. Review Priority Score

Every review candidate should have a priority score.

Example:

Priority =
    overdue score
  + weakness score
  + mistake score
  + importance score
  + recency score

Normalize the result if required.

38. Overdue Score

Example:

Not due:
0

Due today:
moderate

1 day overdue:
higher

7 days overdue:
very high

The longer a review is overdue, the higher its priority.

39. Weakness Score

Example:

Mastery 80+
→ low priority

Mastery 50–79
→ medium priority

Mastery below 50
→ high priority

40. Mistake Score

Example:

0 recent mistakes
→ 0

1–2
→ low

3–5
→ medium

6+
→ high

Values should be configurable.

41. Concept Importance

Some concepts are foundational.

Example:

Basic verbs
Articles
Pronouns
Word order

should receive higher importance than optional vocabulary.

Example:

importance:
1.0 = normal
1.5 = important
2.0 = foundational

42. Next Best Activity

The system should select the next activity using:

Due reviews
+
Weak areas
+
Current lesson
+
Prerequisites
+
Learner goal
+
Available time

43. Next Activity Types

The MVP should support:

NEW_LESSON
REVIEW
PRACTICE
CONVERSATION
VOCABULARY
GRAMMAR
READING
LISTENING
WRITING
SPEAKING

44. Next Activity Algorithm

Basic algorithm:

1. Find all eligible activities.
2. Remove activities blocked by prerequisites.
3. Identify overdue reviews.
4. Calculate priority score.
5. Identify weak concepts.
6. Add current lesson activities.
7. Apply learner goal weighting.
8. Apply available-time constraint.
9. Select highest-value activity.
10. Return recommendation.

45. Pseudocode

function getNextActivity(learner) {

  const candidates = getEligibleActivities(learner);

  const eligible = candidates.filter(
    activity => prerequisitesSatisfied(activity, learner)
  );

  const scored = eligible.map(activity => ({
    activity,
    score:
      overdueScore(activity, learner) +
      weaknessScore(activity, learner) +
      mistakeScore(activity, learner) +
      importanceScore(activity) +
      goalScore(activity, learner)
  }));

  scored.sort((a, b) => b.score - a.score);

  return scored[0];
}

46. Available Time

The recommendation should consider how much time the learner has.

Example:

5 minutes
→ Quick review

15 minutes
→ Review + practice

30 minutes
→ Review + new lesson + application

60 minutes
→ Full learning session

The system should not recommend a 30-minute lesson when the learner has selected a 5-minute session.

47. Learner Goal

The learner's goal should influence recommendations.

Example:

Goal:
WORK_IN_GERMANY

May prioritize:

Workplace vocabulary
Professional writing
Speaking
Interview conversations
Workplace scenarios

Another learner:

Goal:
TRAVEL

may prioritize:

Travel vocabulary
Directions
Restaurants
Hotels
Transportation

48. Personalization

Personalization should come from measurable learner data.

Examples:

Repeated mistakes
Weak skills
Strong skills
Preferred learning duration
Goal
Recent activity

Avoid arbitrary personalization.

49. AI Integration

The AI Coach may provide:

Mistake classification
Open-answer evaluation
Content generation
Explanation
Conversation feedback

The Learning Engine consumes these results.

Example:

AI:
Concept = FEMININE_ARTICLE
Confidence = high

Learning Engine:
Increase review priority

50. AI Must Not Control Progress

Do not allow AI to directly say:

"Mastery = 95"

and store that value without validation.

Instead:

AI evaluation
 ↓
Validated evidence
 ↓
Learning Engine
 ↓
Mastery calculation

51. Deterministic Rules

Where possible, use deterministic logic.

Examples:

XP calculation
Streak calculation
Review dates
Lesson completion
Prerequisites
Mastery aggregation

These should not depend on AI.

52. Learning Session

A session should contain:

Session
 ├── Start time
 ├── End time
 ├── Activities
 ├── Attempts
 ├── XP earned
 └── Concepts practiced

53. Session Types

Examples:

DAILY_PLAN
LESSON
REVIEW
PRACTICE
CONVERSATION
ASSESSMENT

54. Session Completion

A session is complete when:

Required activities completed

or when the learner explicitly ends it.

Partial sessions should still record progress.

55. Progress Tracking

Track progress at multiple levels:

Overall
 ↓
CEFR level
 ↓
Skill
 ↓
Concept
 ↓
Exercise

Example:

A1
 ├── Grammar: 68%
 │    ├── Articles: 52%
 │    ├── Pronouns: 82%
 │    └── Verbs: 71%
 │
 └── Vocabulary: 76%

56. Overall Progress

Overall progress should not simply be:

Completed lessons / total lessons

Instead, combine:

Concept mastery
+
Skill mastery
+
Curriculum completion

Example:

Overall Progress =
40% Concept Mastery
+
30% Skill Mastery
+
30% Curriculum Completion

Weights should be configurable.

57. CEFR Advancement

A learner should advance to the next level only when sufficient evidence exists.

Example requirements:

A1 → A2

[ ] A1 curriculum completed
[ ] Minimum concept mastery
[ ] Minimum skill mastery
[ ] Assessment passed
[ ] Required foundational concepts mastered

58. Assessment

Assessments should measure:

Vocabulary
Grammar
Reading
Listening
Writing
Speaking

where supported.

59. Assessment Frequency

Avoid excessive assessments.

Possible approach:

Diagnostic:
At onboarding

Progress assessment:
After major milestones

Level assessment:
At end of CEFR level

60. Diagnostic Assessment

The initial diagnostic should estimate the learner's starting point.

It should identify:

Known concepts
Weak concepts
Unknown concepts
Potential level

AI may assist with open-ended evaluation.

The final placement should be determined by the assessment engine.

61. Difficulty

Every concept and exercise should have a difficulty.

Example:

1 = Very Easy
2 = Easy
3 = Medium
4 = Difficult
5 = Very Difficult

Difficulty should be associated with CEFR where possible.

62. Adaptive Difficulty

If the learner performs consistently well:

Increase difficulty

If performance is poor:

Reduce difficulty

Example:

5 correct answers
→ slightly harder exercise

Repeated failures
→ easier exercise + explanation

63. Difficulty Boundaries

Do not increase difficulty too quickly.

Example:

A1 learner
→ A1 easy
→ A1 medium
→ A1 difficult
→ A2 introductory

Avoid:

A1
→ B1

without evidence.

64. Exercise Selection

Exercise selection should consider:

Concept
Mastery
Difficulty
Previous attempts
Recent mistakes
Exercise type
Review schedule

Avoid repeatedly showing the exact same exercise.

65. Exercise Diversity

For one concept, vary exercise formats.

Example:

Multiple choice
Fill in the blank
Translation
Sentence ordering
Writing
Conversation
Listening

This helps test actual understanding rather than memorization of one exercise.

66. Transfer Learning

A learner should demonstrate a concept in multiple contexts.

Example:

Practice:
Ich kaufe eine Pizza.

Later:
Ich möchte eine Tasche.

Later:
Ich sehe eine Lampe.

The concept:

Feminine article

is reinforced across contexts.

67. Mastery Confirmation

Before marking a concept as mastered, prefer evidence from:

Different exercises
+
Different contexts
+
Different sessions

68. Review Scheduling Data

Each reviewable concept should store or derive:

lastReviewedAt
nextReviewAt
reviewCount
successfulReviews
failedReviews
currentInterval
mastery

69. Review Scheduling Algorithm

Basic approach:

function calculateNextReview(
  quality: number,
  currentInterval: number
) {

  if (quality <= 1) {
    return 1; // day
  }

  if (quality === 2) {
    return Math.max(1, Math.floor(currentInterval * 0.5));
  }

  if (quality === 3) {
    return Math.max(1, Math.floor(currentInterval * 1.5));
  }

  return Math.max(1, Math.floor(currentInterval * 2));
}

This is an initial MVP algorithm.

It can later be replaced with a more sophisticated spaced-repetition model.

70. Time Zone

Review dates must use the learner's configured time zone.

Example:

Learner:
Asia/Kolkata

The backend must not assume UTC dates represent the learner's local day.

71. Daily Streak Boundary

The Learning Engine should determine whether a learner completed meaningful learning based on their local date.

Example:

Asia/Kolkata
August 28

should be evaluated according to the learner's local calendar day.

72. Meaningful Learning

A streak should not be maintained by meaningless activity.

Possible minimum:

At least one meaningful learning activity

Examples:

Complete lesson
Complete review
Complete 5 exercises
Complete meaningful conversation

The exact requirement should be configurable.

73. XP Boundary

XP belongs to the gamification system.

The Learning Engine should emit learning events such as:

LESSON_COMPLETED
EXERCISE_COMPLETED
REVIEW_COMPLETED
CONVERSATION_COMPLETED
CONCEPT_MASTERED

The Gamification Engine converts those events into XP.

74. Learning Events

Recommended events:

LEARNING_SESSION_STARTED
LEARNING_SESSION_COMPLETED
LESSON_STARTED
LESSON_COMPLETED
EXERCISE_ATTEMPTED
EXERCISE_COMPLETED
REVIEW_COMPLETED
CONCEPT_PRACTICED
CONCEPT_MASTERED
ASSESSMENT_COMPLETED
CONVERSATION_COMPLETED
WRITING_SUBMITTED
SPEAKING_COMPLETED

75. Event Processing

Architecture:

Learning Activity
      ↓
Learning Engine
      ↓
Learning Event
      ↓
Event Handler
      ↓
Progress / Mastery / Gamification

76. Idempotency

Learning events must be safely processed.

If the same event is accidentally submitted twice:

Do not:
Double XP
Double progress
Double review count

Use an event ID or idempotency key.

77. Data Integrity

The Learning Engine must validate:

User exists
Concept exists
Exercise belongs to concept
Exercise is available
Attempt belongs to user
Review belongs to user

Never trust client-side learning results.

78. Backend Ownership

All important learning calculations must happen on the backend.

Client may display:

Progress
Mastery
XP
Review date

but should not be trusted to calculate them.

79. Caching

Frequently accessed data may be cached:

Current progress
Today's plan
Skill summary

But learning writes must invalidate or update relevant caches.

80. Performance

The next activity recommendation should be fast.

Target:

Typical recommendation:
< 500ms

AI calls should not be required for basic next-activity selection.

This is important for both performance and free-tier AI usage.

81. Offline / Temporary AI Failure

The Learning Engine must continue to function if AI is unavailable.

Example:

AI unavailable
 ↓
Existing lessons
 ↓
Existing exercises
 ↓
Reviews
 ↓
Progress

AI should enhance the learning system, not become a single point of failure.

82. Recommended Service Architecture

Suggested backend services:

LearningService
    │
    ├── MasteryService
    ├── ReviewService
    ├── RecommendationService
    ├── ProgressService
    ├── AssessmentService
    └── LearningEventService

83. Mastery Service

Responsibilities:

calculateMastery()
updateMastery()
getMastery()
getSkillMastery()
getConceptMastery()

84. Review Service

Responsibilities:

getDueReviews()
calculateNextReview()
recordReview()
getReviewPriority()

85. Recommendation Service

Responsibilities:

getNextActivity()
generateDailyPlan()
scoreActivity()
getWeakAreas()

86. Progress Service

Responsibilities:

getOverallProgress()
getSkillProgress()
getCEFRProgress()
getConceptProgress()

87. Assessment Service

Responsibilities:

createAssessment()
recordAssessmentAnswer()
calculateAssessmentScore()
determineReadiness()

88. Learning Event Service

Responsibilities:

recordEvent()
processEvent()
ensureIdempotency()
publishEvent()

89. Example Recommendation

Input:

Level:
A1

Goal:
WORK_IN_GERMANY

Available time:
15 minutes

Due reviews:
Articles
Word order

Current lesson:
Workplace vocabulary

Weak areas:
Articles
Speaking

Recent mistakes:
Articles × 4
Word order × 2

Expected recommendation:

1. Review Articles
2. Practice Word Order
3. Workplace Vocabulary
4. Short speaking activity

90. Explainable Recommendations

The system should explain why an activity is recommended.

Example:

Review Articles

Why?
You made 4 mistakes with German articles recently,
and this topic is due for review.

This increases user trust.

91. Recommendation Response

Example:

{
  "activityType": "REVIEW",
  "conceptId": "feminine-articles",
  "title": "Review German Articles",
  "reason": "You have made several recent mistakes with articles.",
  "estimatedMinutes": 5,
  "priority": 0.91
}

92. Daily Plan Response

Example:

{
  "estimatedMinutes": 20,
  "activities": [
    {
      "type": "REVIEW",
      "concept": "German Articles",
      "minutes": 5
    },
    {
      "type": "NEW_LESSON",
      "concept": "Workplace Vocabulary",
      "minutes": 8
    },
    {
      "type": "SPEAKING",
      "concept": "Introducing Yourself at Work",
      "minutes": 7
    }
  ]
}

93. Learning Engine API

Possible endpoints:

GET /api/learning/progress

GET /api/learning/skills

GET /api/learning/concepts/:id

GET /api/learning/reviews/due

GET /api/learning/next

GET /api/learning/daily-plan

POST /api/learning/attempts

POST /api/learning/reviews

POST /api/learning/sessions/start

POST /api/learning/sessions/:id/complete

GET /api/learning/weak-areas

94. Security

Learning endpoints must require authentication.

A user must only be able to access:

Their own learning data

Never trust:

userId
mastery
score
XP
nextReviewAt

from the client.

95. Testing Strategy

The Learning Engine must be heavily unit tested.

Test:

Mastery
Review scheduling
Prerequisites
Recommendation scoring
Difficulty
Progress
Streak boundaries
Idempotency

96. Mastery Test Cases

Example:

Input:
5 correct answers

Expected:
Mastery increases

Input:
Repeated incorrect answers

Expected:
Mastery decreases

Input:
Long period without review

Expected:
Review priority increases

97. Review Test Cases

Test:

Correct first review
Correct repeated review
Failed review
Hint-assisted answer
Skipped answer
Overdue review
Multiple overdue concepts

98. Recommendation Test Cases

Example:

Concept A:
Mastery 40
Due today

Concept B:
Mastery 80
Not due

Expected:
Concept A selected

Another:

Concept A:
Mastery 40

Concept B:
Mastery 50
Due for review

Expected:
Compare calculated priority

99. Prerequisite Test

Example:

Articles mastery:
45

Accusative Articles:
blocked

After:

Articles mastery:
75

then:

Accusative Articles:
eligible

100. Performance Test

Test:

10,000 concepts
10,000 review records
Large mistake history

The recommendation engine should remain performant.

101. MVP Algorithm Philosophy

Do not over-engineer the first version.

Start with:

Simple rules
+
Clear scoring
+
Strong tests

Then improve based on real learner data.

102. Future Improvements

Potential future versions:

FSRS-style scheduling
Bayesian mastery estimation
Knowledge tracing
Personalized difficulty models
Reinforcement learning
AI-assisted curriculum planning
Skill graph optimization
Predictive forgetting model

These should not be required for MVP.

103. MVP Learning Engine

The MVP should implement:

[ ] Concept model
[ ] Skill model
[ ] CEFR levels
[ ] Learning states
[ ] Mastery calculation
[ ] Mistake tracking
[ ] Weak-area detection
[ ] Prerequisites
[ ] Spaced repetition
[ ] Review scheduling
[ ] Difficulty
[ ] Next activity recommendation
[ ] Daily plan
[ ] Progress tracking
[ ] Learning events
[ ] Idempotency
[ ] Backend validation
[ ] Unit tests

104. Definition of Done

The Learning Engine is complete when:

[ ] Learner can start a lesson
[ ] Learner can complete exercises
[ ] Attempts are recorded
[ ] Mistakes are recorded
[ ] Mastery is calculated
[ ] Mastery changes based on performance
[ ] Reviews are scheduled
[ ] Failed reviews are rescheduled sooner
[ ] Successful reviews get longer intervals
[ ] Weak areas are detected
[ ] Prerequisites are enforced
[ ] Next activity can be calculated
[ ] Daily learning plan can be generated
[ ] Progress is calculated
[ ] CEFR progression is measurable
[ ] AI results are validated before use
[ ] AI cannot directly change mastery
[ ] Client cannot manipulate progress
[ ] Learning events are idempotent
[ ] AI outage does not break core learning
[ ] Unit tests cover core algorithms
[ ] Recommendation performance is acceptable

105. Final Architecture

The final MVP learning architecture should look like:

                         USER
                           │
                           ▼
                       FRONTEND
                           │
                           ▼
                       API LAYER
                           │
                           ▼
                  ┌─────────────────┐
                  │ LEARNING ENGINE │
                  └─────────────────┘
                           │
       ┌───────────────────┼───────────────────┐
       │                   │                   │
       ▼                   ▼                   ▼
   MASTERY             REVIEW              PROGRESS
   SERVICE             SERVICE              SERVICE
       │                   │                   │
       └───────────────────┼───────────────────┘
                           │
                           ▼
                  RECOMMENDATION SERVICE
                           │
                           ▼
                       DATABASE
                           ▲
                           │
                    AI COACH EVIDENCE
                           ▲
                           │
                    GEMINI / GROQ

The key architectural rule is:

AI provides evidence and intelligence. The Learning Engine owns learning state and decisions.

106. Product Outcome

The learner should experience:

I learn something
       ↓
I practice it
       ↓
The system understands how well I know it
       ↓
It remembers my mistakes
       ↓
It schedules the right review
       ↓
It identifies my weak areas
       ↓
It gives me the next best activity
       ↓
I improve

The goal is not to provide unlimited lessons.

The goal is:

Give the learner the right thing to learn at the right time
