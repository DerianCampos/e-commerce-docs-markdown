# GitHub Issue Templates

This document provides templates for creating consistent GitHub issues for epics, user stories, bugs, and technical tasks.

---

## Epic Issue Template

```markdown
## Epic: [Epic Name]

### Description

[Brief description of the epic and its business value]

### User Value

[Explain the value this epic provides to users]

### Scope

- [ ] Story 1
- [ ] Story 2
- [ ] Story 3

### Success Criteria

- [ ] Criterion 1
- [ ] Criterion 2

### Dependencies

- [List any dependent epics or external dependencies]

### Story Points

Total: [X SP]

### Phase

[MVP / Phase 1 / Phase 2 / Phase 3]

### Labels

`epic`, `[phase]`, `[team]`
```

---

## User Story Issue Template

```markdown
## User Story: [Story Name]

### Story

As a **[user role]**,  
I want to **[action/goal]**,  
So that **[benefit/outcome]**.

### Acceptance Criteria

- [ ] Given [context], When [action], Then [expected result]
- [ ] Given [context], When [action], Then [expected result]
- [ ] [Additional criteria]

### Technical Notes

- [Implementation guidance]
- [Design patterns to use]
- [Security considerations]
- [Performance requirements]

### Tasks

- [ ] Task 1 ([Role], [X SP])
- [ ] Task 2 ([Role], [X SP])
- [ ] Task 3 ([Role], [X SP])

### Dependencies

- #[Issue Number] must be completed first
- [External dependency]

### Story Points

[X SP]

### Phase

[MVP / Phase 1 / Phase 2 / Phase 3]

### Epic

Part of #[Epic Issue Number]

### Labels

`user-story`, `[phase]`, `[team]`, `[priority]`
```

---

## Bug Report Template

```markdown
## Bug: [Brief Description]

### Description

[Clear description of the bug]

### Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Step 3]

### Expected Behavior

[What should happen]

### Actual Behavior

[What actually happens]

### Screenshots/Logs

[If applicable, add screenshots or error logs]

### Environment

- Browser/Device: [e.g., Chrome 90, iPhone 12]
- OS: [e.g., Windows 10, iOS 14]
- Version/Branch: [e.g., v1.2.0, feature/checkout]

### Severity

- [ ] Critical (blocks core functionality)
- [ ] High (major feature broken)
- [ ] Medium (feature partially broken)
- [ ] Low (minor issue, workaround exists)

### Story Points

[X SP] (if needs estimation)

### Labels

`bug`, `[severity]`, `[team]`
```

---

## Technical Task Template

```markdown
## Technical Task: [Task Name]

### Description

[Clear description of the technical work needed]

### Context

[Why this task is needed - technical debt, performance improvement, refactoring, etc.]

### Scope

- [ ] Sub-task 1
- [ ] Sub-task 2
- [ ] Sub-task 3

### Success Criteria

- [ ] [Measurable outcome 1]
- [ ] [Measurable outcome 2]

### Technical Details

[Implementation approach, architectural considerations]

### Testing Requirements

[How to verify this task is complete]

### Story Points

[X SP]

### Labels

`tech-task`, `[team]`, `[type]` (refactor/performance/infrastructure/debt)
```

---

## Example Issues

### Example Epic Issue

```markdown
## Epic: User Management

### Description

Complete user authentication, authorization, and profile management system to allow users to create accounts, log in, and manage their profiles.

### User Value

Users need secure accounts to save shopping preferences, track orders, and manage their purchase history.

### Scope

- [ ] Story 1.1: User Registration (#10)
- [ ] Story 1.2: User Login (#11)
- [ ] Story 1.3: User Profile Management (#12)
- [ ] Story 1.4: Password Reset (#13)

### Success Criteria

- [ ] Users can register with email and password
- [ ] Users can log in and maintain session for 24 hours
- [ ] Users can update profile and manage addresses
- [ ] Users can reset forgotten passwords via email

### Dependencies

- SendGrid email service configured
- JWT authentication middleware implemented

### Story Points

Total: 60 SP (18 MVP + 10 Phase 1 + 32 Phase 3)

### Phase

MVP (Core features) + Phase 1 & 3 (Enhancements)

### Labels

`epic`, `mvp`, `backend`, `frontend`
```

