# JWT Access Tokens

## 1. Purpose

This document defines how **JWT access tokens** are issued, validated, and used within **Atlas.LearningPlatform**.

JWT access tokens are designed to:

* Enable stateless API authentication
* Carry identity and authorization context (claims)
* Support policy-based authorization at the API boundary
* Minimize session risk via short-lived lifetimes

---

## 2. Token Role in the System

JWT access tokens are used for **authentication on each request**.

A typical request includes:

* `Authorization: Bearer <access_token>`

The API validates the token and extracts claims that are later evaluated by authorization policies.

JWT access tokens are intentionally **short-lived**.

---

## 3. Token Contents (Claims)

Access tokens will include a minimal set of claims needed for authorization and auditing.

### Planned Required Claims

* `sub` (subject / user identifier)
* `jti` (token identifier)
* `iat` (issued at)
* `exp` (expiration)

### Planned Authorization Claims

One or more of:

* `role` (coarse-grained access)
* `perm` or `permissions` (fine-grained permissions)

The final claim shape should remain stable and be treated as part of the API contract.

---

## 4. Token Lifetime

Access tokens should be configured with a short expiration window.

Planned guideline:

* 10–15 minutes lifetime

Rationale:

* Reduces the impact of token theft
* Forces session continuity through refresh flow
* Limits exposure window in compromised clients

---

## 5. Token Signing and Validation

### Signing

Tokens are signed using a secret key.

Key requirements:

* Strong entropy
* Stored outside source control
* Rotatable through configuration

### Validation

On each request, middleware validates:

* Signature validity
* Token expiration
* Issuer/audience (if configured)
* Required claims

Validation failures result in:

* `401 Unauthorized`

---

## 6. Key Management

Signing keys must not be committed to the repository.

Planned approach:

* Local development: environment variables or `.env` (gitignored)
* CI/CD: secret store (e.g., GitHub Secrets)
* Production: managed secrets (e.g., Key Vault / Secrets Manager)

Key rotation strategy (future):

* Support multiple valid keys during a transition window
* Invalidate tokens signed with deprecated keys after a safe period

---

## 7. Security Considerations

JWT access tokens are treated as bearer credentials.

Risks and mitigations:

* **Token theft** → short lifetimes, refresh rotation, secure client storage
* **Replay** → rotate refresh tokens; access token replay is bounded by short TTL
* **Over-privileged tokens** → minimal claims; permissions derived intentionally
* **Sensitive data exposure** → never include PII or secrets in token claims

---

## 8. Authorization Integration

Authorization is evaluated after authentication.

* The authenticated principal is built from token claims
* Policies validate required roles/permissions

The authorization model is documented separately:

* `docs/architecture/authorization-model.md`

---

## 9. Non-Goals

JWT access tokens will not initially support:

* Encryption (JWE)
* Complex ABAC decision graphs
* External federation protocols (SAML/OIDC) as primary auth method

These can be introduced later if required.

---

## Notes

This document will be updated once implementation details are finalized and key rotation requirements become explicit.

Significant changes to token structure or validation rules should be captured via ADRs.
