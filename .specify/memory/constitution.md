
# Pizza Ordering System Constitution



## Core Principles

### I. Domain-Driven Design (DDD)
All business logic and boundaries are modeled using DDD principles. Ubiquitous language is enforced in code, documentation, and user interfaces. Aggregates, entities, and value objects are explicit and mapped to real-world concepts.

### II. Event-Driven Architecture
All state changes are modeled as events. Communication between bounded contexts and services is asynchronous and event-based. No direct cross-context calls except via events.

### III. Event Sourcing & Projections
All business state is persisted as an immutable event log. Projections are used to build query models and support read operations. No direct mutation of state outside event handlers.

### IV. Command Query Responsibility Separation (CQRS)
Commands and queries are strictly separated. Commands mutate state asynchronously; queries never cause side effects. All commands are tracked by unique command IDs.

### V. Saga Patterns (Choreography & Orchestration)
Long-running and distributed transactions are managed using sagas. Choreography is used for happy paths; orchestration is used for unhappy paths (e.g., rollbacks, compensations). Sagas are observable and debuggable.

### VI. Asynchronous Commands & Status Tracking
All commands are processed asynchronously. There is a central mechanism to query the status of any command by its unique ID. No synchronous command processing is allowed.

### VII. CloudEvents & Distributed Tracing
All events and commands use the CloudEvents specification. W3C distributed tracing (traceparent, tracestate) is propagated across all boundaries for end-to-end observability.

### VIII. TypeScript Strictness & Engineering Discipline
All code is written in TypeScript. No use of `any`. All functions and methods have explicit parameter and return types. Interfaces are used for all contracts. Dependency injection is mandatory for all services and providers. No scattered constants—use symbols or enums for lookups. Service providers (e.g., date/time) are injected, not instantiated directly.
Builder and factory patterns MUST be used where they provide clarity, flexibility, or testability (e.g., for complex object construction, service instantiation, or configuration).




## Engineering Constraints & Deployment Boundaries

- All implementation is in TypeScript (no JavaScript, no mixed languages).
- No use of `any` type anywhere in the codebase.
- All interfaces, types, and contracts must be explicit and documented.
- Dependency injection is required for all services, providers, and infrastructure.
- Service providers (e.g., date/time, random, config) must be injected.
- All constants must be centralized (no magic strings or numbers in code).
- All code must be independently testable and support mocking of dependencies.

### Deployment Boundaries & Structure
- Each bounded context maps to one or more microservices, each in its own top-level folder (no deep nesting).
- Each microservice folder contains its own source, tests, and docs.
- High-level documentation and event schemas must be maintained and versioned.

### Deployment Technology
- Every deployable component must have its own Dockerfile.
- One or more Docker Compose files must enable local deployment of all services.
- All infrastructure (databases, brokers, etc.) must be swappable for local, AWS, or Azure use via configuration/abstraction.
- If Terraform (IaC) is used, it must be separately deployable and live within the deployable component (e.g., src, tests, docs, iac folders).
- Future AWS deployment must be considered (Terraform if/when needed).

### Artifacts & Mono-Repo Practices
- Each deployable item must publish build artifacts to GitHub artifacts or equivalent.
- Terraform must only deploy infrastructure, not application code; code deployment is a separate step.
- Developers must be able to work on, test, and deploy a single microservice without affecting others.
- CI/CD pipelines must be templated and support selective build/deploy: only changed microservices are built/deployed on commit.
- On main branch commits: artifacts are published, Terraform infra is deployed, and code is deployed in separate steps.
- Robust CI/CD is mandatory: branch-based development, artifact publishing, infra/code separation, and mono-repo selective workflows are required.

### Separation of Concerns
- All infrastructure dependencies must be abstracted to allow local or cloud-native operation.
- No hard-coding of environment-specific details.



## Development Workflow & Quality Gates

- Test-First: All features and changes require tests before implementation (TDD strongly encouraged).
- Code Review: All code must be reviewed for adherence to principles and constraints.
- Quality Gates: No code may be merged unless it passes all tests, meets all principles, and is free of `any` and magic constants.
- Documentation: All public interfaces, events, and commands must be documented.
- Observability: All services must emit structured logs and support distributed tracing.



## Governance

- This constitution supersedes all other engineering practices for this project.
- Amendments require documentation, review, and a migration plan for any breaking changes.
- All PRs and reviews must verify compliance with every principle and constraint.
- Any complexity or deviation must be justified and documented in the plan.
- Constitution versioning follows semantic versioning: MAJOR for breaking/removal, MINOR for new/expanded principles, PATCH for clarifications.
- Compliance is reviewed at every major milestone and before each release.


**Version**: 1.2.1 | **Ratified**: 2026-01-30 | **Last Amended**: 2026-01-30
