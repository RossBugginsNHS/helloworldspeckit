# Implementation Plan: [FEATURE]

**Branch**: `[###-feature-name]` | **Date**: [DATE] | **Spec**: [link]
**Input**: Feature specification from `/specs/[###-feature-name]/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

[Extract from feature spec: primary requirement + technical approach from research]

## Technical Context

<!--
  ACTION REQUIRED: Replace the content in this section with the technical details
  for the project. The structure here is presented in advisory capacity to guide
  the iteration process.
-->

**Language/Version**: [e.g., Python 3.11, Swift 5.9, Rust 1.75 or NEEDS CLARIFICATION]  
**Primary Dependencies**: [e.g., FastAPI, UIKit, LLVM or NEEDS CLARIFICATION]  
**Storage**: [if applicable, e.g., PostgreSQL, CoreData, files or N/A]  
**Testing**: [e.g., pytest, XCTest, cargo test or NEEDS CLARIFICATION]  
**Target Platform**: [e.g., Linux server, iOS 15+, WASM or NEEDS CLARIFICATION]
**Project Type**: [single/web/mobile - determines source structure]  
**Performance Goals**: [domain-specific, e.g., 1000 req/s, 10k lines/sec, 60 fps or NEEDS CLARIFICATION]  
**Constraints**: [domain-specific, e.g., <200ms p95, <100MB memory, offline-capable or NEEDS CLARIFICATION]  
**Scale/Scope**: [domain-specific, e.g., 10k users, 1M LOC, 50 screens or NEEDS CLARIFICATION]


## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*


- Domain-Driven Design (DDD)
- Event-Driven Architecture
- Event Sourcing & Projections
- CQRS
- Saga Patterns (Choreography/Orchestration)
- Asynchronous Commands & Status Tracking
- CloudEvents & Distributed Tracing
- TypeScript Strictness & Engineering Discipline (no anys, DI, explicit types, no scattered constants)
- **Deployment Boundaries:** Each bounded context maps to one or more microservices, each in its own top-level folder (no deep nesting). Each microservice folder contains its own source, tests, and docs. High-level docs and event schemas must be maintained and versioned.
- **Deployment Technology:** Every deployable component must have its own Dockerfile. One or more Docker Compose files must enable local deployment. All infrastructure (databases, brokers, etc.) must be swappable for local, AWS, or Azure use via configuration/abstraction. Future AWS deployment must be considered (Terraform if/when needed).
- **Separation of Concerns:** All infrastructure dependencies must be abstracted to allow local or cloud-native operation. No hard-coding of environment-specific details.

**All plans must comply with the project constitution:**
- Domain-Driven Design (DDD)
- Event-Driven Architecture
- Event Sourcing & Projections
- CQRS
- Saga Patterns (Choreography/Orchestration)
- Asynchronous Commands & Status Tracking
- CloudEvents & Distributed Tracing
- TypeScript Strictness & Engineering Discipline (no anys, DI, explicit types, no scattered constants)
- **Deployment Boundaries:** Each bounded context maps to one or more microservices, each in its own top-level folder (no deep nesting). Each microservice folder contains its own source, tests, and docs. High-level docs and event schemas must be maintained and versioned.
- **Deployment Technology:** Every deployable component must have its own Dockerfile. One or more Docker Compose files must enable local deployment. All infrastructure (databases, brokers, etc.) must be swappable for local, AWS, or Azure use via configuration/abstraction. If Terraform (IaC) is used, it must be separately deployable and live within the deployable component (e.g., src, tests, docs, iac folders). Future AWS deployment must be considered (Terraform if/when needed).
- **Artifacts & Mono-Repo Practices:** Each deployable item must publish build artifacts to GitHub artifacts or equivalent. Terraform must only deploy infrastructure, not application code; code deployment is a separate step. Developers must be able to work on, test, and deploy a single microservice without affecting others. CI/CD pipelines must be templated and support selective build/deploy: only changed microservices are built/deployed on commit. On main branch commits: artifacts are published, Terraform infra is deployed, and code is deployed in separate steps. Robust CI/CD is mandatory: branch-based development, artifact publishing, infra/code separation, and mono-repo selective workflows are required.
- **Separation of Concerns:** All infrastructure dependencies must be abstracted to allow local or cloud-native operation. No hard-coding of environment-specific details.

Any violation must be justified in the Complexity Tracking section below.

## Project Structure

### Documentation (this feature)

```text
specs/[###-feature]/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)
<!--
  ACTION REQUIRED: Replace the placeholder tree below with the concrete layout
  for this feature. Delete unused options and expand the chosen structure with
  real paths (e.g., apps/admin, packages/something). The delivered plan must
  not include Option labels.
-->

```text
# [REMOVE IF UNUSED] Option 1: Single project (DEFAULT)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [REMOVE IF UNUSED] Option 2: Web application (when "frontend" + "backend" detected)
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [REMOVE IF UNUSED] Option 3: Mobile + API (when "iOS/Android" detected)
api/
└── [same as backend above]

ios/ or android/
└── [platform-specific structure: feature modules, UI flows, platform tests]
```

**Structure Decision**: [Document the selected structure and reference the real
directories captured above]

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |
