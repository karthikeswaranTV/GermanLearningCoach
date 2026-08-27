Product Requirements Document — Step 6: AI Coach & Prompt Architecture

Version: 0.1
Status: Draft
Depends on:

01-product-definition.md

02-ux-ui-specification.md

03-technical-architecture.md

04-database-design.md

05-api-specification.md

1. Purpose

The AI Coach is the intelligence layer of the German Language Coach.

It should behave like a personalized German teacher rather than a generic chatbot.

The AI Coach must:

Understand the learner's current level

Understand the learner's goals

Adapt explanations to the learner

Correct mistakes

Explain why something is wrong

Generate useful practice

Conduct conversations

Evaluate writing

Support speaking practice

Identify recurring weaknesses

Recommend what to learn next

Encourage the learner

Avoid overwhelming beginners

The AI must work with the application's learning engine rather than replacing it.

2. Primary Design Goal

The AI Coach should answer:

"Given what this learner knows, what they are trying to achieve, what they are currently learning, and where they are struggling, what is the best response or learning activity right now?"

3. Free Cloud AI Strategy

The MVP should target:

₹0 AI inference cost

The architecture must use cloud AI providers with free tiers rather than requiring a local model.

Primary provider:

Google Gemini API

Fallback provider:

Groq

Optional provider:

OpenRouter free models

The exact model must be configurable through environment variables and must not be hardcoded throughout the application.

Free-tier quotas and model availability can change. Provider limits must therefore be configuration-driven and verified before release.

4. Provider Architecture

Never allow application modules to directly call Gemini or Groq.

Use:

Application
     ↓
AIService
     ↓
AIProvider
     ↓
Provider Adapter
     ↓
Cloud AI

Example:

AIService
   │
   ├── GeminiProvider
   │
   ├── GroqProvider
   │
   └── OpenRouterProvider

5. Provider Interface

Create a common interface.

Example:

interface AIProvider {
  generate(
    request: AIRequest
  ): Promise<AIResponse>;

  generateStructured<T>(
    request: AIRequest
  ): Promise<T>;
}

The application must not care which provider is being used.

6. AI Service

Create:

AIService

Responsibilities:

evaluateAnswer()
evaluateWriting()
evaluateSpeaking()
generateExercise()
explainGrammar()
generateConversationResponse()
analyzeMistake()
generateReview()
generateRecommendation()
estimateLevel()

7. Provider Selection

Default:

Gemini

Fallback:

Groq

Example:

Request
   ↓
Gemini
   ↓
Success
   ↓
Return

or

Gemini
   ↓
Quota / temporary failure
   ↓
Groq
   ↓
Return

Only retry automatically when the error is retryable.

Do not retry endlessly.

8. Free Tier Principle

The application must be designed around the assumption that free cloud AI has limited:

Requests per minute

Requests per day

Input tokens

Output tokens

Concurrent requests

Therefore:

Every AI request must have a reason to exist.

Avoid unnecessary AI calls.

9. Reduce AI Usage

Prefer deterministic application logic when AI is not necessary.

For example:

Vocabulary lookup
→ Database

Lesson metadata
→ Database

XP calculation
→ Backend

Streak calculation
→ Backend

Review scheduling
→ Backend

Mastery calculation
→ Backend

Use AI for:

Correction
Conversation
Personalized explanation
Speaking evaluation
Writing evaluation
Content generation
Adaptive feedback

10. AI Request Budget

Each user interaction should use the minimum AI calls possible.

Example:

Bad

Exercise answer
 ↓
AI call 1 → Is it correct?
 ↓
AI call 2 → Explain
 ↓
AI call 3 → Find mistake
 ↓
AI call 4 → Generate feedback

Good

Exercise answer
 ↓
One structured AI request
 ↓
Correctness
+
Correction
+
Explanation
+
Mistake

11. AI Request Structure

Every request should contain only the context required for the task.

Example:

{
  "task": "EVALUATE_ANSWER",
  "learner": {
    "level": "A1",
    "nativeLanguage": "English"
  },
  "learningContext": {
    "topic": "Food",
    "concept": "German Articles"
  },
  "question": {
    "text": "Ich möchte ___ Pizza.",
    "expectedType": "ARTICLE"
  },
  "answer": "ein"
}

