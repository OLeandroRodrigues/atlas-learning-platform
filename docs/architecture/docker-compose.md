# Local Development with Docker Compose

## Status

⏳ **Planned**

---

## Context

Local development environments should be **reproducible**, **isolated**, and **close to production**. For backend systems, this usually involves running multiple infrastructure dependencies (such as databases) alongside the API.

Docker Compose is commonly used to orchestrate these dependencies in local environments while keeping setup friction low for developers.

---

## Goals

The goal of using Docker Compose in this project is to:

* Provide a consistent local development environment
* Reduce onboarding time for new contributors
* Avoid manual installation of infrastructure dependencies
* Mirror production topology where reasonable

---

## Planned Components

The local environment is expected to include:

* **API service**

  * Built from the project Dockerfile
  * Runs the ASP.NET Core application

* **Database service**

  * PostgreSQL instance
  * Used exclusively for local development

Optional future components:

* Database migration runner
* Observability tooling (e.g. log aggregation, tracing)

---

## Configuration Strategy

Planned configuration principles:

* Environment variables used for all runtime configuration
* No secrets committed to the repository
* `.env` file supported for local overrides (gitignored)
* Same configuration keys used across environments

---

## Development Workflow (Planned)

Typical local workflow:

1. Clone the repository
2. Configure environment variables (or `.env` file)
3. Run `docker compose up`
4. Access the API via exposed HTTP port

This allows developers to start the full stack with a single command.

---

## Non-Goals

The Docker Compose setup is **not** intended to:

* Replace production deployment tooling
* Support horizontal scaling scenarios
* Act as a full infrastructure simulation

Its purpose is limited to **local development and testing**.

---

## Notes

This document will be updated once the Docker Compose configuration is implemented.

Design decisions related to containerization and local environments may be captured separately as ADRs if they become significant architectural concerns.
