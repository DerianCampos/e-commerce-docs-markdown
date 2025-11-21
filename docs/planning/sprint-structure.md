# Sprint Structure - Electronics Store for Programmers

This document defines the sprint workflow, ceremonies, and best practices for the development team.

## Sprint Overview

- **Sprint Duration**: 2 weeks (10 working days)
- **Sprint Capacity**: 40 story points per sprint (team of 4, assuming 10 SP per person per week)
- **Total Sprints**: 9 sprints (Sprint 0 + 3 MVP sprints + 2 Phase 1 + 2 Phase 2 + 2 Phase 3)

---

## Sprint Ceremonies

### 1. Sprint Planning (First Monday, 2 hours)

**Participants**: All team members + Product Owner

**Agenda**:

1. Review sprint goal (Product Owner, 15 min)
2. Review prioritized backlog (Product Owner, 15 min)
3. Team discusses technical approach (30 min)
4. Task breakdown and estimation (45 min)
5. Commit to sprint scope (15 min)

**Output**:

- Sprint goal statement
- Committed user stories and tasks
- Sprint backlog in GitHub Projects/Issues

**Best Practices**:

- Stories should be "ready" (well-defined, estimated, with acceptance criteria)
- Break large stories into tasks
- Identify dependencies and blockers
- Don't over-commit (leave 10% buffer for unknowns)

---

### 2. Daily Standup (Every Weekday, 15 minutes, 9:00 AM)

**Participants**: All team members (Product Owner optional)

**Format** (Each person shares):

1. What I completed yesterday
2. What I'm working on today
3. Any blockers or help needed

**Best Practices**:

- Keep it brief (2-3 minutes per person max)
- Focus on work, not status reporting
- Take detailed discussions offline
- Use async Slack updates if someone is unavailable

**Async Alternative** (if needed):

- Post standup update in Slack by 9:00 AM
- Format: Yesterday/Today/Blockers
- Team Lead reviews and follows up on blockers

---

### 3. Backlog Refinement (Mid-Sprint Wednesday, 1 hour)

**Participants**: All team members + Product Owner

**Agenda**:

1. Review upcoming stories (next 2-3 sprints)
2. Clarify requirements and acceptance criteria
3. Estimate story points
4. Identify technical dependencies
5. Split large stories if needed

**Output**:

- "Ready" stories for next sprint
- Updated story point estimates
- Identified technical spikes or research needed

**Best Practices**:

- Refine 1-2 sprints ahead
- Involve tech team in estimation
- Don't spend time on low-priority backlog items
- Document decisions in story descriptions

---

### 4. Sprint Review (Last Friday, 1 hour)

**Participants**: All team members + Product Owner + Stakeholders

**Agenda**:

1. Demo completed features (30 min)
2. Review sprint metrics (velocity, burndown) (10 min)
3. Stakeholder feedback (15 min)
4. Discuss incomplete items (5 min)

**Output**:

- Demonstrated working software
- Feedback incorporated into backlog
- Incomplete stories moved to next sprint or backlog

**Best Practices**:

- Demo on staging environment (not localhost)
- Focus on user value, not technical implementation
- Get concrete feedback, not just "looks good"
- Celebrate wins and completed features

---

### 5. Sprint Retrospective (Last Friday, 1 hour, after review)

**Participants**: All team members (no Product Owner or stakeholders)

**Agenda**:

1. What went well? (15 min)
2. What didn't go well? (15 min)
3. What should we try differently? (20 min)
4. Action items for next sprint (10 min)

**Output**:

- 2-3 actionable improvements for next sprint
- Assigned owners for action items

**Best Practices**:

- Create safe space for honest feedback
- Focus on process, not people
- Identify root causes, not symptoms
- Make improvements actionable and measurable
- Review previous retrospective action items

---

## Sprint Workflow

### Week 1 (Sprint Days 1-5)

**Monday**:

- Sprint Planning (2 hours)
- Begin development on top-priority stories

**Tuesday-Thursday**:

- Daily standups
- Active development
- Code reviews
- Pull requests

**Friday**:

- Daily standup
- Mid-sprint check-in (informal)
- Code reviews

### Week 2 (Sprint Days 6-10)

**Monday-Wednesday**:

- Daily standups
- Active development
- Code reviews
- Begin testing completed stories

