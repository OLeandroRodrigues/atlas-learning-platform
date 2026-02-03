## Architecture Documentation

### 0. Architecture & Code Structure

| Document                                                                     | Description                           | Status    |
| ---------------------------------------------------------------------------- | ------------------------------------- | --------- |
| [Architecture Overview](docs/architecture/architecture-overview.md)          | High-level system view and boundaries | ⏳ Planned |
| [Code Structure & Architecture Mapping](docs/architecture/code-structure.md) | How architecture maps to the codebase | ⏳ Planned |

---

### 1. Core Concepts

|   # | Document                                                                           | Status    |
| --: | ---------------------------------------------------------------------------------- | --------- |
| 1.1 | [Authentication Flow](docs/architecture/authentication-flow.md)                    | ⏳ Planned |
| 1.2 | [Authorization Model (Roles & Policies)](docs/architecture/authorization-model.md) | ⏳ Planned |
| 1.3 | [JWT Access Tokens](docs/architecture/jwt-access-tokens.md)                        | ⏳ Planned |
| 1.4 | [Refresh Tokens & Session Management](docs/architecture/refresh-tokens.md)         | ⏳ Planned |
| 1.5 | [Session Revocation & Logout](docs/architecture/session-revocation.md)             | ⏳ Planned |
| 1.6 | [Error Handling & Problem Details](docs/architecture/problem-details.md)           | ⏳ Planned |
| 1.7 | [Clean Architecture Boundaries](docs/architecture/clean-architecture.md)           | ⏳ Planned |

---

### 2. Architectural Decisions (ADR)

|        # | Decision                                                                                       | Status      |
| -------: | ---------------------------------------------------------------------------------------------- | ----------- |
| ADR-0001 | [Authentication Strategy (JWT + Refresh Tokens)](docs/adr/ADR-0001-authentication-strategy.md) | ⏳ Planned  |
| ADR-0002 | [Refresh Token Storage & Rotation](docs/adr/ADR-0002-refresh-token-strategy.md)                | ⏳ Planned  |
| ADR-0003 | [Clean Architecture & Layered Design](docs/adr/ADR-0003-clean-architecture.md)                 | ⏳ Planned  |
| ADR-0004 | [Authorization Model: Roles vs Policies](docs/adr/ADR-0004-authorization-model.md)             | ⏳ Planned  |

---

### 3. Platform & Infrastructure

|   # | Document                                                                                 | Status    |
| --: | ---------------------------------------------------------------------------------------- | --------- |
| 3.1 | [Persistence Strategy (EF Core & PostgreSQL)](docs/architecture/persistence-strategy.md) | ⏳ Planned |
| 3.2 | [Database Migrations & Seeding](docs/architecture/migrations-and-seeding.md)             | ⏳ Planned |
| 3.3 | [Observability & Logging](docs/architecture/observability.md)                            | ⏳ Planned |
| 3.4 | [Health Checks & Readiness](docs/architecture/health-checks.md)                          | ⏳ Planned |

---

### 4. Quality & Delivery

|   # | Document                                                                                 | Status    |
| --: | ---------------------------------------------------------------------------------------- | --------- |
| 4.1 | [Testing Strategy](docs/architecture/testing-strategy.md)                                | ⏳ Planned |
| 4.2 | [Integration Testing with WebApplicationFactory](docs/architecture/integration-tests.md) | ⏳ Planned |
| 4.3 | [CI Pipeline (GitHub Actions)](docs/architecture/ci-pipeline.md)                         | ⏳ Planned |
| 4.4 | [Local Development with Docker Compose](docs/architecture/docker-compose.md)             | ⏳ Planned |

---

## Documentation Status Legend

| Status        | Meaning                                          |
| ------------- | ------------------------------------------------ |
| ❌ Not Started | Document has been identified but not created yet |
| ⏳ Planned     | Document structure defined, content pending      |
| 📝 Draft      | Initial version written, subject to changes      |
| ✅ Completed   | Content is complete and stable                   |
| ⚠️ Deprecated | Document no longer reflects current architecture |

---

### 📌 Notes

* This repository intentionally documents **architecture first**, with code evolving incrementally.
* Documentation status is kept explicit to reflect the current maturity of each area.
* Architectural Decision Records (ADR) are treated as immutable once accepted.
