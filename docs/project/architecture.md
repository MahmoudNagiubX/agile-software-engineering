# Architecture Decision — Web-First, Mobile-Companion

**Status:** Accepted for planning (Sprint 0)  
**Product:** Graduation Project Tracker  
**Decision:** One product, two purpose-built frontends, one backend and database.

## Platforms and technology

| Component | Technology | Responsibility |
|---|---|---|
| Main web application | React + TypeScript | Full student, supervisor and coordinator workspaces; complex dashboards, planning and approvals |
| Companion Android application | Flutter / Dart | Focused student workflow: sign in, profile, quick project status, tasks, deadlines, reminders, camera-assisted university ID entry and focus sessions |
| Shared API | FastAPI / Python | Authentication, team membership, permissions, milestones, submissions, tasks, review workflows, audit/activity events |
| Shared database | PostgreSQL | Single source of truth for all clients |
| Synchronization | REST API; WebSockets if justified | Mutations persist centrally; refresh/re-fetch shows them on both clients. Real-time push is an enhancement. |

**Primary implementation target:** Web application. The Flutter companion remains intentionally smaller; it does not need to duplicate the full coordinator/supervisor experience.

## Key user journeys

1. **Student team:** Form a team, associate with its supervisor, create and assign tasks, manage files and milestones, and submit work for review.
2. **Supervisor:** See assigned teams, inspect deliverables and team progress, comment, return or approve submissions.
3. **Programme coordinator:** Review cross-team progress, risk and analytics.
4. **Mobile companion:** Sign in, see deadlines and activity, add or update a task and observe the change on the web after a data refresh.

## Non-negotiable business rules

- A submission made after the milestone deadline must be flagged **late**.
- **Only the assigned supervisor** can approve the milestone.
- Access control must be enforced server-side, not only by hiding frontend controls.
- Only synthetic/test student data is used in course demos, repositories and submissions.

## Confirmed feature roadmap

**Priority 1 — Foundation:** All four mandatory MVP epics (team-to-supervisor linking; milestone deadlines; file/link submission; supervisor feedback and approve/return), accounts and role-based access, team collaboration, task assignment and tracking, comments, task attachments, profile editing and photos.

**Priority 2 — Enhanced experience:** Interactive project timeline, analytics dashboards, smart deadline and task notifications, activity history and collaboration improvements.

**Priority 3 — Advanced functionality:** Camera/OCR-assisted university ID signup, Pomodoro sessions, hardware purchases/inventory, AI assistance and other agreed stretch enhancements.

**Login and profile options:** University ID OCR-assisted data entry, manual confirmation/completion, email/password and email OTP; Google sign-in as an additional option. OCR does not authenticate or establish eligibility on its own. Real student ID photos must not enter project datasets, logs, commits or submissions.

**Product-wide lifecycle:** Team formation → proposal → requirements and research → design and implementation → testing and milestone submissions → feedback/rework → final documentation and defense.

## Development and documentation

- Maintain a **single GitHub repository** and a **single Jira project**.
- Use traceable Jira stories, short-lived branches, reviewed pull requests, tests and per-sprint documentation.
- Required course paths include `README.md`, `src/`, `tests/`, `features/`, `.github/workflows/ci.yml` and `docs/project/`. A future scaffold may organize code in `src/web/`, `src/mobile/` and `src/backend/`.
- Preserve genuine individual contributions. Do not fabricate commits, reviews, votes or user feedback.
- Meet the course's 15+ INVEST story requirement; acceptance criteria, TDD for business rules, 5+ passing Gherkin scenarios, a working CI pipeline, deployable or Docker-runnable software, feedback from 3+ users, and Scrum process evidence.
- Treat `Smart-Life-Planner` as a **reference/source for selectively adapted existing code**, especially FastAPI auth/tasks, reminders, timelines and Flutter components, not as a wholesale drop-in backend.
- Any coding assistance (Codex and Antigravity CLI) must be reviewed, tested and documented by the team.

## Deployment and demo

- Host the web app and API or provide Docker-based startup following the brief.
- Produce a testable Android APK for the Flutter companion; demonstrate on a Samsung A54 if available.
- A task edited on Flutter must be visible on web re-fetch; no page reload or realtime connection is required for basic correctness.
- If live synchronization is later implemented, use WebSockets or a comparable server-side event mechanism with robust reconnection and state refresh.

## Decisions deferred to later steps

Hosting provider, Flutter navigation scope, UX design system, specific OCR library and measured accuracy, WebSocket implementation, file-storage provider, GitHub branch rules, and exact sprint story allocations.

This file records the accepted architecture, not a claim that either client or the synchronization is already implemented.
