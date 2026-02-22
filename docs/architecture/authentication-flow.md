# Authentication Flow

---

## 1. Purpose

This document describes the end-to-end authentication flow for **Atlas.LearningPlatform**, including token issuance, validation, refresh, and session lifecycle management.

The authentication model is designed to be:

* Stateless at the API layer
* Secure against common token-based threats
* Compatible with SPA and mobile clients
* Explicit in terms of session lifecycle

---

## 2. High-Level Flow

The authentication flow follows a token-based model:

```
Client → Login Request → API
API → Validate Credentials
API → Issue Access Token (JWT)
API → Issue Refresh Token
Client → Use Access Token for API calls
Client → Use Refresh Token to obtain new Access Token
```

Two token types are involved:

* **Access Token (JWT)** – short-lived, used for authorization
* **Refresh Token** – long-lived, used to renew access tokens

---

## 3. Registration Flow

1. Client sends registration request
2. API validates input
3. Password is hashed using a secure hashing algorithm
4. User entity is persisted
5. Optional: issue initial tokens

Validation and business rules are enforced in the Application layer.

---

## 4. Login Flow

1. Client submits credentials
2. API validates credentials
3. Access token (JWT) is generated
4. Refresh token is generated and stored server-side
5. Tokens are returned to the client

### Access Token Characteristics

* Short-lived (e.g., 10–15 minutes)
* Contains claims (user id, roles, permissions)
* Signed using a secure key

### Refresh Token Characteristics

* Long-lived
* Stored server-side
* Associated with a specific session

---

## 5. Token Validation Flow

For each authenticated request:

1. Client sends access token in Authorization header
2. Middleware validates signature and expiration
3. Claims are extracted
4. Authorization policies are evaluated
5. Request proceeds or is rejected

Authorization logic is handled separately from authentication.

---

## 6. Refresh Flow

When the access token expires:

1. Client sends refresh token
2. API validates token existence and status
3. Refresh token is rotated (invalidated and replaced)
4. New access token is issued
5. New refresh token is returned

Token rotation prevents replay attacks and improves session security.

---

## 7. Logout & Session Revocation

On logout:

* The associated refresh token is invalidated
* The session is marked as revoked

If a compromised token is detected, individual sessions can be revoked without affecting other active sessions.

---

## 8. Security Considerations

The authentication system accounts for:

* Token expiration enforcement
* Replay attack mitigation (refresh rotation)
* Password hashing best practices
* Separation of authentication and authorization
* Explicit session tracking

Further security hardening may include:

* Rate limiting on login endpoints
* Account lockout mechanisms
* Multi-factor authentication (future consideration)

---

## 9. Boundaries

Authentication concerns are handled in:

* Application layer (use cases)
* Infrastructure layer (token generation, persistence)
* API layer (HTTP endpoints and middleware configuration)

Domain layer remains independent of token mechanisms.

---

## Notes

This document will be updated as implementation details are finalized and security trade-offs are captured in ADRs.
