German Language Coach

Product Requirements Document — Step 9: Content Curriculum

Version: 0.1
Status: Draft

1. Purpose

The Content Curriculum defines what the German Language Coach teaches, in what order, at what difficulty, and through which learning activities.

The curriculum must provide a structured path from beginner German toward practical A2 competence while allowing the Learning Engine to personalize the sequence.

The curriculum is the content foundation for:

Lessons

Concepts

Vocabulary

Grammar

Reading

Listening

Speaking

Writing

Exercises

Reviews

Assessments

AI Coach activities

2. Core Principle

The application should not be an unstructured collection of German lessons.

It should provide:

CEFR Level
    ↓
Module
    ↓
Lesson
    ↓
Concept
    ↓
Practice
    ↓
Review
    ↓
Mastery

The learner should always know:

What am I learning, why am I learning it, and what should I learn next?

3. MVP Curriculum Scope

The MVP should focus on:

A1
A2

Future levels:

B1
B2
C1
C2

The initial release should prioritize a high-quality A1 curriculum rather than attempting to create every CEFR level immediately.

4. CEFR Structure

The curriculum should use CEFR as the organizing framework.

A1 → Beginner
A2 → Elementary
B1 → Intermediate
B2 → Upper Intermediate
C1 → Advanced
C2 → Mastery

For MVP:

A1 → Full curriculum
A2 → Initial curriculum / expansion-ready

5. Curriculum Architecture

German
│
├── A1
│   ├── Module 1: Foundations
│   ├── Module 2: Introducing Yourself
│   ├── Module 3: Everyday Life
│   ├── Module 4: Family & People
│   ├── Module 5: Food & Shopping
│   ├── Module 6: Home & City
│   ├── Module 7: Time & Routines
│   ├── Module 8: Work & Communication
│   └── Module 9: A1 Review & Assessment
│
└── A2
    ├── Module 1: Everyday Communication
    ├── Module 2: Past Experiences
    ├── Module 3: Travel
    ├── Module 4: Health
    ├── Module 5: Work
    ├── Module 6: Social Situations
    ├── Module 7: Opinions & Plans
    └── Module 8: A2 Review & Assessment

The exact module count can change as content is validated.

6. Learning Domains

Every curriculum concept should belong to one or more domains:

Vocabulary
Grammar
Pronunciation
Reading
Listening
Speaking
Writing
Communication

The main skill categories remain:

Vocabulary
Grammar
Reading
Listening
Speaking
Writing

7. Lesson Structure

Each lesson should follow:

1. Objective
2. Introduction
3. Explanation
4. Examples
5. Guided Practice
6. Independent Practice
7. Application
8. Review

Not every lesson needs all sections to be visible to the learner.

8. Lesson Objective

Every lesson must have a measurable objective.

Bad:

Learn German articles.

Better:

By the end of this lesson, the learner can
identify and use der, die, and das with common nouns.

9. Concept

A concept is a teachable learning unit.

Examples:

Personal Pronouns
sein
haben
German noun gender
der
die
das
Accusative articles
Present tense
Modal verbs
Separable verbs

A lesson may contain several concepts.

10. Vocabulary Item

Each vocabulary item should contain:

German word
English meaning
Part of speech
Gender, if applicable
Plural form, if applicable
Pronunciation
Example sentence
CEFR level
Topic
Concept association

Example:

{
  "word": "der Tisch",
  "meaning": "the table",
  "partOfSpeech": "NOUN",
  "gender": "MASCULINE",
  "plural": "die Tische",
  "level": "A1",
  "topic": "HOME"
}

11. Noun Content Rule

German nouns should preferably be taught with their article.

Use:

der Tisch
die Lampe
das Buch

rather than:

Tisch
Lampe
Buch

This helps learners develop grammatical gender knowledge from the beginning.

12. Verb Content Rule

Verbs should include:

Infinitive
Meaning
Present tense forms where relevant
Example sentence
Common usage

Example:

gehen
to go

Ich gehe zur Arbeit.

13. Grammar Curriculum

The A1 grammar progression should include concepts such as:

Personal pronouns
sein
haben
Regular verbs
Basic word order
Questions
Negation
Noun gender
Definite articles
Indefinite articles
Plural forms
Possessives
Accusative
Modal verbs
Separable verbs
Imperatives
Basic prepositions
Time expressions

The final ordering should be validated against the selected CEFR-aligned curriculum.

14. A1 Module 1 — Foundations

Focus:

German pronunciation
Alphabet
Greetings
Basic classroom phrases
Numbers
Yes / No
Basic question words

Example lessons:

1. German Sounds
2. Greetings
3. Introducing Yourself
4. Numbers 0–20
5. Basic Questions

Core communication:

Hallo!
Guten Morgen!
Wie geht es dir?
Ich heiße ...
Ich komme aus ...
Ich spreche ...

15. A1 Module 2 — Introducing Yourself

Topics:

Name
Age
Country
Nationality
Languages
Occupation
Basic personal information

Grammar:

sein
Personal pronouns
Basic sentence structure
Question formation

Speaking goal:

Introduce yourself in German.

16. A1 Module 3 — Everyday Life

Topics:

Daily activities
Common verbs
Time
Days
Months
Routine

Grammar:

Present tense
Verb position
Time expressions

Speaking goal:

Describe a normal day.

17. A1 Module 4 — Family & People

Topics:

Family members
Relationships
Personal descriptions
Jobs
Basic adjectives

Grammar:

Possessives
Articles
Plural
Basic adjective usage

Speaking goal:

Describe your family.

18. A1 Module 5 — Food & Shopping

Topics:

Food
Drinks
Restaurants
Shopping
Prices
Quantities
Preferences

Grammar:

Accusative introduction
möchten
können
Basic negation

Speaking goal:

Order food and buy basic items.

19. A1 Module 6 — Home & City

Topics:

Rooms
Furniture
Places in a city
Directions
Transportation

Grammar:

Location expressions
Basic prepositions
Imperative

Speaking goal:

Ask for and understand simple directions.

20. A1 Module 7 — Time & Routines

Topics:

Appointments
Schedules
Daily routines
Free time
Hobbies

Grammar:

Time expressions
Frequency
Separable verbs
Modal verbs

Speaking goal:

Talk about your schedule.

21. A1 Module 8 — Work & Communication

This module is particularly important for learners interested in working in Germany.

Topics:

Job
Workplace
Colleagues
Basic professional communication
Meetings
Working hours
Simple requests

Vocabulary examples:

die Arbeit
der Beruf
der Kollege
die Kollegin
der Chef
die Aufgabe
der Termin
das Büro

Communication goals:

Introduce yourself professionally.
Ask simple workplace questions.
Understand basic instructions.
Schedule a simple appointment.

22. A1 Module 9 — Review & Assessment

Review:

Vocabulary
Grammar
Listening
Reading
Speaking
Writing

Assessment should determine:

A1 readiness
Weak concepts
Weak skills
Review priorities

Passing the assessment does not automatically mean complete mastery of every concept.

23. A2 Curriculum

A2 should extend A1 rather than simply repeat it.

Focus areas:

Past experiences
Travel
Health
Workplace communication
Social interactions
Plans
Opinions
More complex sentences

Grammar expansion:

Perfekt
Dative
More prepositions
Subordinate clauses
Comparatives
Reflexive verbs
More modal constructions

24. A2 Practical Communication

The learner should gradually be able to:

Describe past events
Talk about plans
Explain simple problems
Describe experiences
Handle common travel situations
Discuss work activities
Express preferences
Give simple opinions

25. Workplace German Track

The application should eventually support a dedicated goal-oriented track:

General German
        +
Workplace German

Workplace modules may include:

Job applications
CV vocabulary
Interviews
Office communication
Email writing
Meetings
Deadlines
Tasks
Feedback
Professional introductions

This track should use the same underlying concepts and Learning Engine.

26. Lesson Difficulty

Every lesson should have:

CEFR level
Difficulty
Prerequisites
Estimated duration

Example:

