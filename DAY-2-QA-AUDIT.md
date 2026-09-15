# DAY-2-QA-AUDIT

**QA Engineer:** Manshi  
**Day:** Day 2  
**Date:** 2026-09-15  
**Environment:** Local QA environment  
**Backend:** `http://localhost:3000`  
**Backend commit tested:** `ff3d34c8d267a4a40ed70350b136edd4974ba622`

---

## 1. Day-2 Scope

This Day-2 audit records the QA execution and regression evidence completed during the current testing session.

Primary areas exercised:
- Users
- Addresses
- Preferences
- User History
- Authentication / Signup OTP dependency
- Database verification
- Automated regression suite
- API authentication/security checks

The execution followed the available QA plan and records actual observed results only.

---

## 2. Environment Verification

| Check | Result |
|---|---|
| Backend running | PASS |
| Health endpoint | PASS |
| PostgreSQL container | PASS / healthy |
| Redis container | PASS / running |
| Backend commit recorded | PASS |

Health endpoint result:

```json
{
  "success": true,
  "data": {
    "status": "ok"
  }
}
```

---

## 3. Manual API Test Results

### Users

| Test Case | Result | Observation |
|---|---|---|
| GET `/users/:id` without authentication | PASS | Returned `401 AUTHENTICATION_REQUIRED` |
| PATCH `/users/:id` without authentication | PASS | Returned `401 AUTHENTICATION_REQUIRED` |
| POST `/users` | PASS | Route is not exposed; returned `404 Cannot POST /users` |
| Invalid user UUID | BLOCKED | Authentication middleware returned `401` before UUID validation could be reached |

### Addresses

| Test Case | Result | Observation |
|---|---|---|
| GET `/users/:id/addresses` without authentication | PASS | Returned `401 AUTHENTICATION_REQUIRED` |
| POST `/users/:id/addresses` without authentication | PASS | Returned `401 AUTHENTICATION_REQUIRED` |
| PATCH `/users/:id/addresses/:addressId` without authentication | PASS | Returned `401 AUTHENTICATION_REQUIRED` |

### Preferences

| Test Case | Result | Observation |
|---|---|---|
| GET `/users/:id/preferences` without authentication | PASS | Returned `401 AUTHENTICATION_REQUIRED` |

### User History

| Test Case | Result | Observation |
|---|---|---|
| GET `/users/:id/history` without authentication | PASS | Returned `401 AUTHENTICATION_REQUIRED` |

---

## 4. Signup / OTP Testing

### Signup

The real signup endpoint was exercised using the normal application flow.

Observed:
- Signup request returned HTTP `201 Created`.
- Sendmator returned a provider session.
- Backend diagnostic output reported:
  - `send_failed: true`
  - provider error: `Request failed with status code 400`
- No SMS OTP was received.

### Signup OTP Verification

Verification could not be completed because no OTP was received.

An invalid/example OTP resulted in:
- HTTP `400`
- `INVALID_OTP`

This was not counted as a valid-OTP test.

### Signup OTP Resend

`POST /auth/signup/resend` returned:

```json
{
  "success": false,
  "error": {
    "code": "OTP_PROVIDER_RESEND_FAILED",
    "message": "Failed to resend verification code"
  }
}
```

### OTP Status

**BLOCKED**

The normal signup → OTP verification flow cannot currently be completed because Sendmator SMS delivery/resend is failing.

---

## 5. Database Verification

### Users

Query confirmed:

```text
(0 rows)
```

Result:
- No user was created by the unverified signup.
- PASS for preventing unverified signup from creating a user.

### Pending Signup

The tested signup created a pending signup with:
- `contact_type = phone`
- `role = customer`
- `otp_provider = sendmator`
- Provider session ID present
- Provider expiry present
- `otp_attempts = 0`
- `otp_verified_at = NULL`

The provider session subsequently expired because the configured expiry period elapsed.

Sensitive fields such as password hashes and provider session tokens were not inspected or exposed.

---

## 6. Automated Regression Test Results

Command executed:

```text
npm test
```

The first attempted command used `--runInBand`, which is a Jest option and is not supported by this Vitest project. The correct `npm test` command was then executed successfully.

Final regression result:

```text
Test Files  60 passed | 3 skipped (63)
Tests       540 passed | 12 skipped (552)
Failures    0
```

### Regression Status

- PASS: 60 test files
- PASS: 540 tests
- SKIPPED: 3 test files / 12 tests
- FAILURES: 0

Skipped tests were reported by the existing suite and include database/load integration coverage for rides and rentals. They were not counted as failures.

---

## 7. Defects / Blockers

### BUG-01 — Signup OTP SMS Delivery Failure

**Severity:** High / Blocking for signup verification

**Description:**
The signup API returns `201 Created` and creates a pending signup even though the Sendmator response reports SMS delivery failure.

**Evidence:**
```text
send_failed: true
error: Request failed with status code 400
```

**Impact:**
The user does not receive the OTP, so normal signup verification cannot be completed.

**Current status:** BLOCKED / OPEN

---

### BUG-02 — Signup OTP Resend Failure

**Severity:** High / Blocking for signup verification

**Description:**
The signup OTP resend endpoint returns `OTP_PROVIDER_RESEND_FAILED`.

**Impact:**
The user cannot recover from the missing OTP through the resend flow.

**Current status:** BLOCKED / OPEN

---

## 8. Security Observations

Unauthenticated requests to the tested User, Address, Preferences, and History endpoints were consistently rejected with `401 AUTHENTICATION_REQUIRED`.

No password, OTP, provider session token, or other credential material was intentionally exposed during QA.

No manual database modification was performed.

---

## 9. Tests Blocked by OTP Dependency

The following require a valid authenticated user and therefore could not be completed through the normal application flow during this execution:

- Authenticated user profile retrieval
- Authenticated profile update
- Cross-user profile access
- Address CRUD as an authenticated owner
- Cross-user address access
- Preferences update
- Cross-user preferences access
- Authenticated history retrieval
- Cross-user history access
- Authenticated negative/edge cases requiring a real user

These are **BLOCKED**, not counted as PASS or FAIL.

---

## 10. Test Count Summary

### Manual tests executed

- PASS: 8
- BLOCKED: 1 explicitly attempted validation case
- OTP flow: BLOCKED
- Additional authenticated cases: BLOCKED due to OTP dependency

### Automated tests

- PASS: 540
- FAIL: 0
- SKIPPED: 12

**No test counts have been invented.**

---

## 11. Overall Day-2 Status

**PARTIALLY COMPLETE — BLOCKED BY SENDMATOR OTP DELIVERY**

The backend is running correctly in the local environment, the tested unauthenticated User/Address/Preferences/History routes enforce authentication, the unverified signup did not create a user, and the automated regression suite completed with zero failures.

However, the Sendmator SMS delivery/resend failure prevents completion of authenticated end-to-end User-domain testing.

### Carry-forward

1. Resolve or investigate Sendmator SMS delivery HTTP 400.
2. Retest signup OTP delivery.
3. Complete authenticated User profile tests.
4. Complete authenticated Address CRUD and ownership tests.
5. Complete Preferences tests.
6. Complete History tests.
7. Perform required integration and final regression checks.
8. Update this audit with retest results and final Day-2 status.

---

## 12. QA Evidence

Backend commit:

```text
ff3d34c8d267a4a40ed70350b136edd4974ba622
```

Automated test command:

```text
npm test
```

Automated result:

```text
60 test files passed
540 tests passed
3 test files skipped
12 tests skipped
0 failures
```
