German Language Coach

Product Requirements Document — Step 10: Authentication & User Management

Version: 0.1
Status: Draft

1. Purpose

The Authentication & User Management system provides secure account creation, login, sessions, onboarding, profiles, preferences, and account lifecycle management.

The MVP should make authentication:

Simple

Secure

Reliable

Mobile-friendly

Easy to maintain

Ready for future social login

Authentication must be separated from learning, gamification, and AI logic.

2. Core Principle

Authentication answers:

Who is this user?

User management answers:

What does this user want and what is their current application state?

Learning systems answer:

What should this user learn?

Architecture:

Authentication
      ↓
User Identity
      ↓
User Profile & Preferences
      ↓
Learning Profile
      ↓
Learning Engine
      ↓
Gamification

3. MVP Authentication Scope

The MVP should support:

[ ] Register
[ ] Login
[ ] Logout
[ ] Session management
[ ] Password hashing
[ ] Password reset
[ ] Email verification
[ ] User profile
[ ] Learning preferences
[ ] Onboarding
[ ] Account deletion
[ ] Basic roles

Future:

[ ] Google login
[ ] Apple login
[ ] GitHub login
[ ] Passkeys
[ ] Multi-factor authentication

4. Registration

The learner should be able to create an account using:

Email
Password
Display name

Do not collect unnecessary personal information.

5. Password Security

Passwords must never be stored in plain text.

Use a modern password hashing algorithm such as:

Argon2id
bcrypt

Minimum MVP password length:

8 characters

The backend should reject obviously weak/common passwords where practical.

Never log passwords.

6. Registration Flow

User enters details
        ↓
Validate input
        ↓
Check existing account
        ↓
Hash password
        ↓
Create user
        ↓
Create profile
        ↓
Create default preferences
        ↓
Send verification email
        ↓
Onboarding

7. Duplicate Email

Email addresses should be unique.

Use a safe generic response to reduce account enumeration.

Example:

Unable to create the account.
Please check your email or try signing in.

8. Email Normalization

Email handling should be consistent:

Trim whitespace
Validate format
Normalize casing where appropriate

Avoid provider-specific transformations that could alter valid addresses.

9. Email Verification

Flow:

Account created
       ↓
Verification email
       ↓
User clicks verification link
       ↓
Email verified
       ↓
Account becomes fully activated

Verification tokens must:

Expire
Be single-use
Be cryptographically random
Never contain sensitive information

10. Login

Login requires:

Email
Password

Flow:

Login request
      ↓
Validate input
      ↓
Find account
      ↓
Verify password
      ↓
Check account status
      ↓
Create authenticated session
      ↓
Return authenticated state

Use:

Invalid email or password.

for failed authentication rather than revealing which credential was incorrect.

11. Brute-Force Protection

Rate-limit:

Login
Registration
Password reset
Email verification

Example:

Too many attempts
        ↓
Temporary rate limit
        ↓
Try again later

Exact limits should be configurable.

12. Session Management

For a web application, prefer secure HTTP-only cookies for sessions where architecture permits.

Cookie settings should include appropriate:

HttpOnly
Secure
SameSite

Session lifetime should use configurable:

Idle timeout
Maximum session lifetime

13. Logout

Logout must:

Invalidate the session
Clear authentication state
Return user to login/home

14. Password Reset

Flow:

Forgot password
      ↓
Enter email
      ↓
Send reset email
      ↓
Open reset link
      ↓
Set new password
      ↓
Invalidate appropriate existing sessions
      ↓
Login again

Reset tokens must be:

Cryptographically random
Expiring
Single-use
Safely stored
Never logged

Use a generic response:

If an account exists, a password reset email has been sent.

15. Change Password

Authenticated users should be able to change their password.

Current password
New password
Confirm new password

Re-authentication may be required for sensitive account changes.

16. Account Deletion

The user must be able to request account deletion.

Flow:

Settings
   ↓
Delete Account
   ↓
Warning
   ↓
Confirmation
   ↓
Re-authenticate if required
   ↓
Delete/anonymize data

Deletion should clearly state that the action may be irreversible.

17. Data Deletion

Account deletion should address:

User account
Profile
Preferences
Learning history
Gamification data
AI conversation history
Generated content references
Sessions
Notifications

Any legally required retention should be handled separately and, where possible, anonymized.

18. User Roles

MVP:

USER
ADMIN

Future:

CONTENT_EDITOR
MODERATOR
SUPPORT

Authorization must always be enforced server-side.