Do not send the complete user history.

12. Learner Context

The AI Coach should have access to a compact learner context.

Example:

{
  "level": "A1",
  "goal": "WORK_IN_GERMANY",
  "nativeLanguage": "English",
  "currentSkills": {
    "grammar": 62,
    "vocabulary": 71,
    "speaking": 48,
    "listening": 54
  },
  "weakAreas": [
    "articles",
    "word_order"
  ],
  "recentMistakes": [
    {
      "concept": "feminine_articles",
      "frequency": 3
    }
  ]
}

13. Context Hierarchy

AI context should follow this priority:

1. System instructions
2. Task instructions
3. Learner profile
4. Current learning context
5. Relevant learner history
6. Current user input

Do not allow user-generated text to override system instructions.

14. Context Window Management

Do not send unlimited conversation history.

Use:

Current conversation
+
Recent relevant messages
+
Conversation summary
+
Relevant mistakes

For long conversations:

Old messages
   ↓
Summarize
   ↓
Store summary
   ↓
Use summary instead of full history

15. AI Coach Personality

The AI Coach should be:

Friendly

Patient

Encouraging

Clear

Practical

Supportive

Beginner-friendly

It should NOT be:

Condescending

Overly verbose

Constantly correcting

Aggressive

Judgmental

16. German Teaching Philosophy

The coach should prefer:

Explain → Practice → Apply → Review

rather than:

Explain everything → Ask learner to memorize everything

17. Language Strategy

For A1 learners:

Use:

Simple German
+
Native-language explanation when needed

Example:

Das ist eine Pizza.

"Pizza" ist feminin.
Deshalb sagen wir "eine Pizza".

Avoid unnecessarily advanced German explanations.

As the learner progresses, gradually increase German usage.

18. Correction Philosophy

Not every mistake requires a long explanation.

The AI should classify mistakes:

MINOR
IMPORTANT
RECURRING
CRITICAL_FOR_MEANING

Example:

Ich habe gegangen.

Correction:
Ich bin gegangen.

Explanation:
Bei "gehen" verwenden wir im Perfekt "sein".

19. Correction Output

Correction responses should be structured.

Example:

{
  "correct": false,
  "original": "Ich möchte ein Pizza.",
  "correction": "Ich möchte eine Pizza.",
  "category": "ARTICLE",
  "concept": "FEMININE_ARTICLE",
  "severity": "IMPORTANT",
  "explanation": "Pizza is a feminine noun, so we use 'eine'.",
  "naturalAlternative": null
}

20. Do Not Hallucinate Corrections

The AI must distinguish between:

Definitely incorrect
Probably incorrect
Acceptable
Natural alternative

Do not mark a grammatically valid sentence as incorrect merely because another phrasing is more natural.

21. Naturalness

The AI should distinguish:

Grammatically correct

Ich möchte einen Kaffee.

More conversational

Ich hätte gerne einen Kaffee.

The second should not be presented as a correction of the first.

It is a natural alternative.

22. Grammar Explanation

Grammar explanations should follow:

Rule
 ↓
Simple explanation
 ↓
Example
 ↓
Counter-example
 ↓
Practice

Example:

Rule:
"Pizza" is feminine.

Therefore:
eine Pizza

Example:
Ich esse eine Pizza.

Practice:
Ich kaufe ___ Pizza.

23. Exercise Generation

AI-generated exercises must contain:

question
type
expectedAnswer
acceptableAnswers
concept
difficulty
explanation

Example:

{
  "question": "Ich kaufe ___ Apfel.",
  "type": "FILL_BLANK",
  "expectedAnswer": "einen",
  "acceptableAnswers": [],
  "concept": "ACCUSATIVE_MASCULINE_ARTICLE",
  "difficulty": "A1",
  "explanation": "Apfel is masculine and is the direct object."
}

24. Exercise Validation

AI-generated exercises must be validated before being shown.

Validation must check:

[ ] Required fields exist
[ ] Valid exercise type
[ ] Valid CEFR level
[ ] Expected answer exists
[ ] Explanation exists
[ ] No contradictory answers
[ ] No inappropriate content

