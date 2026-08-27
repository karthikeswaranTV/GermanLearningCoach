# German Language Coach

## Product Requirements Document — Step 2: UX/UI Specification

**Version:** 0.1
**Status:** Draft
**Depends on:** `01-product-definition.md`

---

# 1. UX Vision

The application should feel like a **personal German tutor**, not a traditional language-learning application.

The interface should communicate:

> "I know where you are, I know what you need, and I know what you should do next."

The UX must prioritize:

* Clarity
* Simplicity
* Personalization
* Learning focus
* Conversation
* Progress visibility
* Low cognitive load

The user should never feel overwhelmed by the number of available features.

---

# 2. Core UX Principle

Every screen should answer one of these questions:

1. **What should I do?**
2. **Why should I do it?**
3. **How do I do it?**
4. **How am I improving?**

Avoid displaying information simply because it is available.

Every element should have a purpose.

---

# 3. Primary Navigation

Use five primary destinations.

### Desktop

```text
┌──────────────────────────────────────────────────────────┐
│ Logo     Dashboard   Learn   Practice   Talk   Review   │
│                                                   Profile│
└──────────────────────────────────────────────────────────┘
```

### Mobile

Use bottom navigation:

```text
┌──────────────────────────────────────────────┐
│                                              │
│              Application                     │
│                                              │
├──────────────────────────────────────────────┤
│ Learn │ Practice │ Talk │ Review │ Profile  │
└──────────────────────────────────────────────┘
```

Dashboard should remain easily accessible through the logo/home action.

---

# 4. Information Architecture

```text
Application
│
├── Dashboard
│
├── Learn
│   ├── Learning Roadmap
│   ├── Lessons
│   └── Lesson Player
│
├── Practice
│   ├── Grammar
│   ├── Vocabulary
│   ├── Listening
│   ├── Speaking
│   └── Writing
│
├── Talk
│   ├── Conversation
│   ├── Scenarios
│   └── Conversation History
│
├── Review
│   └── Smart Review
│
└── Profile
    ├── Account
    ├── Goals
    ├── Learning Preferences
    └── Settings
```

Progress should be accessible from the Dashboard and Profile initially.

It does not need to be a top-level navigation item in the MVP.

---

# 5. Design Philosophy

The UI should be:

* Modern
* Premium
* Calm
* Friendly
* Professional
* Focused
* Responsive

Avoid:

* Excessive colors
* Excessive cards
* Childish game graphics
* Large numbers everywhere
* Cluttered dashboards
* Too many navigation items
* Flashcard-heavy interfaces

The application should look like a modern productivity/productivity-learning application rather than a children's game.

---

# 6. Visual Language

## Color

Use a restrained palette.

Suggested:

### Primary

Deep German-inspired red.

Use it primarily for:

* Primary CTA
* Important progress states
* Active navigation
* Key highlights

### Neutral

Use neutral colors for:

* Background
* Cards
* Text
* Borders
* Secondary controls

Do not make the entire application red.

---

# 7. Typography

Use a modern, highly readable sans-serif font.

Typography hierarchy:

```text
Page title
    ↓
Section title
    ↓
Card title
    ↓
Body
    ↓
Secondary information
```

German words should receive slightly stronger visual emphasis when appropriate.

Example:

> **die Wohnung**

rather than:

> die Wohnung — apartment

The German word should be visually primary.

---

# 8. Spacing

Use a consistent spacing system.

Prefer:

* Generous whitespace
* Clear separation between sections
* Comfortable reading width
* Large touch targets on mobile

Avoid tightly packed interfaces.

---

# 9. Component Philosophy

Create reusable components.

Core components:

```text
Button
Card
ProgressBar
ProgressRing
Badge
Avatar
AudioButton
MicrophoneButton
LessonCard
VocabularyCard
GrammarCard
ExerciseCard
ConversationBubble
CorrectionCard
XPIndicator
StreakIndicator
SkillProgress
BottomNavigation
TopNavigation
Modal
Toast
SkeletonLoader
```

Components should support different states.

---

# 10. Application States

Every important screen must support:

### Loading

Show skeletons or meaningful loading indicators.

### Empty

Explain what the user can do.

### Error

Provide a clear recovery action.

### Success

Confirm the action without unnecessary interruption.

### Offline / network failure

Explain what happened and allow retry.

---

# 11. Landing Page

## Goal

