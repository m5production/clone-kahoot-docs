# Clone-Kahoot: business requirements

Product and business documentation for **Clone-Kahoot**, a real-time quiz game inspired by Kahoot. A host builds a quiz and runs it live; players join from their own devices, type answers and see the winners table at the end.

The requirements were reverse-engineered from the current code of both repositories and describe **what the product does today**, the **business rules** it follows, and the **gaps** between the current implementation and a product that is ready for real users.

| Repository | Role |
|---|---|
| [clone-kahoot](https://github.com/m5production/clone-kahoot) | Frontend: React SPA, hosted on GitHub Pages ([live demo](https://dimetriyo.github.io/clone-kahoot/)) |
| [clone-kahoot-backend](https://github.com/m5production/clone-kahoot-backend) | Backend: Express REST API + WebSocket server, Prisma + SQLite, Docker on Amvera |

## Contents

1. [Product vision](docs/01-product-vision.md): problem, goals, target users, scope
2. [Roles and user stories](docs/02-roles-and-user-stories.md): personas, user stories, acceptance criteria
3. [Functional requirements](docs/03-functional-requirements.md): numbered requirements with implementation status
4. [Business rules](docs/04-business-rules.md): authentication, quiz building, answering and scoring rules
5. [Domain model and glossary](docs/05-domain-model.md): entities, relations, terms
6. [Live game flow](docs/06-live-game-flow.md): game lifecycle, sequence of events, real-time message protocol
7. [Non-functional requirements](docs/07-non-functional-requirements.md): security, performance, availability, UX
8. [Gaps and backlog](docs/08-gaps-and-backlog.md): known issues and a prioritized product backlog

## Status legend

Requirements use these statuses:

| Status | Meaning |
|---|---|
| Implemented | Works end to end in the current code |
| Partial | Works with limitations described in the requirement |
| Not implemented | Needed for the product, missing in the code |

## How to update

- Every requirement has a stable ID (`FR-…`, `BR-…`, `NFR-…`, `GAP-…`). Reference these IDs in issues and PRs of the frontend and backend repositories.
- When a feature ships, update its status and the related backlog item in the same PR.
