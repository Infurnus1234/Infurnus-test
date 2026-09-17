# DAY-4 QA AUDIT - Rides, Driver State, PostGIS, Live Location, Matching & Concurrency

## 1. Test Information

**Tester:** Manshi

**Day:** Day 4

**Date:** 17 September 2026

**Project:** INFURNUS

**Backend Repository:** Infurnus-new

**QA Repository:** Infurnus-test

**Branch:** manshi

---

## 2. Scope

Day 4 focused on:

- Ride APIs
- Ride lifecycle
- Driver availability and assignment state
- Live driver location
- PostGIS location handling
- Nearby driver discovery
- Driver matching
- Atomic driver acceptance and concurrency
- Vehicle assignment
- Ride integration and security
- Google Maps provider behavior

---

## 3. Automated Test Execution

**Command executed:**

```cmd
node --import=dotenv/config node_modules\vitest\vitest.mjs run src\modules\rides\tests
```

### Result

- Test files passed: **6**
- Test files skipped: **1**
- Tests passed: **36**
- Tests skipped: **1**
- Tests failed: **0**
- Total tests: **37**
- Duration: **2.72 seconds**

### Test Breakdown

| Test Suite | Passed | Failed | Skipped |
|---|---:|---:|---:|
| `ride.schemas.test.ts` | 5 | 0 | 0 |
| `route-recalculation.service.test.ts` | 4 | 0 | 0 |
| `driver.service.test.ts` | 8 | 0 | 0 |
| `matching.service.test.ts` | 5 | 0 | 0 |
| `google.maps.provider.test.ts` | 8 | 0 | 0 |
| `ride.database.integration.test.ts` | 6 | 0 | 0 |
| `ride.load.integration.test.ts` | 0 | 0 | 1 |
| **Total** | **36** | **0** | **1** |

### Overall Result

**36 passed / 0 failed / 1 skipped**

---

## 4. Ride API Verification

The following ride routes were reviewed and/or manually exercised:

- `POST /rides`
- `GET /rides`
- `GET /rides/:id`
- `POST /rides/:id/cancel`
- `POST /rides/:id/accept`
- `POST /rides/:id/complete`
- `POST /rides/:id/status`
- `PATCH /rides/driver/availability`
- `POST /rides/driver/location`

### Manual API Results

| Test | Result |
|---|---|
| Valid ride creation | **PASS** |
| Ride retrieval by owner | **PASS** |
| Ride listing | **PASS** |
| Ride cancellation | **PASS** |
| Cancellation persistence after retrieval | **PASS** |
| Unauthenticated ride creation | **PASS — HTTP 401** |
| Unauthenticated ride retrieval | **PASS — HTTP 401** |
| Customer attempting driver-only completion | **PASS — HTTP 403** |

---

## 5. Ride Lifecycle Verification

The database defines the following lifecycle:

```text
REQUESTED
    ↓
SEARCHING
    ↓
DRIVER_ASSIGNED
    ↓
DRIVER_ARRIVING
    ↓
DRIVER_ARRIVED
    ↓
IN_PROGRESS
    ↓
COMPLETED
```

Cancellation is supported from:

- `REQUESTED`
- `SEARCHING`
- `DRIVER_ASSIGNED`
- `DRIVER_ARRIVING`
- `DRIVER_ARRIVED`

Invalid lifecycle transitions are rejected by the database trigger and surfaced by the service as a conflict/business error.

### Database Integration Test

- Invalid lifecycle transition without modifying the row: **PASS**

### Result

**PASS**

---

## 6. Driver Availability

Implemented driver availability states found in the database:

- `available`
- `unavailable`
- `busy`
- `stale`

### Verified Behavior

