# Refresh Tokens & Session Management

## 1. Purpose

This document defines how **refresh tokens** are handled within Atlas.LearningPlatform, including issuance, storage, rotation, and session lifecycle management.

Refresh tokens exist to:

* Maintain user sessions without requiring repeated credential entry
* Support short-lived access tokens
* Enable controlled session revocation
* Reduce the security risk associated with long-lived bearer tokens

---

## 2. Role in the Authentication Model

The authentication system uses two-token strategy:

* **Access Token (JWT)** → short-lived, used for API requests
* **Refresh Token** → long-lived, used only to obtain new access tokens

Access tokens expire quickly.
Refresh tokens extend session continuity in a controlled manner.

---

## 3. Refresh Token Lifecycle

### 3.1 Issuance

Refresh tokens are generated during:

* Successful login
* Successful token refresh

Each refresh token is:

* Cryptographically random
* Unique per session
* Associated with a specific user

---

### 3.2 Storage Strategy

Refresh tokens are stored **server-side**.

Planned attributes:

* Token identifier
* User identifier
* Expiration timestamp
* Revocation flag
* Creation timestamp
* Session identifier (optional)

Storing refresh tokens server-side enables explicit revocation and auditability.

---

### 3.3 Rotation

Refresh tokens are **rotated on every use**.

Flow:

1. Client sends refresh token
2. Server validates token status
3. Server invalidates the existing refresh token
4. Server issues a new refresh token
5. Server issues a new access token

Rotation prevents replay attacks and limits damage from token theft.

---

## 4. Expiration Policy

Refresh tokens are long-lived but finite.

Planned guideline:

* Valid for several days (exact duration configurable)

If a refresh token expires:

* The user must re-authenticate with credentials

Expiration policies are configurable through environment settings.

---

## 5. Session Management

Each refresh token corresponds to a logical session.

This allows:

* Multiple concurrent sessions per user (e.g., multiple devices)
* Revocation of a specific session
* Global logout (invalidate all active sessions)

Session metadata may include:

* Creation timestamp
* Last usage timestamp
* Device or client metadata (future enhancement)

---

## 6. Revocation Strategy

Refresh tokens can be revoked in the following scenarios:

* User logout
* Password change
* Suspicious activity detected
* Administrative action

Revocation invalidates only the targeted session unless explicitly configured for global revocation.

---

## 7. Security Considerations

Refresh tokens represent high-value credentials.

Mitigation strategies include:

* Server-side storage
* Token rotation
* Expiration enforcement
* Audit logging of refresh operations
* Rate limiting on refresh endpoint

Refresh tokens must never be:

* Logged in plaintext
* Stored in version control
* Embedded in client-side scripts

---

## 8. Failure Scenarios

### Token Reuse (Replay Attempt)

If a refresh token is used after it has already been rotated:

* The system detects token reuse
* The session is invalidated
* Optional: all sessions for the user may be revoked

This behavior should be documented and consistent.

---

## 9. Non-Goals

The refresh token strategy does not initially include:

* Distributed cache invalidation across regions
* Advanced anomaly detection systems
* Full identity federation

The goal is clarity and correctness for a production-oriented backend reference.

---

## 10. Relationship to ADR

The refresh token strategy is formalized in:

- [ADR-0002 — Refresh Token Storage & Rotation](/docs/decisions/ADR-0002-refresh-token-strategy.md)

Any changes to rotation logic or storage strategy must be captured in a new ADR.

---

## Notes

This document will evolve as implementation details are finalized and session management features expand.