---

### Example User Story Issue

```markdown
## User Story: User Registration

### Story

As a **new user**,  
I want to **create an account with email and password**,  
So that **I can save my shopping preferences and track my orders**.

### Acceptance Criteria

- [ ] Given I am on the registration page, When I enter valid email, password (min 8 chars, 1 uppercase, 1 number), first name, last name, Then my account is created and I receive a verification email
- [ ] Given I register with valid details, When I click the verification link in email, Then my email is verified and I can log in
- [ ] Given I try to register with an existing email, When I submit the form, Then I see an error "Email already registered"
- [ ] Given I enter an invalid password, When I submit the form, Then I see clear validation errors

### Technical Notes

- Use Pydantic for input validation on backend
- Store password hash (bcrypt, cost factor 12), never plain text
- JWT tokens with 24-hour expiration for sessions
- Email verification token expires in 24 hours
- Implement rate limiting (5 registration attempts per hour per IP)

### Tasks

- [ ] Create User entity and Email/Password value objects (Backend Engineer, 2 SP)
- [ ] Implement UserRepository interface and PostgreSQL implementation (Backend Engineer, 2 SP)
- [ ] Create RegisterUser use case in application layer (Backend Engineer, 2 SP)
- [ ] Build registration API endpoint with validation (Backend Engineer, 2 SP)
- [ ] Set up email service integration (SendGrid) (Backend Engineer, 2 SP)
- [ ] Design and implement registration form UI (Frontend Engineer, 2 SP)
- [ ] Integrate registration form with API (Frontend Engineer, 2 SP)
- [ ] Write unit and integration tests (Backend + Frontend, 2 SP)
- [ ] Email verification page and flow (Frontend Engineer, 2 SP)

### Dependencies

- #5 (Infrastructure setup) must be completed first
- #6 (Email service configuration) must be completed

### Story Points

18 SP

### Phase

MVP

### Epic

Part of #9 (Epic: User Management)

### Labels

`user-story`, `mvp`, `backend`, `frontend`, `high-priority`
```

---

### Example Bug Report

```markdown
## Bug: Cart Total Calculates Incorrectly with Discount Code

### Description

When applying a percentage discount code at checkout, the cart total shows an incorrect amount.

### Steps to Reproduce

1. Add 2 products to cart (total $100)
2. Proceed to checkout
3. Enter discount code "SAVE20" (20% off)
4. Observe cart total

### Expected Behavior

Cart total should be $80 ($100 - 20% = $80)

### Actual Behavior

Cart total shows $120 (incorrect)

### Screenshots/Logs

[Attached screenshot of checkout page]

Error log:
```

TypeError: Cannot read property 'percentage' of undefined
at calculateDiscount (checkout.service.ts:45)

```

### Environment
- Browser/Device: Chrome 110, Desktop
- OS: macOS Ventura
- Version/Branch: v1.0.0 / main

### Severity
- [x] High (major feature broken)

### Story Points
3 SP

### Labels
`bug`, `high-severity`, `backend`, `checkout`
```

---

## Label Conventions

### Type Labels

- `epic` - High-level feature grouping
- `user-story` - Individual user story
- `bug` - Bug report
- `tech-task` - Technical task (refactor, infrastructure, etc.)

### Phase Labels

- `mvp` - MVP features
- `phase-1` - Phase 1 enhancements
- `phase-2` - Phase 2 features
- `phase-3` - Phase 3 advanced features

### Team Labels

- `backend` - Backend engineer responsibility
- `frontend` - Frontend engineer responsibility
- `ui-ux` - UI/UX engineer responsibility
- `tech-lead` - Tech lead responsibility

### Priority Labels

- `critical` - Must be fixed immediately
- `high-priority` - Important, address soon
- `medium-priority` - Normal priority
- `low-priority` - Nice to have

### Severity Labels (Bugs)

- `critical-severity` - Blocks core functionality
- `high-severity` - Major feature broken
- `medium-severity` - Feature partially broken
- `low-severity` - Minor issue

---

**Document Owner**: Tech Lead + Product Owner  
**Last Updated**: November 12, 2025  
**Version**: 1.0.0  
**Status**: ✅ Complete