19. Role Permissions

Capability

USER

ADMIN

Learn

Yes

Yes

View own progress

Yes

Yes

Edit own profile

Yes

Yes

Manage curriculum

No

Yes

Manage exercises

No

Yes

View content reports

No

Yes

Manage users

No

Yes

20. User Profile

Recommended fields:

id
email
displayName
avatar
role
status
emailVerified
createdAt
updatedAt
lastLoginAt

Do not collect unnecessary personal information.

21. Account Status

Recommended:

PENDING_VERIFICATION
ACTIVE
SUSPENDED
DELETED

Suspended/deleted users must not access normal authenticated functionality.

22. Learning Profile

Keep learning information separate from authentication.

Recommended:

userId
nativeLanguage
targetLanguage
currentLevel
targetLevel
learningGoal
dailyGoalMinutes
preferredLearningStyle
timezone

Example:

{
  "userId": "user_123",
  "nativeLanguage": "en",
  "targetLanguage": "de",
  "currentLevel": "A1",
  "targetLevel": "B1",
  "learningGoal": "WORK_IN_GERMANY",
  "dailyGoalMinutes": 20,
  "timezone": "Asia/Kolkata"
}

23. Onboarding

Keep onboarding short.

Recommended:

1. Welcome
2. Why are you learning German?
3. Current German level
4. Target level
5. Daily available time
6. Preferred skills
7. Start first lesson

24. Learning Goal

Possible goals:

TRAVEL
WORK
STUDY
PERSONAL
EXAM
GENERAL_COMMUNICATION

Future:

JOB_INTERVIEW
RELOCATION
BUSINESS

Goals should influence recommendations without locking the learner into a rigid path.

25. Current Level

Allow:

Beginner / A0
A1
A2
B1
B2
C1
C2
Not sure

If the learner selects "Not sure", recommend a placement assessment.

26. Placement Assessment

Future flow:

Onboarding
    ↓
Optional placement test
    ↓
Estimate level
    ↓
Learning Engine initializes starting point

Do not require placement testing for MVP if it delays launch.

27. Daily Goal

During onboarding:

5 min
10 min
20 min
30 min
45 min
60 min

The default should be achievable rather than overly ambitious.

28. Timezone

Store the user's IANA timezone.

Examples:

Asia/Kolkata
Europe/Berlin
America/New_York

Timezone is important for:

Daily goals
Streaks
Daily reset
Notifications
Progress reports

29. User Preferences

Recommended:

language
timezone
dailyGoalMinutes
notificationsEnabled
soundEnabled
preferredDifficulty
explanationLanguage

Only store preferences that have a current or planned product use.

30. Notification Preferences

Future settings:

Daily reminder
Streak reminder
Goal completion
Weekly summary
Achievement

Each should be individually configurable.

31. Data Separation

Separate the following areas:

Identity
Profile
Learning
Gamification
AI Conversations

Conceptually:

User
 ├── UserProfile
 ├── LearningProfile
 ├── UserPreferences
 ├── LearningProgress
 ├── GamificationProfile
 └── AIConversation

32. Database Entities

Recommended:

User
UserProfile
LearningProfile
UserPreferences
Session
EmailVerificationToken
PasswordResetToken

Future:

LoginAttempt
UserDevice
AuditLog

33. User Table

Example:

id
email
passwordHash
role
status
emailVerified
createdAt
updatedAt
lastLoginAt

Never return passwordHash from APIs.

34. UserProfile Table

id
userId
displayName
avatarUrl
createdAt
updatedAt

35. LearningProfile Table

id
userId
nativeLanguage
targetLanguage
currentLevel
targetLevel
learningGoal
dailyGoalMinutes
timezone
createdAt
updatedAt

36. UserPreferences Table

id
userId
explanationLanguage
preferredDifficulty
notificationsEnabled
soundEnabled
createdAt
updatedAt

37. Session Table

If server-side sessions are used:

id
userId
sessionTokenHash
expiresAt
createdAt
lastUsedAt
deviceInfo

Store a hash/reference rather than unnecessary raw secrets.

38. Verification Token

id
userId
tokenHash
expiresAt
usedAt
createdAt

Never store raw verification tokens if avoidable.

39. Password Reset Token

id
userId
tokenHash
expiresAt
usedAt
createdAt

Tokens must be single-use.

40. Authentication APIs

POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
GET  /api/auth/me
POST /api/auth/verify-email
POST /api/auth/resend-verification
POST /api/auth/forgot-password
POST /api/auth/reset-password
POST /api/auth/change-password