25. Lesson Generation

AI may generate lesson material, but published core curriculum should preferably be curated.

Recommended approach:

Core A1 curriculum
→ Curated/static

Personalized exercises
→ AI generated

This improves consistency and reduces AI usage.

26. Conversation Coach

Conversation mode should simulate realistic German interactions.

Examples:

At a restaurant
At a supermarket
Introducing yourself
At work
Meeting a colleague
Doctor appointment
Apartment viewing
Job interview
Train station

27. Conversation Rules

The AI should:

Stay within the learner's level.

Keep responses reasonably short.

Encourage the learner to respond.

Avoid answering its own questions.

Adapt difficulty gradually.

Correct important recurring mistakes.

Preserve conversation flow.

28. Conversation Example

Learner:

Ich möchte ein Kaffee.

Coach:

Natürlich!

Kleine Korrektur:
"Ich möchte einen Kaffee."

Was möchten Sie noch bestellen?

Do not turn every conversation into a grammar lecture.

29. Conversation Difficulty

Difficulty should adapt based on:

Learner level
+
Recent performance
+
Conversation performance

Example:

A1:
Short sentences

A2:
Longer sentences

B1:
More spontaneous conversation

B2:
Natural discussion and workplace scenarios

30. Speaking Evaluation

Speaking flow:

Audio
 ↓
Speech-to-text
 ↓
Transcript
 ↓
AI evaluation
 ↓
Structured result
 ↓
Learning engine

AI should evaluate:

Grammar
Vocabulary
Fluency
Comprehensibility
Pronunciation signals available from the pipeline

Do not claim phonetic precision if the underlying speech system does not provide sufficient evidence.

31. Speaking Output

Example:

{
  "transcript": "Ich möchte einen Kaffee.",
  "score": 82,
  "grammar": 90,
  "fluency": 75,
  "vocabulary": 80,
  "comprehensibility": 85,
  "feedback": [
    "Good sentence structure.",
    "Try speaking slightly more slowly."
  ],
  "mistakes": []
}

32. Writing Evaluation

Writing evaluation should identify:

Grammar
Vocabulary
Spelling
Word order
Naturalness
Meaning

Example:

{
  "score": 78,
  "correctedText": "Ich gehe morgen zur Arbeit.",
  "issues": [
    {
      "category": "WORD_ORDER",
      "original": "...",
      "correction": "...",
      "explanation": "..."
    }
  ]
}

33. Assessment AI

AI may be used to evaluate open-ended answers.

However:

Final level calculation must remain controlled by the application's assessment engine.

AI provides evidence.

The backend calculates the final result.

34. CEFR Awareness

The AI must know the learner's CEFR level.

For example:

A1:
Basic vocabulary
Simple present tense
Simple questions
Everyday situations

A2:
Routine communication
Past events
More varied vocabulary

B1:
Opinions
Reasons
Workplace communication
Longer conversations

B2:
Nuanced discussion
Professional communication
Complex grammar

Do not teach B2 concepts to an A1 learner unless explicitly requested.

35. Mistake Extraction

The AI should identify meaningful mistakes.

Example:

{
  "category": "ARTICLE",
  "concept": "FEMININE_ARTICLE",
  "original": "ein Pizza",
  "correction": "eine Pizza",
  "confidence": 0.98
}

The backend should decide whether to persist the mistake based on validation and business rules.

36. Mistake Deduplication

Do not create a new database mistake record for every occurrence.

Example:

ein Pizza
eine Pizza

should map to the same underlying concept:

FEMININE_ARTICLE

The occurrence count should increase.

37. Recommendation Engine

AI may provide recommendations, but the final recommendation should be generated using application data.

Inputs:

Mastery
+
Recent mistakes
+
Due reviews
+
Current lesson
+
User goal
+
Daily time

Output:

Next best learning activity

38. Recommendation Example

{
  "type": "REVIEW",
  "concept": "FEMININE_ARTICLE",
  "reason": "You made three article mistakes recently.",
  "estimatedMinutes": 8
}

39. AI Prompt Architecture