{
  "level": "A1",
  "difficulty": 2,
  "estimatedMinutes": 10,
  "prerequisites": [
    "personal-pronouns"
  ]
}

27. Exercise Types

The MVP should support:

Multiple Choice
Fill in the Blank
Translation
Sentence Ordering
Matching
True / False
Listening Question
Reading Question
Short Writing
Speaking Prompt
Conversation

28. Exercise Selection

Exercise type should depend on the learning objective.

Example:

Vocabulary recognition
→ Multiple Choice

Grammar word order
→ Sentence Ordering

Speaking ability
→ Speaking Prompt

Writing ability
→ Short Writing

Listening comprehension
→ Listening Question

29. Exercise Diversity

A concept should not be tested using only one format.

Example:

Concept:
Accusative Articles

Exercise 1:
Multiple choice

Exercise 2:
Fill in blank

Exercise 3:
Sentence construction

Exercise 4:
Translation

Exercise 5:
Conversation

This tests transfer rather than memorization.

30. Review Content

Review exercises should come from previously learned concepts.

Review selection is controlled by the Learning Engine.

Content should include:

Recent concepts
Weak concepts
Overdue concepts
Frequently mistaken concepts
Important prerequisite concepts

31. AI-Generated Content

AI can generate:

Example sentences
Conversation prompts
Practice questions
Reading passages
Writing prompts
Explanations
Alternative examples

AI-generated content must follow the curriculum and concept metadata.

32. AI Content Constraint

AI must not freely invent curriculum progression.

The system should provide:

CEFR level
Module
Lesson
Concept
Target vocabulary
Grammar rules
Difficulty
Exercise type

Then AI generates content within those boundaries.

33. AI Content Pipeline

Curriculum Definition
        ↓
Content Specification
        ↓
AI Generation
        ↓
Validation
        ↓
Quality Check
        ↓
Database
        ↓
Learner

34. AI Content Validation

Generated content should be validated for:

Correct German
Correct English explanation
Correct grammar
Correct CEFR difficulty
Target concept coverage
No unsupported claims
No inappropriate content
No accidental answer leakage

35. Static vs AI Content

Use predefined content for:

Core grammar explanations
Core curriculum
Important vocabulary
Assessment questions
Critical learning rules

Use AI for:

Conversation
Personalized examples
Extra practice
Alternative explanations
Dynamic scenarios

This reduces AI cost and improves consistency.

36. Free AI Architecture

Because the MVP should use free or very-low-cost AI services:

Core curriculum
→ stored in database

Core exercises
→ stored in database

AI
→ used only where personalization provides value

Do not require an AI call for every exercise.

37. Content Versioning

Every content item should have:

version
status
createdAt
updatedAt

Possible statuses:

DRAFT
REVIEW
PUBLISHED
ARCHIVED

38. Content Ownership

Content should have a source:

CURRICULUM
AI_GENERATED
ADMIN_CREATED
IMPORTED

AI-generated content should remain identifiable.

39. Content Quality

Every published content item should pass basic quality checks.

Minimum:

Grammar correct
Meaning correct
Level appropriate
No duplicate
Concept correctly mapped
Answer validated

40. Content Database Structure

Recommended entities:

Curriculum
Level
Module
Lesson
Concept
Vocabulary
GrammarRule
Exercise
ExerciseOption
Reading
Listening
SpeakingPrompt
WritingPrompt
ContentVersion

41. Curriculum

Example fields:

id
name
language
targetLanguage
version
status

42. Module

Example:

id
curriculumId
levelId
title
description
orderIndex

43. Lesson

Example:

id
moduleId
title
description
objective
estimatedMinutes
difficulty
orderIndex
status

44. Concept

Example:

id
name
type
level
description
difficulty
masteryThreshold

Types:

VOCABULARY
GRAMMAR
PRONUNCIATION
COMMUNICATION

45. Lesson-Concept Mapping

A lesson may teach multiple concepts.

Use a mapping structure:

LessonConcept

Fields:

lessonId
conceptId
isPrimary
orderIndex

46. Prerequisites

Concept prerequisites should be explicit.

Example:

Accusative Articles
    ↓
requires
    ↓
