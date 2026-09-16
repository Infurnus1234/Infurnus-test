# DAY 3 QA AUDIT — Vehicles, KYC/Documents & Rentals

## 1. Date and Tester
**Date:** 16 September 2026  
**Tester:** Manshi  
**Role:** QA Engineer

## 2. Environment and Backend Commit Tested
- **Environment:** Local QA
- **Backend Repository:** Infurnus-new
- **QA Repository:** Infurnus-test
- **Branch:** manshi
- **Backend Commit:** `ff3d34c8d267a4a40ed70350b136edd4974ba622`
- **Stack:** Node.js, PostgreSQL/PostGIS, Docker, Redis, Vitest, Supertest

## 3. Scope and APIs Covered
### Vehicles
- Create, Get, List, Update, Deactivate/retirement
- Driver-profile association
- Validation, active plate uniqueness
- One active vehicle per driver

### KYC/Documents
- Create, Get/List, Update
- Supported status transitions
- Partner and vehicle association
- Validation, ownership/isolation
- Sensitive KYC-data protection

### Partner → Vehicle → Documents
- Partner → Driver Profile → Vehicle → Vehicle Document
- Vehicle ownership validation
- Invalid partner/vehicle relationships
- Cross-partner isolation

### Rentals
- Create, Get, List, Cancel, Status/lifecycle
- Validation, ownership
- Overlap protection
- Idempotency
- Concurrent booking protection

## 4. Test Case Execution Summary

### Vehicles
| Suite | Passed | Failed | Skipped |
|---|---:|---:|---:|
| Vehicle routes | 6 | 0 | 0 |
| Vehicle schemas | 2 | 0 | 0 |
| Vehicle repository | 1 | 0 | 0 |
| **Total** | **9** | **0** | **0** |

**Result: 9/9 passed**

### KYC/Documents
| Suite | Passed | Failed | Skipped |
|---|---:|---:|---:|
| Partner document lifecycle | 25 | 0 | 0 |
| Partner document routes matrix | 14 | 0 | 0 |
| Partner document schema matrix | 42 | 0 | 0 |
| Partner document repository | 1 | 0 | 0 |
| Partner document service | 5 | 0 | 0 |
| Partner document schemas | 1 | 0 | 0 |
| **Total** | **88** | **0** | **0** |

**Result: 88/88 passed**

### Rentals
| Suite | Passed | Failed | Skipped |
|---|---:|---:|---:|
| Rental routes | 7 | 0 | 0 |
| Rental schemas | 7 | 0 | 0 |
| Rental service | 6 | 0 | 0 |
| Rental PostgreSQL integration | 5 | 0 | 0 |
| **Total** | **25** | **0** | **0** |

**Result: 25/25 passed**

### Overall
| Area | Passed | Failed |
|---|---:|---:|
| Vehicles | 9 | 0 |
| KYC/Documents | 88 | 0 |
| Rentals | 25 | 0 |
| **Total** | **122** | **0** |

**122/122 executed tests passed.**

## 5. Postman Execution Summary
Postman was **not executed** during this QA cycle.

API verification was performed using automated Vitest/Supertest route and service tests.

**Status: Deferred — no Postman execution evidence/count recorded.**

## 6. Database Verification Summary

### Vehicles
Direct PostgreSQL verification confirmed:
- `vehicles_pkey`
- `vehicles_driver_profile_id_fkey`
- `vehicles_active_retirement_ck`
- `vehicles_driver_profile_id_idx`
- `vehicles_one_active_per_driver_uidx`
- `vehicles_plate_number_active_uidx`
- `vehicles_pkey`

Verified:
- One active vehicle per driver
- Active plate uniqueness
- Retired vehicle cannot remain active
- Driver profile foreign-key protection

**Result: PASS**

### Rentals
Database verification confirmed:
- Primary key
- User/vehicle foreign keys
- Rental status and period
- Timestamps
- Idempotency key
- Relevant indexes
- State consistency
- Vehicle-period overlap protection

Verified overlap protection:
`rentals_no_vehicle_overlap`

**Result: PASS**

## 7. Integration Summary
### Partner → Vehicle → Vehicle Document
Verified:
- Vehicle association through `driverProfileId`
- Vehicle-document association through `vehicleId`
- `VEHICLE_RC` association
- Supported vehicle-document types
- Partner ownership validation
- Invalid partner/vehicle relationship rejection
- Cross-partner isolation

**Result: PASS**

### Rentals
Verified:
- Rental creation
- Overlap prevention
- Adjacent rental periods
- Idempotent retry
- Idempotency-key conflict
- Concurrent booking protection