Prompts must be modular.

Recommended structure:

prompts/
│
├── system/
│   └── coach.md
│
├── assessment/
│   ├── evaluate-answer.md
│   └── evaluate-open-answer.md
│
├── lessons/
│   ├── explain-concept.md
│   └── generate-exercise.md
│
├── correction/
│   ├── grammar.md
│   ├── writing.md
│   └── naturalness.md
│
├── speaking/
│   └── evaluate.md
│
├── conversation/
│   └── tutor.md
│
├── review/
│   └── generate.md
│
└── recommendation/
    └── recommend.md

40. System Prompt

The system prompt should establish:

You are a German language coach.

Your goal is to help the learner improve practical German.

Adapt your language and explanations to the learner's CEFR level.

Be encouraging, concise and accurate.

Correct meaningful mistakes.

Distinguish grammar errors from stylistic alternatives.

Never invent rules.

When uncertain, explicitly indicate uncertainty.

Do not overwhelm beginner learners.

Return structured output when the task requires structured output.

The actual production prompt should be maintained as a versioned file rather than hardcoded inside a component.

41. Prompt Composition

Prompts should be composed from:

System Prompt
+
Task Prompt
+
Learner Context
+
Learning Context
+
Relevant History
+
Current Input

Example:

SYSTEM
+
TASK: Evaluate answer
+
LEVEL: A1
+
WEAK AREA: Articles
+
CURRENT CONCEPT: Feminine nouns
+
ANSWER: ein Pizza

42. Prompt Versioning

Every production prompt must have a version.

Example:

coach-v1
correction-v1
conversation-v1
speaking-v1
exercise-v1

Store the prompt version with AI-generated learning data where useful.

43. Prompt Changes

Never silently change production prompts.

Use:

v1
v1.1
v2

Test a new prompt before replacing the previous version.

44. Structured AI Output

Whenever possible, request structured JSON.

Example:

{
  "correct": false,
  "category": "ARTICLE",
  "correction": "eine",
  "explanation": "Pizza is feminine."
}

Do not rely on parsing natural-language AI responses.

45. JSON Schema Validation

Every structured AI response must be validated.

Flow:

AI response
 ↓
Parse JSON
 ↓
Schema validation
 ↓
Valid?
 ├── YES → Continue
 └── NO  → Retry / fallback

Use Zod or equivalent.

46. Invalid AI Response

If the AI returns invalid data:

Attempt 1
 ↓
Invalid
 ↓
Retry with constrained prompt
 ↓
Invalid
 ↓
Fallback provider
 ↓
Invalid
 ↓
Safe application response

Never save invalid AI output directly into the database.

47. AI Confidence

AI confidence can be stored when useful.

Example:

{
  "confidence": 0.94
}

However, confidence scores must not be treated as objectively calibrated probabilities unless validated.

Use them as internal signals only.

48. AI Hallucination Prevention

The coach should be instructed to:

Prefer provided learner/context data

Avoid inventing grammar rules

Avoid inventing vocabulary meanings

Avoid inventing assessment facts

Avoid claiming unsupported pronunciation details

Ask for clarification when required

Distinguish uncertainty from certainty

49. Grounded Learning Content

For core curriculum explanations, prefer:

Curated content
+
AI explanation

rather than:

AI invents entire curriculum

This ensures that foundational German content remains consistent.

50. AI + Database Boundary

AI should NOT directly modify the database.

Correct:

AI
 ↓
Structured result
 ↓
Backend validation
 ↓
LearningService
 ↓
Database

Incorrect:

AI
 ↓
Database

51. AI + Learning Engine Boundary

The AI suggests.

The learning engine decides.

Example:

AI:
"User appears to struggle with articles."

Learning Engine:
"Increase article review priority."

Database:
"Store updated mastery."

52. AI Memory

Do not rely on the AI model's own memory.

Application memory must live in the database.

Store:

Learner profile
Learning progress
Mistakes
Mastery
Review schedule
Conversation summaries
Goals
Preferences

53. Conversation Memory

For long conversations:

Messages
 ↓
Periodic summary
 ↓
ConversationSummary

Example:

{
  "summary": "Learner practiced ordering food and struggled with masculine and feminine articles."
}

Use the summary in future prompts.

54. Privacy

Send only information required for the current AI task.

Avoid sending:

Passwords

Authentication tokens

Unnecessary personal information

Internal database IDs where unnecessary

Sensitive account information

Use abstract learner context.

55. API Key Security

AI provider API keys must exist only on the backend.

Never:

NEXT_PUBLIC_GEMINI_API_KEY

Never expose provider keys to the browser.

Use server-side environment variables.

56. AI Environment Variables

Example:

AI_PRIMARY_PROVIDER=gemini
AI_FALLBACK_PROVIDER=groq

GEMINI_API_KEY=
GEMINI_MODEL=

GROQ_API_KEY=
GROQ_MODEL=

OPENROUTER_API_KEY=
OPENROUTER_MODEL=

Do not commit actual keys.

57. AI Request Logging

Track:

provider
model
task
duration
success/failure
input token count if available
output token count if available
estimated cost
fallback used
prompt version

Do not store raw sensitive user data in logs.

58. AI Cost Tracking

Even though the target is ₹0, track theoretical usage.

Example:

AI Requests Today
Gemini Requests
Groq Requests
Fallback Count
Tokens Used

This allows us to detect when the free tier is approaching its limits.

59. Free-Tier Quota Management

The application should maintain provider usage metrics.

Example:

Gemini
 ↓
Usage threshold approaching
 ↓
Prefer Groq

Provider limits must be configurable.

Do not hardcode assumptions that may become outdated.

60. Graceful Degradation

If AI is temporarily unavailable:

The application should still work.

Available features:

Static lessons
Vocabulary
Grammar reference
Existing exercises
Review
Progress
XP
Streak

Potentially unavailable:

Live AI conversation
Dynamic correction
AI-generated exercise

The UI should clearly explain temporary AI unavailability.

61. Retry Strategy

Only retry transient errors.

Examples:

429
Temporary network failure
5xx

Avoid retries for:

Invalid API key
Invalid request
Malformed input
User validation errors

Use exponential backoff.

Maximum automatic retries:

2

before switching provider or failing gracefully.

62. Timeout

AI requests must have a timeout.

Example:

AI request timeout:
20–30 seconds

The exact value should be configurable.

Never allow an AI request to hang indefinitely.

63. Streaming

Conversation responses should support streaming if supported by the selected provider.

Example:

User sends message
 ↓
AI starts generating
 ↓
Partial response
 ↓
UI displays progressively

This improves perceived performance.

64. AI Request Types

Initial supported tasks:

EVALUATE_ANSWER
EXPLAIN_GRAMMAR
GENERATE_EXERCISE
EVALUATE_WRITING
EVALUATE_SPEAKING
CONVERSATION_RESPONSE
ANALYZE_MISTAKE
GENERATE_REVIEW
RECOMMEND_ACTIVITY
ESTIMATE_LEVEL

65. Task Routing

Each task maps to one prompt.

Example:

EVALUATE_ANSWER
→ correction/grammar.md

CONVERSATION_RESPONSE
→ conversation/tutor.md

EVALUATE_SPEAKING
→ speaking/evaluate.md

GENERATE_EXERCISE
→ lessons/generate-exercise.md

66. Prompt Input Limits

Every task should define limits.

Example:

Conversation message:
maximum 2,000 characters

Writing submission:
maximum 5,000 characters

Exercise generation:
maximum 10 exercises per request

Limits should be configurable.

67. Content Safety

The AI Coach should remain appropriate for language learning.

If a user attempts to turn the coach into an unrelated or harmful assistant, the coach should maintain its educational purpose.

68. AI Evaluation Dataset

Create:

tests/ai/

with representative examples.

Example:

article-errors.json
grammar-errors.json
conversation.json
writing.json
speaking.json
a1-exercises.json

69. Golden AI Cases

Maintain known examples.

Example:

{
  "input": "Ich möchte ein Pizza.",
  "expectedCategory": "ARTICLE",
  "expectedCorrection": "eine Pizza"
}

The exact model wording does not need to match.

Evaluate semantic correctness.

