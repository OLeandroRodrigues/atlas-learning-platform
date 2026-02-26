# ADR-0002: Refresh Token Storage & Rotation

**Status:** Accepted
**Date:** 2026-02-26
**Decision Makers:** Atlas.LearningPlatform Maintainers

---

## Context

Atlas.LearningPlatform uses short-lived JWT access tokens for API authentication.
To maintain user sessions without repeatedly requesting credentials, a secure refresh mechanism is required.

The refresh token strategy must:

* Allow controlled session continuity
* Enable explicit session revocation
* Reduce exposure window in case of token theft
* Be compatible with stateless API architecture
* Support multiple concurrent sessions (e.g., multiple devices)

Improper refresh token handling can introduce replay vulnerabilities and long-lived credential exposure.

---

## Decision

The system will implement **server-side stored, rotating refresh tokens** with the following characteristics:

### 1. Server-Side Storage

Refresh tokens are persisted in the database and associated with:

* User identifier
* Unique token identifier
* Expiration timestamp
* Revocation flag
* Creation timestamp
* Optional session identifier

Refresh tokens are never trusted purely based on client possession.

---

### 2. Rotation on Every Use

Refresh tokens are **single-use**.

On successful refresh:

1. The existing refresh token is validated
2. The existing refresh token is invalidated
3. A new refresh token is generated and stored
4. A new access token is issued

This prevents replay attacks if a refresh token is intercepted.

---

### 3. Replay Detection

If a refresh token is presented after it has already been rotated:

* The reuse attempt is detected
* The associated session is revoked
* Optional: all sessions for that user may be revoked (defense-in-depth)

This protects against token theft and session hijacking.

---

### 4. Explicit Revocation

Refresh tokens may be revoked when:

* User logs out
* Password is changed
* Suspicious activity is detected
* Administrative action occurs

Revocation is enforced server-side.

---

## Alternatives Considered

### 1. Non-Rotating Refresh Tokens

* Simpler implementation
* Vulnerable to replay attacks
* Harder to detect compromised tokens

**Rejected** due to weaker security posture.

---

### 2. Stateless Refresh Tokens

* No server storage required
* Reduced persistence complexity
* No explicit revocation capability

**Rejected** because revocation and session control are required.

---

### 3. Session-Based Authentication Only

* Traditional server-side sessions
* Requires sticky sessions or centralized store

**Rejected** to preserve stateless API characteristics and scalability.

---

## Consequences

### Positive

* Strong protection against refresh token replay
* Fine-grained session revocation
* Auditability of authentication sessions
* Production-aligned security posture

### Trade-offs

* Requires database storage
* Slight increase in implementation complexity
* Requires careful transaction handling during rotation

---

## Security Considerations

* Refresh tokens must never be logged in plaintext
* Refresh tokens must not be committed to version control
* Rotation logic must be atomic
* Rate limiting should be applied to refresh endpoints

---

## Future Evolution

Potential enhancements may include:

* Device fingerprinting per session
* Token binding strategies
* Anomaly detection for abnormal refresh behavior

Any modification to the refresh strategy must be captured in a new ADR and must not silently alter existing guarantees.

---

## Rationale

Rotating, server-stored refresh tokens represent a balanced approach between:

* Stateless access token validation
* Explicit session lifecycle control
* Production-grade security expectations

This decision aligns with industry practices commonly used in high-scale backend systems.