| Scenario | Result |
|---|---|
| Driver can change availability appropriately | **PASS** |
| Driver-controlled `BUSY` state is rejected | **PASS** |
| Driver-controlled `STALE` state is rejected | **PASS** |
| Busy driver cannot incorrectly become unavailable | **PASS** |
| Disconnect marks driver stale | **PASS** |
| Active ride prevents stale recovery from incorrectly restoring availability | **PASS** |

### Observation

The Day 4 QA scope includes an `OFFLINE` driver state. The current database enum does not contain `OFFLINE`.

This is recorded as an implementation/scope gap and was not modified during QA.

---

## 7. Live Location and PostGIS

### Verified

| Scenario | Result |
|---|---|
| Current driver location update | **PASS** |
| Future-dated location rejection | **PASS** |
| Out-of-order location update rejection | **PASS** |
| Driver stale handling on disconnect | **PASS** |
| Freshness threshold passed to nearby-driver search | **PASS** |

### Database Verification

Database verification confirmed:

- Driver location uses PostGIS `geography`
- Location is stored as `Point`
- SRID is `4326`
- GiST spatial index exists for available drivers with location
- Location freshness is enforced during nearby-driver eligibility

### Configured Values Reviewed

| Configuration | Value |
|---|---:|
| `DRIVER_LOCATION_STALE_SECONDS` | 30 seconds |
| `DRIVER_SEARCH_RADIUS_METERS` | 5000 meters |
| `MAX_DRIVER_MATCH_CANDIDATES` | 20 |

### Result

**PASS**

---

## 8. Nearby Driver Discovery

The implementation filters candidates using:

- Active user
- Approved driver verification
- Available driver state
- Fresh driver location
- PostGIS distance filtering
- No active ride
- Active vehicle

Candidates are ordered using spatial distance when route-matrix information is not available.

### Result

**PASS**

Result based on repository implementation and matching/driver tests.

---

## 9. Matching Verification

### Matching Service Tests

**5/5 PASS**

The matching implementation uses:

- Driver availability
- Location freshness
- Eligibility
- Vehicle availability/suitability
- Route duration when Google Maps data is available
- Distance
- Driver profile ID as deterministic tie-breaker

The candidate set is bounded using the configured maximum candidate limit.

No matching test failures were observed.

### Result

**PASS**

---

## 10. Atomic Driver Acceptance — Critical Test

The PostgreSQL integration suite executed:

**100 concurrent acceptance attempts against the same ride.**

### Expected Result

- Exactly 1 successful assignment
- Remaining attempts rejected/conflicted
- No duplicate assignment
- Consistent final ride state

### Actual Result

| Result | Count |
|---|---:|
| Successful winners | **1** |
| Conflicts/failures | **99** |
| Duplicate assignment | **0** |

### Final Database State

- Final ride status: `driver_assigned`
- Final assigned driver: **Correct**
- Driver state: `busy`

### Result

**PASS**

The database uses transactional acceptance, and the ride update requires the ride to remain in `SEARCHING` state.

A partial unique index also prevents one driver from having multiple active rides.

This is a critical Day 4 concurrency requirement.

---

## 11. Vehicle Assignment

Ride acceptance assigns an active vehicle belonging to the accepted driver.

### Database Validation

- Assigned driver and vehicle must form a valid pair.
- Assigned vehicle must belong to the assigned driver.
- Assigned vehicle must be active.
- Vehicle is selected during acceptance.

### Result

**PASS**

---

## 12. Google Maps Provider

### Google Maps Provider Tests

**8/8 PASS**

### Verified Behavior

- No request when server-side key is unavailable
- API key not exposed in logs
- Route matrix request bounds
- Preservation of route matrix positions when an element fails
- Places input validation
- Places request throttling/de-duplication
- Retry of transient failures
- Safe fallback behavior
- No retry for permanent client failures

### Result

**PASS**

---

## 13. Security Verification

### Verified

