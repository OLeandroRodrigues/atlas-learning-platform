# Authorization Model (Roles & Policies)

## 1. Purpose

This document defines how **Atlas.LearningPlatform** models authorization, including roles, policies, and fine-grained permissions.

The authorization model is designed to:

* Keep authorization decisions explicit and traceable
* Scale from simple role checks to fine-grained permissions
* Avoid leaking infrastructure/framework concerns into the Domain
* Support protected admin operations and future product expansion

---

## 2. Key Principles

1. **Authentication and authorization are separate concerns**

   * Authentication establishes *who* the caller is
   * Authorization determines *what* the caller can do

2. **Policies are the long-term scaling mechanism**

   * Roles are useful for coarse-grained access
   * Policies (permissions/claims) provide fine-grained control

3. **Authorization is evaluated at the API boundary**

   * The API layer enforces access through policies
   * The Application layer may also enforce invariants when needed

---

## 3. Roles (Coarse-Grained Access)

The system uses roles to represent broad access groups.

### Planned Roles

* **User**

  * Default role for authenticated users
* **Admin**

  * Full control of privileged operations
* **Editor** (optional, can be introduced later)

  * Content management capabilities without user administration

Roles should be treated as a convenience mechanism, not the primary long-term authorization strategy.

---

## 4. Policies (Fine-Grained Access)

Policies provide fine-grained authorization through claims/permissions.

### Permission Naming

Permissions follow a consistent naming convention:

* `users:read`
* `users:write`
* `content:read`
* `content:write`
* `content:publish`
* `admin:access`

This style is:

* Human-readable
* Easy to audit
* Compatible with modern authorization systems

### Policy Examples

* **CanManageUsers** → requires `users:write`
* **CanWriteContent** → requires `content:write`
* **CanPublishContent** → requires `content:publish`
* **CanAccessAdmin** → requires `admin:access`

---

## 5. Authorization Evaluation

Authorization is primarily enforced in the API layer:

* Endpoints are annotated with role checks or policy requirements
* Policies are evaluated from the authenticated user's claims

### Example Endpoint Classification

* Public endpoints

  * No authorization required

* Authenticated endpoints

  * Require a valid access token

* Admin endpoints

  * Require admin-level policy/role

---

## 6. Claim Population Strategy

Claims required for policy evaluation must be provided consistently.

Planned sources of claims:

* Role claims derived from user roles
* Permission claims derived from role-to-permission mapping

Permissions may be:

* Stored explicitly per user
* Derived from roles at token issuance time

The chosen approach should be documented in an ADR once implemented.

---

## 7. Defense-in-Depth

While endpoint policies provide the primary enforcement mechanism, defense-in-depth principles apply:

* Sensitive operations may be validated again in Application use cases
* Authorization failures are logged (without leaking secrets)
* Admin operations require explicit policies, not implicit access

This reduces the impact of accidental misconfiguration.

---

## 8. Non-Goals

This authorization model is not intended to:

* Support attribute-based access control (ABAC) initially
* Implement complex hierarchical permission graphs
* Expose a full IAM product surface

The goal is clarity, correctness, and scalability for typical backend systems.

---

## 9. Notes

This document will be updated as the authorization model is implemented and refined.

Significant changes should be captured via ADRs (e.g., role vs policy decisions, claim strategy, token contents).