41. User APIs

GET /api/users/me
PUT /api/users/me
DELETE /api/users/me

GET /api/users/me/profile
PUT /api/users/me/profile

GET /api/users/me/preferences
PUT /api/users/me/preferences

GET /api/users/me/learning-profile
PUT /api/users/me/learning-profile

42. Current User Endpoint

GET /api/auth/me

Example:

{
  "id": "user_123",
  "email": "learner@example.com",
  "displayName": "Learner",
  "role": "USER",
  "emailVerified": true
}

Never return:

passwordHash
resetToken
verificationToken
sessionToken

43. Authentication Middleware

Protected APIs:

Request
  ↓
Authentication Middleware
  ↓
Validate session
  ↓
Identify user
  ↓
Authorization
  ↓
Controller

44. Authorization

Authentication:

Who are you?

Authorization:

What are you allowed to do?

Never rely only on frontend route protection.

45. API Security

Authentication APIs must include appropriate:

HTTPS
Rate limiting
Input validation
Secure cookies/tokens
CSRF protection where applicable
Safe error messages
Security headers
Audit logging for sensitive actions

46. CSRF

If cookie-based authentication is used, protect state-changing requests against CSRF.

Possible mechanisms:

SameSite cookies
CSRF tokens
Origin checks

Use the approach appropriate to the architecture.

47. CORS

If frontend and backend are hosted separately:

Allow only trusted origins.

Do not use unrestricted wildcard origins for authenticated credentialed requests.

48. Input Validation

Validate server-side:

Email
Password
Display name
Timezone
Language codes
CEFR levels
Learning goals
Preferences

Never trust client-side validation alone.

49. Logging

Never log:

Passwords
Authentication tokens
Password reset tokens
Verification tokens
Session secrets

Safe audit events may include:

Login success
Login failure
Logout
Password reset requested
Password changed
Account deleted

Do not log sensitive values.

50. Audit Events

Future:

USER_REGISTERED
EMAIL_VERIFIED
LOGIN_SUCCESS
LOGIN_FAILED
PASSWORD_CHANGED
PASSWORD_RESET
ACCOUNT_DELETED
ROLE_CHANGED
ACCOUNT_SUSPENDED

Audit records must not contain credentials or secrets.

51. Account Recovery

If the user loses access to the registered email:

MVP:
Direct the user to their email provider/account recovery.

Do not build manual identity verification for MVP unless required.

52. Social Login

Social login should be an extension rather than a core dependency.

Future:

User
  │
  ├── Password Credential
  ├── Google Identity
  ├── Apple Identity
  └── GitHub Identity

53. Authentication Service

Use an abstraction such as:

AuthService

Responsibilities:

register()
login()
logout()
verifyEmail()
requestPasswordReset()
resetPassword()
changePassword()

Do not spread authentication logic across controllers.

54. User Service

Use a separate:

UserService

Responsibilities:

Get user
Update profile
Update preferences
Update learning profile
Delete account

55. Service Separation

Recommended:

AuthService
    ↓
Identity/session

UserService
    ↓
Profile/preferences

LearningService
    ↓
Learning state

GamificationService
    ↓
XP/streaks

CoachService
    ↓
AI conversations

Do not put all application logic into UserService.

56. Frontend Authentication State

The frontend should represent:

authenticated
unauthenticated
loading

Flow:

App starts
   ↓
Check session
   ↓
Authenticated?
 ┌───────┴───────┐
Yes             No
 ↓               ↓
Dashboard       Login

57. Protected Routes

Examples:

/dashboard
/learn
/review
/progress
/settings

Public:

/
/login
/register
/forgot-password
/reset-password

58. Onboarding State

Store onboarding progress:

NOT_STARTED
IN_PROGRESS
COMPLETED

The learner should be able to resume onboarding after a temporary network failure.

59. Onboarding Completion

On completion:

User profile created
+
Learning preferences saved
+
Initial learning state created
+
First lesson recommended

60. First Login Experience

Example:

Welcome!
        ↓
Your goal: Learn German for work
        ↓
Daily goal: 20 minutes
        ↓
Current level: A1
        ↓
Start today's lesson

The learner should reach useful learning quickly.

61. Security Headers

Production should consider:

Content-Security-Policy
Strict-Transport-Security
X-Content-Type-Options
Referrer-Policy
Frame protection

Use secure framework/server defaults where appropriate and test before release.

62. Privacy

Collect the minimum information required.