German Articles

Recommended table:

ConceptPrerequisite

47. Vocabulary Table

Example fields:

id
conceptId
word
translation
partOfSpeech
gender
plural
pronunciation
exampleSentence
level
topic

48. Exercise Table

Example fields:

id
lessonId
conceptId
type
difficulty
question
answer
explanation
source
version
status

Do not expose correct answers to the client before the learner submits an answer.

49. Exercise Options

For multiple choice:

id
exerciseId
text
isCorrect
orderIndex

Correct answers must be protected server-side.

50. Reading Content

Reading content should contain:

title
text
level
concepts
vocabulary
questions
difficulty

A1 texts should be short and practical.

51. Listening Content

Listening content should contain:

title
transcript
audio reference
level
concepts
questions
difficulty

The transcript may be hidden until the learner requests help or completes the exercise.

52. Speaking Content

Speaking prompts should include:

prompt
expectedLevel
targetConcepts
targetVocabulary
estimatedMinutes
evaluationCriteria

Example:

Introduce yourself to a new colleague.

53. Writing Content

Writing prompts should include:

prompt
level
targetConcepts
targetVocabulary
minimumExpectation
evaluationCriteria

Example:

Write a short message to your colleague
about a meeting appointment.

54. Content Tags

Content should support tags such as:

A1
A2
GRAMMAR
VOCABULARY
WORK
TRAVEL
FOOD
HOME
FAMILY
SPEAKING
LISTENING

Tags help the Learning Engine select relevant content.

55. Search

Content should be searchable by:

Level
Module
Lesson
Concept
Skill
Topic
Difficulty
Exercise type

56. Content Recommendation

The Learning Engine should select content based on:

Learner mastery
Prerequisites
Review schedule
Current lesson
Goal
Difficulty
Skill weakness
Recent mistakes

The curriculum defines the available content; the Learning Engine chooses what the learner should do next.

57. Curriculum Progression

The default progression should be:

Foundational concepts
        ↓
Basic communication
        ↓
Grammar expansion
        ↓
Practical situations
        ↓
Application
        ↓
Review
        ↓
Assessment

Learners may deviate when the Learning Engine determines that review or remediation is needed.

58. Remediation

If a learner repeatedly fails a concept:

Failed concept
     ↓
Identify prerequisite
     ↓
Review prerequisite
     ↓
Simpler exercise
     ↓
Alternative explanation
     ↓
Retry original concept

This prevents learners from simply being pushed forward.

59. Enrichment

If a learner consistently performs well:

Strong performance
     ↓
Increase exercise difficulty
     ↓
Introduce contextual application
     ↓
Optional enrichment

Do not skip essential prerequisites.

60. Lesson Completion

A lesson should require:

Required activities completed
+
Minimum lesson criteria

Completion is separate from mastery.

Example:

Lesson completed:
YES

Concept mastery:
58%

The learner can continue while the Learning Engine schedules additional review.

61. Curriculum Completion

A CEFR level should not be considered complete simply because all lessons were opened.

Use:

Curriculum completion
+
Concept mastery
+
Skill evidence
+
Assessment

62. Assessment Blueprint

Each level assessment should contain a balanced sample of:

Vocabulary
Grammar
Reading
Listening
Writing
Speaking

The exact distribution should be configurable.

63. Assessment Content

Assessment questions should preferably be stable and versioned.

AI may assist with open-ended evaluation, but the assessment framework should remain controlled.

64. Content Analytics

Track:

Exercise completion rate
Exercise accuracy
Average attempts
Drop-off rate
Time spent
Most difficult concepts
Most common mistakes
Content reuse

This helps improve the curriculum.

65. Content Feedback

Learners should be able to report:

Wrong answer
Confusing explanation
Incorrect German
Too easy
Too difficult
Audio problem
Other

These reports should enter a content review queue.

66. Content Moderation

All published AI-generated content should pass automated validation.

High-risk or questionable content should require manual review.

For MVP, the safest approach is:

AI generates
     ↓
Validation
     ↓
Optional admin approval
     ↓
Publish

67. Content Deduplication

