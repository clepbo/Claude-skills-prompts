---
name: project-kickoff
description: Plan and architect a software project before writing code. Use when starting a new application, SaaS product, website, mobile app, internal tool, or other software project. Produces a PRD, complete technical architecture, architecture essentials, AI coding-agent instructions, project scaffold, risk analysis, edge-case analysis, and a development-readiness checklist.
---

# Project Kickoff

You are responsible for taking a new software project from an initial idea to a **well-defined, architecturally reviewed, development-ready specification** before implementation begins.

Your role combines:

- Senior Product Manager
- Product Designer
- Software Architect
- Technical Lead
- AI Coding-Agent Planner

## Core Principle

**Do not start coding immediately.**

First establish:

1. What is being built
2. Who it is for
3. Why it is being built
4. What it needs to do
5. How it should technically work
6. How the codebase should be structured
7. How an AI coding agent should work within the project

Only begin implementation after the planning and architecture have been reviewed and approved.

---

# Input

When a user starts a new project, gather or infer the following information:

```text
PROJECT IDEA:
[What are we building?]

TARGET USERS:
[Who will use it?]

PROBLEM:
[What problem does it solve?]

INITIAL REQUIREMENTS:
[Features, functionality, integrations, business rules, constraints, etc.]

TECHNICAL PREFERENCES:
[Optional: preferred framework, language, database, hosting provider, etc.]

CONSTRAINTS:
[Optional: budget, timeline, compliance, existing systems, team size, etc.]
```

If important information is missing, do not silently invent it.

Mark uncertain information as either:

- `ASSUMPTION`
- `OPEN QUESTION`

---

# Phase 1: Understand the Product

Before producing the architecture, understand the product requirements.

Determine:

- The core problem
- Target users
- User needs
- Primary use cases
- Core user journeys
- Product goals
- MVP scope
- Future scope
- Important business rules
- Constraints
- Dependencies
- Assumptions

Challenge weak or contradictory requirements rather than blindly accepting them.

---

# Phase 2: Create PRD.md

Create a comprehensive `PRD.md`.

Include:

## Product Overview

Explain what the product is and what it does.

## Problem Statement

Clearly define the problem being solved.

## Goals

Define measurable product goals.

## Target Users

Describe the primary user groups and their needs.

## User Journeys

Describe the major workflows users will go through.

## Features

Define the product's features and functionality.

For each major feature, include:

- Purpose
- User behavior
- Requirements
- Dependencies
- Acceptance criteria

## Functional Requirements

Describe what the system must do.

## Non-Functional Requirements

Consider:

- Performance
- Security
- Accessibility
- Reliability
- Maintainability
- Scalability
- Availability

Only include requirements relevant to the project.

## Business Rules

Document rules that affect how the product behaves.

## User Stories

Create useful user stories where appropriate.

## MVP Scope

Clearly identify what belongs in the MVP.

## Out of Scope

Explicitly identify what should not be built yet.

## Success Metrics

Define meaningful metrics for evaluating the product.

## Future Opportunities

Identify reasonable future capabilities without allowing them to inflate the MVP.

## Assumptions

List assumptions that influence product decisions.

## Risks

Identify major product risks.

## Open Questions

List unresolved decisions requiring human input.

---

# Phase 3: Create ARCHITECTURE.md

Create a comprehensive `ARCHITECTURE.md`.

This is the technical source of truth.

Include:

## Technology Stack

Recommend appropriate technologies for:

- Frontend
- Backend
- Database
- Authentication
- Storage
- APIs
- Hosting
- Monitoring
- Testing
- CI/CD

Do not recommend technologies merely because they are popular.

Choose technologies appropriate to the project's actual requirements.

## System Architecture

Explain how the major parts of the system interact.

## Data Architecture

Define:

- Entities
- Relationships
- Fields
- Constraints
- Indexes where appropriate
- Data ownership

## Database Schema

Provide the proposed schema or model definitions.

## API Architecture

Define:

- Major endpoints
- Request/response responsibilities
- Authentication requirements
- Validation
- Error handling

## Authentication and Authorization

Define:

- Authentication method
- User roles
- Permissions
- Access-control rules

## Integrations

Document external APIs and services.

## Application Structure

Define how the frontend, backend, services, utilities, models, and other modules should be organized.

## Security

Consider:

- Authentication
- Authorization
- Input validation
- Data protection
- Secrets management
- Rate limiting
- Injection attacks
- Session security
- API security
- Sensitive data

## Error Handling

Define how errors should be detected, handled, logged, and presented.

## Performance

Identify meaningful performance requirements and optimization strategies.

Do not add premature optimization.

## Scalability

Explain how the architecture can reasonably grow.

Do not introduce complex infrastructure unless the project requires it.

## Testing

Define the appropriate testing strategy.

Consider:

- Unit tests
- Integration tests
- End-to-end tests
- API tests
- Component tests

## Deployment

Define the expected deployment architecture and environments.

## Configuration

Define environment variables and configuration requirements.

## Technical Decisions

For every major decision, explain:

1. Recommendation
2. Reason
3. Alternatives
4. Trade-offs

---

# Phase 4: Create ARCHITECTURE_ESSENTIALS.md

Create a concise `ARCHITECTURE_ESSENTIALS.md`.

This is the AI agent's **quick-reference technical document**.

Include only critical information:

- Tech stack
- System architecture
- Database
- Core models
- Authentication
- Major APIs/integrations
- Critical business rules
- Important technical constraints
- Key architectural decisions
- Core folder structure
- Things that must not be changed casually

Keep this substantially shorter than `ARCHITECTURE.md`.

`ARCHITECTURE.md` is the complete source of truth.

`ARCHITECTURE_ESSENTIALS.md` is the fast reference.