**Wednesday Afternoon**:

- Backlog Refinement (1 hour)

**Thursday**:

- Daily standup
- Wrap up in-progress stories
- Begin sprint review prep (demos)
- Code freeze for sprint stories (focus on testing/bugs)

**Friday**:

- Daily standup
- Sprint Review (1 hour)
- Sprint Retrospective (1 hour)
- Team celebration/social time

---

## Definition of Done

A story is "Done" when:

### Backend

- [ ] Code implements acceptance criteria
- [ ] Unit tests written and passing (85%+ coverage for domain/application)
- [ ] Integration tests written and passing
- [ ] Code reviewed and approved
- [ ] Linting passes (Black, Flake8)
- [ ] API documentation updated (OpenAPI)
- [ ] Merged to main branch

### Frontend

- [ ] Code implements acceptance criteria
- [ ] Component tests written and passing (70%+ coverage)
- [ ] Code reviewed and approved
- [ ] Linting passes (ESLint, Prettier)
- [ ] Responsive design verified (mobile, tablet, desktop)
- [ ] Accessibility checked (keyboard navigation, ARIA labels)
- [ ] Merged to main branch

### UI/UX

- [ ] Design specifications provided to developers
- [ ] Implementation reviewed against designs
- [ ] Usability verified with test users (if applicable)
- [ ] Design files updated and organized in Figma

---

## Story Point Estimation Guide

### 1 SP (Trivial)

- Simple configuration change
- Minor UI tweak
- Small bug fix
- Example: Update button color

### 2 SP (Simple)

- Single API endpoint (CRUD)
- Single UI component (no complex logic)
- Simple unit tests
- Example: Create "Add to wishlist" button

### 3 SP (Moderate)

- Feature with 2-3 sub-tasks
- Moderate backend/frontend integration
- Moderate testing effort
- Example: Implement basic search

### 5 SP (Complex)

- Feature with 4-6 sub-tasks
- Complex business logic
- Integration with third-party service
- Significant testing effort
- Example: Implement Stripe payment

### 8 SP (Very Complex)

- Large feature with 7-10 sub-tasks
- Multiple integrations or dependencies
- Complex state management
- Extensive testing required
- Example: Build admin dashboard

### 13 SP (Epic-Sized, Should Be Split)

- Too large for one sprint
- Should be broken into smaller stories
- Example: Entire product catalog feature

---

## Git Workflow

### Branch Naming

- Feature: `feature/epic-name-story-name` (e.g., `feature/user-management-registration`)
- Bugfix: `bugfix/issue-number-description` (e.g., `bugfix/123-login-error`)
- Hotfix: `hotfix/issue-number-description`

### Commit Messages

Follow Conventional Commits format:

- `feat: Add user registration endpoint`
- `fix: Resolve cart total calculation bug`
- `docs: Update API documentation`
- `test: Add unit tests for Order aggregate`
- `refactor: Extract payment service interface`

### Pull Request Process

1. Create PR with clear title and description
2. Link to GitHub issue/story
3. Request review from appropriate team member
4. Address review feedback
5. Merge after approval and CI passes
6. Delete feature branch after merge

---

## Communication Guidelines

### Slack Channels

- `#general`: Team announcements, general discussion
- `#development`: Technical discussions, questions
- `#standup`: Daily standup updates (async)
- `#pull-requests`: PR notifications and reviews
- `#deployments`: Deployment notifications
- `#monitoring`: Alerts and monitoring notifications

### Response Time Expectations

- Urgent (production down): < 15 minutes
- Blocker (can't proceed): < 1 hour
- Question/clarification: < 4 hours (same day)
- Code review: < 24 hours (next business day)

---

## Velocity Tracking

### Sprint Velocity

- Track completed story points per sprint
- Calculate 3-sprint rolling average
- Use for future sprint planning

### Burndown Chart

- Track remaining story points daily
- Identify scope creep or underestimation
- Adjust mid-sprint if needed

### Metrics to Monitor

- Velocity (story points completed per sprint)
- Sprint commitment accuracy (planned vs completed)
- Defect rate (bugs found per story)
- Code review time (hours from PR to merge)

---

**Document Owner**: Tech Lead + Product Owner  
**Last Updated**: November 12, 2025  
**Version**: 1.0.0  
**Status**: ✅ Complete