Explain the product in seconds.

### Hero

Headline:

> **Learn German with your personal AI coach.**

Supporting text:

> A German tutor that remembers your mistakes, adapts your lessons, and helps you speak German with confidence.

Primary CTA:

> **Start Learning Free**

Secondary CTA:

> See how it works

---

## Key benefits

Show three or four:

### Personalized

> Your learning adapts to you.

### Conversational

> Practice real German conversations.

### Adaptive

> Your mistakes shape future lessons.

### Goal-oriented

> Learn German for life and work in Germany.

---

# 12. Registration

Keep registration simple.

Options:

* Email/password
* Google login

Do not ask for learning information during registration.

After successful registration:

> Continue to onboarding.

---

# 13. Onboarding

Onboarding should feel like the beginning of the learning journey.

Use a multi-step flow.

Display progress:

```text
● ○ ○ ○ ○ ○
Step 1 of 6
```

Do not show all questions on one screen.

---

# 14. Onboarding — Step 1

### Welcome

```text
Willkommen! 🇩🇪

Your personal German coach is ready.

Let's personalize your learning experience.
```

CTA:

> Start

---

# 15. Onboarding — Step 2

### Native Language

Question:

> What is your native language?

Options:

* English
* Tamil
* Hindi
* Telugu
* Malayalam
* Kannada
* Other

Allow one selection.

---

# 16. Onboarding — Step 3

### German Level

Question:

> How much German do you already know?

Options:

* Complete beginner
* A1
* A2
* B1
* B2

Secondary option:

> I'm not sure — take an assessment

---

# 17. Onboarding — Step 4

### Goal

Question:

> Why are you learning German?

Multi-select:

* Live in Germany
* Work in Germany
* Job interviews
* Daily life
* Travel
* University
* Social conversations
* Professional German

---

# 18. Onboarding — Step 5

### Interests

Question:

> What would you like to talk about?

Examples:

* Software Engineering
* Automation Testing
* IT
* Business
* Finance
* Travel
* Daily life
* General German

Allow multiple selections.

---

# 19. Onboarding — Step 6

### Daily Time

Question:

> How much time can you learn each day?

Options:

* 10 minutes
* 15 minutes
* 30 minutes
* 45 minutes
* 60+ minutes

Recommended default:

> 15 minutes

---

# 20. Onboarding Completion

Show:

```text
Your learning plan is ready.

German Level
A1

Goal
Work & live in Germany

Daily Goal
15 minutes

Focus
Speaking + Listening + Workplace German
```

CTA:

> **See My Learning Plan**

---

# 21. Assessment

If the user chooses assessment:

Show:

> Let's find your German level.

Explain:

> This isn't a test you can fail. It helps your coach understand where to start.

Assessment should evaluate:

* Vocabulary
* Grammar
* Reading
* Listening
* Basic writing
* Basic speaking where available

---

# 22. Assessment Result

Show:

```text
Your German Level

A1 — Developing

Great start!

Your strengths:
✓ Basic vocabulary
✓ Simple sentences

Focus areas:
→ Articles
→ Word order
→ Listening
```

Primary CTA:

> **Start My Learning Plan**

Do not overwhelm the user with detailed analytics at this stage.

---

# 23. Dashboard

The dashboard is the most important screen.

Its primary purpose is:

> **Tell the learner what to do next.**

---

# 24. Dashboard Layout

Recommended hierarchy:

```text
Greeting
↓
Current level
↓
Today's recommendation
↓
Smart review
↓
Daily goal
↓
Weak areas
↓
Progress
↓
XP / streak
```

Do not give every section equal visual weight.

---

# 25. Dashboard Header

Example:

> Guten Morgen! 👋

> Ready for today's German?

Below:

```text
A1 → A2

████████████░░░
68%
```

Keep the level visible but not dominant.

---

# 26. Today's Recommendation

This is the most important dashboard component.

Example:

```text
TODAY'S RECOMMENDATION

You struggled with German articles yesterday.

Let's practice:

der / die / das
+
ein / eine / einen

12 min

[ Start Lesson ]
```

The recommendation should be generated from the learner's current state.

---

# 27. Continue Learning

If the learner has an unfinished lesson:

```text
CONTINUE LEARNING

German Word Order

8 min · A1 · Grammar

[ Continue → ]
```

This should take priority over browsing the curriculum.

---

# 28. Smart Review

