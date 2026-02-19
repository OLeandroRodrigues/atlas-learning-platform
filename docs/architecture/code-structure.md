# Code Structure & Architecture Mapping

## Status

⏳ **Planned**

---

## 1. Purpose

This document explains how the high-level architectural design of **Atlas.LearningPlatform** maps directly to the codebase structure.

The goal is to ensure that:

* Architectural boundaries are visible in the folder structure
* Responsibilities are clearly separated
* Dependency direction is enforced by design
* The codebase remains maintainable as the system evolves

This mapping is intentional and reflects production-grade backend organization.

---

## 2. Solution Structure

The solution is organized into explicit architectural layers:

```
/src
  Atlas.LearningPlatform.Api
  Atlas.LearningPlatform.Application
  Atlas.LearningPlatform.Domain
  Atlas.LearningPlatform.Infrastructure
/tests
  Atlas.LearningPlatform.UnitTests
  Atlas.LearningPlatform.IntegrationTests
```

Each project corresponds to a well-defined architectural responsibility.

---

## 3. Layer Responsibilities

### 3.1 Domain Layer

**Project:** `Atlas.LearningPlatform.Domain`

Responsibilities:

* Core entities (e.g., User, Session)
* Domain rules and invariants
* Value objects
* Domain exceptions

Constraints:

* No dependency on external frameworks
* No dependency on EF Core, ASP.NET, or infrastructure libraries

The Domain layer represents the core business model and must remain framework-agnostic.

---

### 3.2 Application Layer

**Project:** `Atlas.LearningPlatform.Application`

Responsibilities:

* Use cases (e.g., RegisterUser, LoginUser, RefreshToken)
* Application services
* Interfaces for infrastructure dependencies
* Validation logic

Constraints:

* Depends on Domain
* Defines abstractions implemented by Infrastructure
* Does not reference ASP.NET Core directly

This layer orchestrates business logic without being tied to delivery mechanisms.

---

### 3.3 Infrastructure Layer

**Project:** `Atlas.LearningPlatform.Infrastructure`

Responsibilities:

* EF Core persistence implementation
* Identity integration
* Token generation services
* External integrations (future)
* Database configuration and migrations

Constraints:

* Implements interfaces defined in Application
* May depend on external libraries and frameworks

Infrastructure details are isolated here to prevent leakage into higher layers.

---

### 3.4 API Layer

**Project:** `Atlas.LearningPlatform.Api`

Responsibilities:

* HTTP endpoints
* Request/response models
* Middleware configuration
* Dependency injection wiring
* Authentication/authorization configuration

Constraints:

* Depends only on Application
* No business logic beyond orchestration

The API layer acts as a thin delivery mechanism.

---

## 4. Dependency Direction

The dependency rule is strictly enforced:

```
API → Application → Domain
Infrastructure → Application
```

* Domain has no outward dependencies.
* Application depends only on Domain abstractions.
* Infrastructure depends on Application and Domain.
* API depends on Application.

This prevents circular dependencies and enforces architectural discipline.

---

## 5. Mapping Use Cases to Code

Each significant feature is represented as an explicit use case in the Application layer.

Example mapping:

| Feature           | Application Component | Infrastructure Component | API Component    |
| ----------------- | --------------------- | ------------------------ | ---------------- |
| User Registration | `RegisterUser`        | `UserRepository`         | `AuthController` |
| Login             | `LoginUser`           | `JwtTokenService`        | `AuthController` |
| Token Refresh     | `RefreshToken`        | `RefreshTokenRepository` | `AuthController` |

This makes feature ownership and flow traceable across layers.

---

## 6. Testing Structure Mapping

Testing projects mirror the production layers:

* `UnitTests` focus on Domain and Application logic
* `IntegrationTests` focus on API and infrastructure wiring

This separation ensures both behavioral correctness and system-level validation.

---

## 7. Evolution Strategy

As the system evolves:

* New features must introduce new use cases in the Application layer
* Infrastructure changes must not leak into Domain
* API changes must not introduce business logic

Structural discipline is maintained through code reviews and ADR documentation.

---

## Notes

This document will evolve as new modules or architectural refinements are introduced.

The primary objective remains: **architecture clarity reflected directly in the codebase structure.**