| Security Check | Result |
|---|---|
| Authentication required for ride routes | **PASS** |
| Unauthenticated ride creation rejected | **PASS** |
| Unauthenticated ride retrieval rejected | **PASS** |
| Driver-specific routes require driver role | **PASS** |
| Customer cannot use driver-only completion endpoint | **PASS** |
| Customer ride ownership enforced during retrieval/listing | **PASS** |

### Driver API Limitation

A complete real authenticated-driver API flow could not be executed because available driver test records contained invalid phone values and no supported driver-profile creation endpoint was identified during QA.

No database modification was made to bypass this limitation.

**Status:** Blocked / Deferred

---

## 14. Load Test

`ride.load.integration.test.ts` contains one skipped test.

### Result

| Status | Count |
|---|---:|
| Passed | **0** |
| Failed | **0** |
| Skipped | **1** |

The skipped test is recorded as deferred rather than failed.

### Status

**Deferred**

---

## 15. Defects / Observations

### Observation 1 — OFFLINE Driver State

**Severity:** Medium

**Priority:** Medium

The Day 4 QA scope includes an `OFFLINE` driver state, but the current database enum contains only:

- `available`
- `unavailable`
- `busy`
- `stale`

The implementation currently represents disconnect behavior using `STALE`.

**Status:** Open / Scope clarification required

---

### Observation 2 — Authenticated Driver API Flow

**Severity:** Medium

**Priority:** Medium

Existing QA driver records contained invalid phone values that could not pass the application's phone validation.

The codebase did not expose a supported driver-profile creation API during this audit.

Therefore, a complete real authenticated driver API flow could not be executed without manually modifying database records.

No database modification was performed to bypass this limitation.

**Status:** Blocked / Deferred

---

### Observation 3 — Workload Ranking

The matching implementation excludes drivers with active rides and ranks available candidates using:

- Route duration
- Distance
- Driver profile ID

A separate workload-based ranking factor was not identified in the inspected matching implementation.

**Status:** Observation for product/requirements review

---

## 16. Regression Summary

### Day 4 Automated Ride Test Suite

**36 passed / 0 failed / 1 skipped**

### Regression Result

No automated regression failures were observed during the executed Day 4 ride test suite.

**Result: PASS**

---

## 17. Overall Day 4 Status

# PASS WITH DOCUMENTED OBSERVATIONS

### Final Summary

Core ride, lifecycle, PostGIS, driver eligibility, matching, vehicle assignment, Google Maps resilience, and critical concurrency behavior passed.

The following remain documented:

1. `OFFLINE` state is not represented in the current driver availability enum.
2. Full authenticated driver API testing is deferred due to unavailable valid driver authentication/provisioning through the supported application flow.
3. One load test is skipped.
4. Workload-based matching was not identified as an explicit ranking factor.

### QA Safety and Environment

- No production credentials were used.
- No production data was used.
- No manual database modifications were performed for QA.
- QA was executed against the local QA environment.

### Final QA Status

**PASS WITH DOCUMENTED OBSERVATIONS**

---

## 18. Evidence

### Primary Automated Evidence

- `src/modules/rides/tests/ride.schemas.test.ts`
- `src/modules/rides/tests/route-recalculation.service.test.ts`
- `src/modules/rides/tests/driver.service.test.ts`
- `src/modules/rides/tests/matching.service.test.ts`
- `src/modules/rides/tests/google.maps.provider.test.ts`
- `src/modules/rides/tests/ride.database.integration.test.ts`
- `src/modules/rides/tests/ride.load.integration.test.ts`

### Critical Concurrency Evidence

- **100** concurrent acceptance attempts
- **Exactly 1** successful assignment
- **99** conflicts

### PostGIS Evidence

- Driver location stored using PostGIS `geography`
- Point geometry with SRID `4326`
- GiST spatial index verified
- Location freshness validation verified

### Google Maps Evidence

- **8/8 tests passed**
- Retry and fallback behavior verified
- API key protection verified

### Final Automated Result

**36 passed, 0 failed, 1 skipped**