**Result: PASS**

## 8. Negative / Edge-Case Summary
### Vehicles
- Invalid vehicle UUID
- Invalid driver-profile UUID
- Missing/nonexistent driver profile
- Empty update
- Unexpected/internal fields
- Duplicate active plate
- Missing vehicle

**Result: PASS**

### KYC/Documents
- Invalid UUIDs
- Unsupported document types/statuses
- Invalid status transitions
- Invalid dates
- Invalid document combinations
- Duplicate documents
- Unexpected fields
- Invalid vehicle association
- Unauthorized/non-owner access

**Result: PASS**

### Rentals
- Invalid input
- Invalid dates
- Overlapping periods
- Adjacent periods
- Invalid/nonexistent resources
- Unauthorized access
- Invalid lifecycle transitions
- Repeated cancellation scenarios
- Idempotency conflicts
- Concurrent booking attempts

**Result: PASS**

## 9. Security Summary
### Vehicles
- Driver-profile validation
- Ownership relationship checks
- Unexpected/internal field rejection

### KYC/Documents
- Partner ownership
- Partner isolation
- Outsider access rejection
- Vehicle ownership validation
- Sensitive document-number protection
- Safe projections
- Aadhaar/PAN raw-number rejection

### Rentals
- Customer ownership isolation
- User-scoped rental access
- Authorization checks
- Protected state manipulation

**Result: PASS**

No passwords, OTPs, API keys or other secrets are included in this report.

## 10. Concurrency / Realtime Results

### Rental Concurrency — Critical
Scenario: **10 concurrent overlapping booking attempts for the same vehicle/time window.**

Expected:
- 1 successful booking
- 9 rejected/conflicted attempts

The PostgreSQL integration test
`allows exactly one winner across 10 concurrent overlapping bookings`
**PASSED**.

**Result: PASS**

### Vehicle Concurrency
No separate vehicle-specific simultaneous-operation API/test was identified in the current implementation.

Database-level active-vehicle and active-plate invariants were verified through unique partial indexes.

**Status: Deferred — separate concurrency test not executed.**

## 11. Bugs Discovered
**Confirmed bugs: 0**

No new confirmed functional defect was identified during this QA execution.

## 12. Fixes Retested and Results
No Day 3 production-code fix was required during this execution.

| Bug | Fix | Retest Result |
|---|---|---|
| None | N/A | N/A |

## 13. Regression Results
### Targeted Regression
Relevant Vehicle, KYC/Document and Rental suites were executed.

**Result: PASS**

### Full Regression
Full repository-wide regression was not used as the Day 3 scope evidence.

Previously established repository baseline:
- 60 test files passed
- 3 test files skipped
- 540 tests passed
- 12 tests skipped

Day 3 scope was validated through targeted execution of the relevant suites.

## 14. Blocked / Deferred Items
### Postman Execution
**Status:** Deferred

**Reason:** No Postman execution evidence/count was recorded during this QA cycle. Automated API tests were used instead.

### Vehicle Concurrency
**Status:** Deferred

**Reason:** No separate vehicle simultaneous-operation workflow/test was identified in the current implementation. Database uniqueness constraints were independently verified.

## 15. Carry-Forward Items
- Execute Postman collections if Postman-based evidence is required.
- Add a dedicated vehicle concurrency test if a simultaneous vehicle-operation workflow is exposed or required.
- Continue regression testing after backend changes.
- Re-run rental concurrency testing after changes to rental creation, availability or database constraints.
- Record and retest any future confirmed defects.

## 16. End-of-Day Status
# PASS WITH KNOWN ISSUES

### Final Summary
- Vehicles: **9/9 passed**
- KYC/Documents: **88/88 passed**
- Partner + Vehicle + Documents integration: **PASS**
- Rentals: **25/25 passed**
- Rental concurrency: **PASS**
- Total executed tests: **122**
- Failed tests: **0**
- Confirmed bugs: **0**
- Postman execution: **Deferred**
- Separate vehicle concurrency test: **Deferred**

**Final QA Status: PASS WITH KNOWN ISSUES**

## 17. QA Evidence Rules
- Only actual executed test counts are recorded.
- Skipped tests are not counted as passed.
- Database claims are based on direct PostgreSQL verification.
- Critical rental concurrency behavior was verified through the executed integration test.
- No secrets, OTPs, passwords or API keys are included.
- Backend commit evaluated: `ff3d34c8d267a4a40ed70350b136edd4974ba622`