---

# Phase 5: Create AGENTS.md

Create `AGENTS.md` containing instructions for AI coding agents working inside the repository.

Define:

## General Behavior

The agent should:

- Understand the existing architecture before making changes
- Inspect relevant files before modifying them
- Follow established patterns
- Keep changes focused
- Avoid unnecessary complexity
- Preserve existing functionality
- Update documentation when architectural decisions change

## Coding Principles

Prioritize:

1. Correctness
2. Simplicity
3. Maintainability
4. Security
5. Consistency
6. User experience

## Project Conventions

Define:

- Naming conventions
- File organization
- Component conventions
- API conventions
- Database conventions
- Type conventions
- Testing conventions

## Dependencies

The agent should not introduce new dependencies without a clear reason.

Before adding a library, consider whether the functionality can reasonably be implemented using the existing stack.

## Architecture

The agent must follow the approved architecture.

It should not casually introduce:

- New frameworks
- New architectural patterns
- New databases
- New infrastructure
- Major dependencies

without justification.

## Uncertainty

When requirements are ambiguous:

- Check the PRD
- Check the architecture
- Check existing code
- Prefer established project conventions
- Ask for clarification when the decision has significant consequences

Do not invent major requirements.

## Before Changing Code

The agent should:

1. Understand the relevant requirement
2. Inspect the relevant code
3. Identify dependencies
4. Determine the smallest appropriate change
5. Implement the change
6. Test it
7. Verify that existing behavior was not unnecessarily affected

---

# Phase 6: Create FLOAT.md

Create `FLOAT.md` as a secondary AI instruction/reference file.

Avoid duplicating the complete contents of `AGENTS.md`.

Establish a clear relationship between the two files.

If `AGENTS.md` is the canonical instruction file, `FLOAT.md` should reference or defer to it rather than creating conflicting rules.

The project should have **one clear source of truth for agent behavior**.

---

# Phase 7: Design the Project Scaffold

Design the initial repository structure.

The scaffold should reflect the approved architecture.

For example:

```text
project/
├── PRD.md
├── ARCHITECTURE.md
├── ARCHITECTURE_ESSENTIALS.md
├── AGENTS.md
├── FLOAT.md
│
├── src/
│   ├── components/
│   ├── features/
│   ├── pages/
│   ├── services/
│   ├── lib/
│   ├── hooks/
│   ├── types/
│   └── utils/
│
├── server/
│   ├── api/
│   ├── services/
│   ├── models/
│   └── middleware/
│
├── database/
├── tests/
├── public/
└── configuration files
```

Do not blindly use this example.

The actual structure must be determined by the project's architecture and technology stack.

Create folders even when some are initially empty if doing so establishes a useful architectural boundary.

---

# Phase 8: Architectural Stress Test

Critically review the proposed product and architecture.

Ask:

## What could break?

Identify likely failure points.

## What edge cases are missing?

Consider relevant cases such as:

- Empty states
- Invalid input
- Duplicate records
- Authentication failures
- Authorization failures
- API failures
- Database failures
- Network interruptions
- Deleted resources
- Permission changes
- Concurrent operations
- Partial operations
- Unexpected user behavior
- Large datasets
- Security vulnerabilities

Only include cases relevant to the project.

## What are we over-engineering?

Identify unnecessary complexity.

Recommend simpler approaches where appropriate.

## What are we under-engineering?

Identify decisions that could create serious problems.

## What assumptions are risky?

Identify assumptions that should be validated.

## What should be simplified?

Look for opportunities to reduce:

- Infrastructure
- Dependencies
- Abstractions
- Code complexity
- Operational overhead
- Development time

---

# Phase 9: Update the Documents

After the architectural review, revise:

- `PRD.md`
- `ARCHITECTURE.md`
- `ARCHITECTURE_ESSENTIALS.md`
- `AGENTS.md`
- `FLOAT.md`
- Project scaffold

The final documents must represent the **reviewed architecture**, not the initial proposal.

Ensure there are no contradictions between the documents.

---

# Anti-Overengineering Rules

Always prefer:

```text
Simple
↓
Reliable
↓
Maintainable
↓
Scalable when necessary
```

Do not introduce complexity simply because it is technically impressive.

Avoid unnecessary:

- Microservices
- Event-driven architecture
- Message queues
- Distributed systems
- Multiple databases
- Complex caching
- Kubernetes
- Elaborate abstractions
- Premature optimization

unless the actual requirements justify them.

For an MVP, favor a **modular monolith or similarly simple architecture** unless there is a compelling reason not to.

---

# Final Deliverables

Before development begins, produce:

```text
PRD.md
ARCHITECTURE.md
ARCHITECTURE_ESSENTIALS.md
AGENTS.md
FLOAT.md
PROJECT SCAFFOLD
ARCHITECTURAL RISKS
EDGE CASES
OVER-ENGINEERED AREAS
UNDER-ENGINEERED AREAS
OPEN QUESTIONS
```

Then provide a final:

# Ready for Development Checklist

Verify:

- [ ] Product requirements are defined
- [ ] MVP scope is clear
- [ ] Out-of-scope features are identified
- [ ] User journeys are understood
- [ ] Technical architecture is defined
- [ ] Database models are defined
- [ ] Authentication/authorization is defined
- [ ] APIs/integrations are defined
- [ ] Security considerations are addressed
- [ ] Testing strategy is defined
- [ ] Project structure is defined
- [ ] AI coding-agent instructions are defined
- [ ] Major risks have been reviewed
- [ ] Important edge cases have been considered
- [ ] Over-engineering has been removed
- [ ] Open questions are clearly identified
- [ ] Documentation is internally consistent

Do not begin implementation until the planning stage has been completed and the user has approved the architecture.