Example:

```text
SMART REVIEW

8 items are ready.

3 article mistakes
2 vocabulary items
2 grammar concepts
1 pronunciation item

[ Review Now ]
```

The user should understand why these items are being reviewed.

---

# 29. Daily Goal

Example:

```text
TODAY'S GOAL

18 / 30 minutes

████████████░░
```

Include:

* Time studied
* Goal
* Completion percentage

When complete:

> 🎉 Daily goal complete!

---

# 30. Weak Areas

Show only the most important weaknesses.

Example:

```text
FOCUS AREAS

Articles          62%
Listening         54%
Pronunciation     71%

[ Practice Weak Areas ]
```

Do not display ten weak areas.

Maximum:

> 3 important areas

---

# 31. Streak

Show:

```text
🔥 12

day streak
```

Keep it visually supportive rather than dominant.

---

# 32. XP

Show:

```text
1,240 XP

Level 6
German Explorer
```

XP should support learning rather than become the primary objective.

---

# 33. Learn Page

Purpose:

> Explore and follow the German learning roadmap.

Structure:

```text
Your Learning Path

A1
│
├── ✓ Introductions
├── ✓ Numbers & Time
├── → Daily Routine
├── 🔒 Food & Café
├── 🔒 Shopping
├── 🔒 Transportation
└── 🔒 Housing

A2
🔒
```

---

# 34. Lesson Cards

Each lesson card should display:

* Title
* CEFR level
* Estimated time
* Skill
* Status

Example:

```text
German Word Order

A1 · Grammar

8 min

In Progress

[ Continue ]
```

Avoid displaying excessive metadata.

---

# 35. Lesson Player

The lesson player should focus on one task at a time.

Structure:

```text
Lesson Progress

██████░░░░ 60%

German Word Order

[Lesson content]

[ Continue → ]
```

Avoid showing the entire lesson on one screen.

---

# 36. Lesson Step — Concept

Example:

```text
GERMAN WORD ORDER

In a simple German sentence,
the verb usually comes second.

Ich lerne Deutsch.

I am learning German.
```

CTA:

> Got it

---

# 37. Lesson Step — Vocabulary

Example:

```text
sprechen

to speak

🔊

Ich spreche Deutsch.

[ Listen Again ]
```

Allow:

* Audio
* Translation toggle
* Example

---

# 38. Lesson Step — Grammar

Example:

```text
Where does the verb go?

Ich ___ Deutsch.

A. lerne
B. Deutsch lerne
C. lernen
```

After selection:

Show feedback immediately.

---

# 39. Lesson Step — Listening

Display:

```text
LISTEN

🔊 Play audio

Listen and choose what you hear.

[ Answer options ]
```

Allow replay.

Do not autoplay audio unless appropriate.

---

# 40. Lesson Step — Speaking

Display:

```text
SPEAK

Say:

Ich lerne Deutsch.

🎙 Hold to speak
```

After recording:

```text
Analyzing...

Your pronunciation was understandable.

Try improving:
"Deutsch"
```

CTA:

> Try Again

or

> Continue

---

# 41. Lesson Step — Writing

Example:

```text
WRITE

Introduce yourself in German.

Type your answer:

[                         ]

[ Check Answer ]
```

AI feedback:

```text
Your sentence:
Ich bin Karthik. Ich arbeite als Tester.

Good!

More natural:
Ich arbeite als Softwaretester.
```

---

# 42. Lesson Completion

Celebrate without overwhelming the learner.

Example:

```text
Lesson Complete! 🎉

German Word Order

+50 XP

You practiced:
✓ Grammar
✓ Listening
✓ Speaking

New mastery:
Word Order ↑

[ Continue ]
```

Secondary:

> Review mistakes

---

# 43. Practice Page

Practice should provide targeted activities.

Categories:

```text
Grammar
Vocabulary
Listening
Speaking
Writing
```

Also show:

> Recommended for you

Example:

```text
Recommended

Articles

You have struggled with this concept recently.

[ Practice ]
```

---

# 44. Vocabulary Page

Vocabulary should be searchable.

Search:

```text
Search German words...
```

Example:

```text
die Wohnung

apartment

🔊

Plural:
die Wohnungen

Mastery:
██████░░ 72%

Next review:
Tomorrow
```

Filters:

* CEFR
* Topic
* Mastery
* Review status

---