Clearly communicate:

What data is stored
Why it is stored
How it is used
How the user can delete their account

AI conversation data must also follow the selected AI provider's terms/data policy and the application's privacy policy.

63. GDPR Readiness

Because the application may be used by people in Germany/EU, design for privacy requirements from the beginning.

Consider:

Data minimization
Consent where required
Access/export
Deletion
Privacy policy
Cookie requirements
Data retention
Processor agreements

Legal compliance should be reviewed before production launch.

64. Account Export

Future:

Export My Data

Potential export:

Profile
Learning progress
Achievements
Learning history
Conversation history
Preferences

Not required for the initial MVP UI.

65. Testing — Registration

Test:

Valid registration
Invalid email
Weak password
Duplicate email
Whitespace handling
Email verification
Database failure
Rate limiting

66. Testing — Login

Test:

Valid login
Wrong password
Unknown email
Unverified account
Suspended account
Expired session
Rate limiting
Concurrent sessions

67. Testing — Password Reset

Test:

Reset request
Unknown email
Expired token
Used token
Valid token
New password validation
Session invalidation

68. Testing — Authorization

Verify:

USER cannot access admin APIs
USER cannot edit another user's profile
USER cannot read another user's learning data
ADMIN can access admin functions
Unauthenticated users cannot access protected APIs

69. Testing — Account Deletion

Verify:

User confirms deletion
Account becomes inaccessible
Required data is deleted/anonymized
Sessions are invalidated
Learning data is handled correctly
Gamification data is handled correctly
AI data is handled correctly

70. Security Testing

Before production test for:

Authentication bypass
Session fixation
CSRF
XSS
SQL injection
Rate-limit bypass
Account enumeration
Token replay
Privilege escalation
Insecure direct object references

Use automated security scanning where practical.

71. MVP User Journey

Landing Page
     ↓
Register
     ↓
Verify Email
     ↓
Onboarding
     ↓
Choose Goal
     ↓
Choose Level
     ↓
Choose Daily Goal
     ↓
First Lesson
     ↓
Learning Engine
     ↓
Progress
     ↓
Gamification
     ↓
Return Daily

72. MVP Scope

Must have:

[ ] Registration
[ ] Login
[ ] Logout
[ ] Secure password hashing
[ ] Session management
[ ] Email verification
[ ] Password reset
[ ] User profile
[ ] Learning profile
[ ] User preferences
[ ] Onboarding
[ ] USER/ADMIN roles
[ ] Protected APIs
[ ] Rate limiting
[ ] Input validation
[ ] Account deletion
[ ] Security tests

Not required for MVP:

[ ] Google login
[ ] Apple login
[ ] GitHub login
[ ] Passkeys
[ ] MFA
[ ] Device management UI
[ ] Data export UI

73. Definition of Done

Authentication & User Management is complete when:

[ ] A user can register
[ ] Passwords are securely hashed
[ ] A user can log in
[ ] A user can log out
[ ] Sessions are secure
[ ] Email verification works
[ ] Password reset works
[ ] Protected APIs require authentication
[ ] Authorization is enforced server-side
[ ] User profile can be updated
[ ] Learning preferences can be updated
[ ] Onboarding can be completed
[ ] User timezone is stored
[ ] Account deletion works
[ ] Authentication endpoints are rate limited
[ ] Sensitive values are not logged
[ ] Core security tests pass

74. Final Architecture

                         USER
                           │
                           ▼
                       FRONTEND
                           │
                           ▼
                    AUTHENTICATION
                           │
              ┌────────────┴────────────┐
              ▼                         ▼
          AuthService              UserService
              │                         │
              ▼                         ▼
          Sessions              Profile/Preferences
              │                         │
              └────────────┬────────────┘
                           ▼
                    LEARNING PROFILE
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
       LEARNING ENGINE  GAMIFICATION  AI COACH
              │            │            │
              └────────────┼────────────┘
                           ▼
                        DATABASE

75. Key Architectural Rule

Authentication identifies the learner; User Management stores the learner's preferences; the Learning Engine owns learning state.

Do not mix authentication logic with:

XP
Streaks
Learning mastery
AI prompts
Curriculum progression

These belong to their respective services.

76. Product Outcome

The learner should experience:

Create account
      ↓
Tell us your goal
      ↓
Tell us your level
      ↓
Choose your daily commitment
      ↓
Start learning
      ↓
Return securely every day

The ultimate goal is:

Make account creation and user management almost invisible so the learner can focus on learning German