Avoid repeatedly generating identical exercises.

Compare:

Question
Concept
Target answer
Exercise type

before publishing new content.

68. Localization

The initial UI/content explanation language may be:

English

The target language is:

German

Future support:

Tamil
Hindi
Other languages

The curriculum data model should not hard-code English as the only explanation language.

69. Content Accessibility

Lessons should support:

Readable typography
Keyboard navigation
Screen-reader friendly structure
Audio controls
Captions/transcripts where applicable
Clear error messages

70. Mobile-First Content

Lessons should work well on mobile.

Avoid:

Large text blocks
Complex tables
Long forms
Tiny controls

Prefer:

Short sections
Cards
Progressive disclosure
One task at a time

71. Lesson Length

Recommended MVP lesson duration:

5–15 minutes

Longer lessons can be split into smaller activities.

This supports daily habit formation.

72. Microlearning

A learner should be able to make progress in:

5 minutes
10 minutes
15 minutes
30 minutes

The Learning Engine should assemble activities based on available time.

73. Curriculum Content Ratio

A practical starting ratio:

30% Vocabulary
25% Grammar
15% Reading
10% Listening
10% Speaking
10% Writing

This is configurable and should be validated using learner outcomes.

74. Practical German Principle

Content should prioritize language the learner can actually use.

Prefer:

Useful phrases
Common vocabulary
Everyday situations
Workplace situations
High-frequency grammar

over:

Rare vocabulary
Uncommon grammar
Long theoretical explanations

75. Example A1 Lesson

Lesson:
Introducing Yourself

Objective:
Introduce yourself in simple German.

Concepts:
- Personal pronouns
- sein
- Basic sentence order

Vocabulary:
- Name
- Herkunft
- Beruf
- Sprache

Examples:
Ich heiße Anna.
Ich komme aus Indien.
Ich bin Softwareentwicklerin.
Ich spreche Englisch.

Practice:
1. Choose the correct sentence.
2. Fill in the blank.
3. Build a sentence.
4. Translate.
5. Speak for 30 seconds.

Review:
Personal pronouns
sein

76. Example Workplace Lesson

Lesson:
Introducing Yourself to a Colleague

Objective:
Introduce yourself professionally.

Vocabulary:
Kollege
Büro
Team
Aufgabe
Projekt
Beruf

Grammar:
sein
kommen aus
arbeiten als

Practice:
Multiple choice
Sentence building
Listening
Speaking

AI conversation:
"Du bist neu im Team. Stelle dich vor."

77. Content Generation Prompt Contract

When AI generates an exercise, provide structured context:

{
  "level": "A1",
  "module": "Work & Communication",
  "lesson": "Introducing Yourself",
  "concepts": [
    "personal-pronouns",
    "sein"
  ],
  "skill": "SPEAKING",
  "difficulty": 2,
  "exerciseType": "CONVERSATION"
}

The AI must return structured output.

78. AI Output Validation

Expected output should contain:

exercise
answer
explanation
targetConcepts
difficulty

Reject content when:

Required fields missing
Invalid concept
Wrong level
Malformed JSON
Answer mismatch
Unsafe content

79. Content API

Possible endpoints:

GET /api/curriculum
GET /api/curriculum/levels
GET /api/curriculum/modules
GET /api/curriculum/lessons/:id
GET /api/curriculum/concepts/:id
GET /api/curriculum/lessons/:id/exercises
GET /api/content/vocabulary
GET /api/content/review

80. Admin Content API

Future/admin endpoints:

POST /api/admin/content
PUT /api/admin/content/:id
DELETE /api/admin/content/:id
POST /api/admin/content/:id/publish
POST /api/admin/content/:id/archive

These endpoints must require elevated authorization.

81. Testing

Test:

Curriculum ordering
Prerequisites
Lesson loading
Concept mapping
Exercise correctness
CEFR mapping
Difficulty
AI output validation
Content versioning
Duplicate detection
Assessment coverage

82. Curriculum Tests

Example:

A1 Foundations
→ must be available before advanced A1 modules.

Example:

Accusative Articles
→ cannot be unlocked before required article concepts.

83. Content Quality Tests

For every exercise:

[ ] Has valid concept
[ ] Has valid level
[ ] Has valid difficulty
[ ] Has answer
[ ] Has explanation
[ ] Has exercise type
[ ] Has no duplicate
[ ] Passes grammar validation

84. MVP Content Scope

Recommended initial launch:

A1
├── 8–10 modules
├── 5–8 lessons per module
├── 3–5 concepts per lesson
├── 5–10 exercises per lesson
└── Review content generated from existing concepts

This gives a meaningful MVP without requiring thousands of manually authored exercises.

85. AI-Generated Expansion

Once the core curriculum is stable:

Core exercises
        +
AI-generated variations
        ↓
Large practice pool

This is preferable to manually storing thousands of near-identical exercises.

86. Content Cost Strategy

To keep the app free:

Store reusable content
        ↓
Generate AI variations only when needed
        ↓
Cache generated content
        ↓
Reuse validated content

Do not generate content repeatedly for the same learner and concept unless personalization requires it.

87. Content Caching

Cache reusable AI-generated content by:

Concept
Level
Difficulty
Exercise type
Language

Example:

A1 + Articles + Difficulty 2 + Fill Blank

88. Content Freshness

Generated content should have:

createdAt
version
source
validationStatus

Poor-performing content can be archived without affecting historical learner records.

89. Historical Integrity

If content changes after a learner completes an exercise:

Historical attempt
→ keep original reference/version

Do not rewrite historical learning records.

90. Content Analytics Loop

Use learner data to improve content:

Learner attempts
      ↓
Analytics
      ↓
Identify difficult concepts
      ↓
Review content
      ↓
Improve exercise
      ↓
Republish version

91. Curriculum Governance

The curriculum should have one authoritative structure.

Avoid allowing:

Frontend
AI
Learning Engine
Admin

to independently define curriculum ordering.

The curriculum database/configuration is authoritative.

92. Definition of Done

The Content Curriculum system is complete for MVP when:

[ ] A1 curriculum structure exists
[ ] A2 structure is defined or expansion-ready
[ ] Modules are defined
[ ] Lessons are defined
[ ] Concepts are defined
[ ] Prerequisites are defined
[ ] Vocabulary structure exists
[ ] Grammar structure exists
[ ] Exercise types exist
[ ] Reading structure exists
[ ] Listening structure exists
[ ] Speaking structure exists
[ ] Writing structure exists
[ ] Lesson objectives are measurable
[ ] Content has CEFR mapping
[ ] Content has difficulty
[ ] Content can be versioned
[ ] AI-generated content can be validated
[ ] Core content can work without AI
[ ] Content can be reused by the Learning Engine
[ ] Content APIs are defined
[ ] Content quality tests exist

93. Final Architecture

                       CURRICULUM
                           │
                           ▼
                    MODULE / LESSON
                           │
                           ▼
                        CONCEPT
                           │
          ┌────────────────┼────────────────┐
          ▼                ▼                ▼
      VOCABULARY        GRAMMAR          SKILLS
          │                │                │
          └────────────────┼────────────────┘
                           ▼
                       EXERCISES
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
            STATIC         AI        REVIEW
            CONTENT      CONTENT      CONTENT
              │            │            │
              └────────────┼────────────┘
                           ▼
                    LEARNING ENGINE
                           │
                           ▼
                         USER

94. Key Architectural Rule

The curriculum defines what can be learned. The Learning Engine decides what should be learned next. AI generates or adapts content within those boundaries.

This separation prevents the AI from becoming an uncontrolled curriculum engine.

95. Product Outcome

The learner should experience:

I know where I am.
      ↓
I know what I am learning.
      ↓
I understand why it matters.
      ↓
I practice it in different ways.
      ↓
The system remembers my weaknesses.
      ↓
I review at the right time.
      ↓
I apply German to real situations.
      ↓
I progress through A1 → A2 → B1...

The ultimate curriculum goal is:

Teach practical German progressively, measurably, and in a way that adapts to the learner without losing a structured CEFR foundation