# 45. Grammar Page

Show concepts by level.

Example:

```text
A1 Grammar

✓ Personal Pronouns
✓ sein
✓ haben
→ Articles
→ Word Order
🔒 Akkusativ
🔒 Modal Verbs
```

Selecting a concept opens:

* Explanation
* Examples
* Practice
* Related mistakes

---

# 46. Talk Page

The Talk page is the AI conversation area.

Header:

> **Talk in German**

Supporting text:

> Practice real conversations with your AI coach.

---

# 47. Conversation Scenario Selection

Categories:

### Daily Life

* Café
* Restaurant
* Shopping
* Train station
* Apartment
* Bank

### Work

* Office introduction
* Team meeting
* Asking for help
* Giving an update

### Career

* Job interview
* Professional introduction

### IT

* Bug discussion
* Release discussion
* Testing discussion

---

# 48. Conversation Mode Selection

Before starting:

```text
Choose your mode

Guided
AI gives hints

Normal
Natural conversation

Immersion
German only

Teacher
Conversation + explanations
```

---

# 49. Conversation Screen

Example:

```text
Café

AI Coach

Guten Tag!
Was möchten Sie bestellen?

🔊

You

[ Type your response ]

🎙

[ Send ]
```

Allow:

* Text input
* Microphone
* Audio playback
* Translation toggle
* Hint

---

# 50. Conversation Corrections

Do not correct every sentence.

Instead provide a correction summary periodically.

Example:

```text
QUICK FEEDBACK

You said:
Ich möchte ein Pizza.

Better:
Ich möchte eine Pizza.

Why:
Pizza is feminine.

We'll review this later.
```

The mistake should be added to the learner's learning history.

---

# 51. Review Page

The review page should feel personalized.

Header:

> **Smart Review**

Supporting text:

> Let's strengthen the areas you've been struggling with.

Display:

```text
8 items ready

3 Grammar
2 Vocabulary
2 Listening
1 Pronunciation

Estimated time:
6 min

[ Start Review ]
```

---

# 52. Review Interaction

One item at a time.

Example:

```text
Fill in the correct article:

Ich möchte ___ Pizza.

[ eine ]

[ Check ]
```

After answer:

```text
✓ Correct

This was one of your previous weak areas.

Mastery ↑
```

---

# 53. Progress Page

The progress page should answer:

> **Am I actually getting better?**

Display:

### Current Level

```text
A1 — Developing
```

### Skill progress

```text
Speaking       42%
Listening      54%
Reading        65%
Writing        55%
Grammar        62%
Vocabulary     71%
Pronunciation  58%
```

Use simple visualizations.

Do not create complicated charts in the MVP.

---

# 54. Progress Insights

Show meaningful statements.

Examples:

> Listening improved 12% this month.

> Article accuracy improved from 54% to 68%.

> You have mastered 42 A1 vocabulary items.

> Speaking is currently your biggest opportunity.

These insights should be based on actual learner data.

---

# 55. Profile Page

Sections:

### Learning Profile

* Native language
* German level
* Goals
* Interests
* Daily goal

### Learning Preferences

* Explanation language
* Learning style
* Conversation mode

### Account

* Email
* Password
* Sign out

### Settings

* Notifications
* Audio
* Privacy

---

# 56. Responsive Design

## Desktop

Use:

* Sidebar or top navigation
* Multi-column layouts where appropriate
* Wider lesson content
* Larger conversation area

## Tablet

Reduce columns.

Prioritize:

* Lesson
* Dashboard
* Conversation

## Mobile

Use:

* Bottom navigation
* Single-column layouts
* Large touch targets
* Sticky primary CTA where appropriate
* Full-screen lesson experience

---

# 57. Mobile Lesson UX

On mobile:

```text
┌────────────────────────────┐
│ ← Lesson          60%      │
│ ███████████░░░             │
│                            │
│ German Word Order          │
│                            │
│ Content                    │
│                            │
│                            │
│                            │
│                            │
│ [ Continue → ]             │
└────────────────────────────┘
```

Avoid unnecessary navigation while learning.

---

# 58. Audio UX

Audio buttons should be consistent.

States:

```text
🔊 Play
⏸ Playing
↻ Replay
```

Provide:

* Normal speed
* Slow speed where useful

Audio should never block the user from continuing if it fails.

---

# 59. Microphone UX

Before first use:

> German Coach needs microphone access to evaluate your speaking.

During recording:

```text
🎙 Listening...

Release to stop
```

After:

```text
Processing...
```

If permission is denied:

> Microphone access is required for speaking practice.

Provide instructions to enable it.

---

# 60. Error States

Errors should be human-friendly.

Bad:

> Error 500

Better:

> We couldn't load your lesson.

> Please try again.

CTA:

> Try Again

For AI failure:

> Your coach is taking a moment to respond.

> Try again.

Do not expose internal API errors.

---

# 61. Loading States

Use meaningful loading states.

Examples:

```text
Preparing your lesson...
Analyzing your answer...
Checking your pronunciation...
Preparing your review...
```

Avoid generic:

> Loading...

when possible.

---

# 62. Empty States

Example — Review:

```text
You're all caught up! 🎉

No reviews are due right now.

Come back tomorrow for your next review.
```

Example — Vocabulary:

```text
No saved vocabulary yet.

Words you learn will appear here.
```

---

# 63. Notifications

Do not overwhelm users.

Initial notification types:

* Daily learning reminder
* Review reminder
* Streak reminder

Allow users to disable notifications.

---

# 64. Gamification UX

Gamification should be subtle.

Use:

* XP
* Streak
* Achievements

Avoid:

* Constant animations
* Loud celebration screens
* Excessive badges
* Competitive pressure

The learner should focus on German, not points.

---

# 65. Accessibility

All screens must support:

* Keyboard navigation
* Visible focus states
* Accessible labels
* Sufficient contrast
* Screen readers
* Large touch targets
* Captions/transcripts for audio where applicable

Do not rely on color alone to communicate correctness.

Example:

Instead of only green:

> ✓ Correct

Instead of only red:

> ✕ Try again

---

# 66. UX Metrics

Measure:

### Onboarding

* Completion rate
* Drop-off per step

### Learning

* Lesson start rate
* Lesson completion rate
* Average lesson duration
* Exercise completion

### Speaking

* Speaking attempts
* Successful completion
* Retry rate

### Conversation

* Conversation starts
* Conversations completed
* Average turns

### Review

* Review starts
* Review completion

### Retention

* Day 1
* Day 7
* Day 30

---

# 67. UX Golden Path

The most important user flow is:

```text
Register
   ↓
Onboarding
   ↓
Assessment
   ↓
Learning Plan
   ↓
Dashboard
   ↓
Today's Recommendation
   ↓
Lesson
   ↓
Vocabulary
   ↓
Grammar
   ↓
Listening
   ↓
Speaking
   ↓
AI Feedback
   ↓
Lesson Complete
   ↓
XP
   ↓
Progress
   ↓
Smart Review
```

This path should be extremely polished.

---

# 68. MVP Screen List

The first implementation should contain:

```text
01 Landing
02 Login
03 Register
04 Onboarding
05 Assessment
06 Assessment Result
07 Dashboard
08 Learning Roadmap
09 Lesson
10 Exercise
11 Vocabulary
12 Practice
13 Conversation
14 Review
15 Progress
16 Profile
```

Do not create additional screens unless required by the core flow.

---

# 69. UX Definition of Done

Before moving to technical architecture:

```text
[ ] Landing UX approved
[ ] Registration flow approved
[ ] Onboarding flow approved
[ ] Assessment flow approved
[ ] Dashboard approved
[ ] Learning roadmap approved
[ ] Lesson flow approved
[ ] Speaking flow approved
[ ] Conversation flow approved
[ ] Review flow approved
[ ] Progress flow approved
[ ] Profile flow approved
[ ] Mobile navigation approved
[ ] Responsive behavior approved
[ ] Loading states defined
[ ] Empty states defined
[ ] Error states defined
[ ] Accessibility requirements defined
```

---

# 70. Next Step

After this UX/UI specification is reviewed and approved, create:

```text
PRD/
├── 01-product-definition.md
├── 02-ux-ui-specification.md
└── 03-technical-architecture.md
```

**Step 3 — Technical Architecture** will define:

* Frontend architecture
* Backend architecture
* PostgreSQL schema
* API structure
* Authentication
* AI service architecture
* Audio architecture
* Learning engine
* Spaced repetition
* Error tracking
* Gamification
* Infrastructure
* Security
* Testing architecture
* CI/CD

Do not begin implementation until Step 3 is sufficiently defined.