70. AI Evaluation Metrics

Track:

Correction accuracy
Grammar accuracy
Exercise validity
CEFR appropriateness
Naturalness
Structured-output validity
Conversation quality
Hallucination rate
Fallback rate

71. Model Evaluation

Before selecting a model as primary, test it against the German Coach dataset.

Compare:

Gemini
Groq models
Other free models

Evaluate:

German accuracy

English explanation quality

A1 suitability

JSON reliability

Latency

Free-tier limits

The model with the best overall result should become the default.

72. Model Configuration

Do not hardcode the model name in business logic.

Use:

AI_PRIMARY_PROVIDER
AI_PRIMARY_MODEL
AI_FALLBACK_PROVIDER
AI_FALLBACK_MODEL

This allows model changes without rewriting the application.

73. AI Architecture Example

                    USER
                      │
                      ▼
                 FRONTEND
                      │
                      ▼
                  API ROUTE
                      │
                      ▼
                 AI SERVICE
                      │
            ┌─────────┴─────────┐
            │                   │
            ▼                   ▼
       Context Builder      Prompt Builder
            │                   │
            └─────────┬─────────┘
                      ▼
                 AI PROVIDER
                      │
             ┌────────┴────────┐
             │                 │
             ▼                 ▼
          Gemini              Groq
          Primary           Fallback
             │                 │
             └────────┬────────┘
                      ▼
               JSON Validation
                      │
                      ▼
                Learning Engine
                      │
             ┌────────┼────────┐
             ▼        ▼        ▼
          Mastery   Mistake   Review
                      │
                      ▼
                   Database

74. Recommended MVP AI Scope

Do NOT implement every AI capability immediately.

Phase 1:

1. Grammar correction
2. Exercise evaluation
3. Grammar explanation
4. Basic conversation

Phase 2:

5. Writing evaluation
6. Mistake analysis
7. Adaptive recommendations

Phase 3:

8. Speaking evaluation
9. Advanced conversation
10. Personalized lesson generation

75. MVP Free AI Architecture

The initial implementation should be:

Next.js
    ↓
AIService
    ↓
Gemini Free Tier
    ↓
Groq Free Tier fallback

with:

PostgreSQL
    ↓
Learner Context
    ↓
Prompt Builder
    ↓
AI

76. Important Cost Rule

The product must never silently switch to a paid API.

If free providers are unavailable:

Free provider unavailable
        ↓
Do NOT automatically charge user
        ↓
Show graceful fallback

A paid provider may only be enabled explicitly through configuration.

77. Future Paid Provider

The architecture may later support:

Gemini
Groq
OpenRouter
OpenAI
Anthropic
Other providers

without changing:

LessonService
ConversationService
LearningService
ReviewService

Only the provider adapter should change.

78. Definition of Done

Before AI implementation:

[ ] AI provider abstraction defined
[ ] Gemini selected as primary candidate
[ ] Groq selected as fallback candidate
[ ] Provider configuration defined
[ ] API key security defined
[ ] Learner context defined
[ ] Prompt structure defined
[ ] System prompt defined
[ ] Correction prompt defined
[ ] Exercise prompt defined
[ ] Conversation prompt defined
[ ] Speaking prompt defined
[ ] Writing prompt defined
[ ] Structured output defined
[ ] JSON validation defined
[ ] Retry strategy defined
[ ] Fallback strategy defined
[ ] Rate limiting defined
[ ] Free-tier usage tracking defined
[ ] Privacy rules defined
[ ] AI evaluation dataset defined
[ ] Prompt versioning defined
[ ] Graceful degradation defined
[ ] No automatic paid fallback

79. Success Criteria

The AI Coach MVP is successful when it can:

Understand an A1 learner
        ↓
Evaluate a German answer
        ↓
Identify meaningful mistakes
        ↓
Explain the mistake simply
        ↓
Return structured data
        ↓
Update learner state
        ↓
Recommend the next activity

and:

Start conversation
        ↓
Maintain context
        ↓
Respond at A1 level
        ↓
Correct important mistakes
        ↓
Complete conversation
        ↓
Generate learning feedback

while remaining within the configured free-tier limits.
