# DAY 6 — Frontend QA & Functional Testing Report

**Project:** Infurnus  
**QA:** Manshi  
**Day:** 6  
**Date:** 22 September 2026  
**Platform:** Android Emulator  
**Device:** Pixel 6 API 36 / Android 16  
**Flutter:** 3.47.4  
**Dart:** 3.13.3  

---

## 1. Objective

Perform frontend environment verification, application launch validation, authentication testing, and prepare the Customer/Driver functional QA flow for the Infurnus Flutter application.

> **QA rule followed:** No frontend application code was modified as part of functional testing. Build/setup issues were treated separately from functional defects.

---

## 2. Environment & Setup

| Check | Status | Result |
|---|---|---|
| Flutter SDK | PASS | Flutter 3.47.4 verified |
| Dart SDK | PASS | Dart 3.13.3 verified |
| Android SDK | PASS | API 36 configured |
| Android Emulator | PASS | Pixel 6 API 36 available |
| Flutter devices | PASS | Emulator detected |
| Java/JDK | PASS | Temurin JDK 17 configured |
| Flutter dependencies | PASS | `flutter pub get` completed |
| APK build | PASS | Debug APK built successfully |
| APK installation | PASS | APK installed on emulator |
| Application launch | PASS | Infurnus launched successfully |

### Build issue encountered

The first Android builds failed during:

`google_maps_flutter_android:compileDebugKotlin`

The error involved Kotlin incremental caches and different Windows drive roots between the project (`D:`) and Pub cache (`C:`).

The generated build/cache directories were cleaned and Kotlin incremental compilation was disabled for the local QA build configuration. After this, the application built and launched successfully.

This was treated as a **local build/setup blocker**, not as a confirmed functional application defect.

---

## 3. Landing Screen Testing

### Observed

- Infurnus logo displayed.
- `Move • Rent • Deliver` tagline displayed.
- `Your City. Your Way.` displayed.
- Rides/Rentals/Logistics text displayed.
- `Get Started` button displayed.
- Login link displayed.
- `Get Started` navigation was tested successfully.

**Result: PASS**

---

## 4. Authentication Testing

### Email Authentication

The email authentication screen was reached successfully.

Verified:
- Email authentication tab displayed.
- Email input displayed.
- Password input displayed.
- Password visibility control displayed.
- `Send OTP` button displayed.
- OTP instruction text displayed.

### Issue Observed — Email OTP Timeout

When the OTP flow was attempted, the application displayed:

`Connection timed out. Please check your internet or firewall settings.`

**Module:** Authentication → Email OTP  
**Severity:** High / Blocker for the affected OTP flow  
**Status:** FAILED / BLOCKED

**Expected:**  
The application should send the OTP successfully or display a meaningful backend/API failure response.

**Actual:**  
The OTP request timed out and the user could not proceed through the email OTP flow.

**Important:**  
This should first be verified against backend/API connectivity before assigning the issue exclusively to the Flutter frontend.

---

## 5. Authentication Session Observation

Application logs showed:

- Authentication status check started.
- Token read timeout was observed.
- Token was `null`.
- User ID was `null`.
- Application entered unauthenticated state.

For a fresh emulator with no existing session, the absence of a token is expected. The token-read timeout should be investigated separately because it may affect startup/session handling.

---

## 6. Performance Observation

During application startup, Android/Flutter logs reported significant skipped frames, including:

- `Skipped 725 frames`
- A `Davey` frame duration of approximately 12.5 seconds.

**Status:** OBSERVATION

This should not be classified as a final performance defect based on logs alone. It requires user-visible verification, such as checking whether the UI freezes or becomes unresponsive during startup.

---

## 7. Notification Service Observation

The application log reported:

`Notification service init failed: TimeoutException after 0:00:05.000000`

**Status:** OBSERVATION / NEEDS VERIFICATION

Notification functionality was not yet fully tested, so this should be verified with an actual notification test before classifying it as a functional defect.

---

## 8. Customer/Passenger Flow

The following functional tests remain pending because authentication could not yet be completed:

- Customer home screen
- Pickup location
- Destination location
- Location permission
- Fare calculation
- Booking creation
- Driver matching
- Driver assignment
- Booking cancellation
- Ride status updates
- Ride start
- Ride tracking
- Ride completion
- Payment
- Booking history

**Status:** BLOCKED / PENDING AUTHENTICATION

---

## 9. Driver Flow

Pending:

- Driver login
- Driver dashboard
- Online/offline status
- Ride request
- Accept ride
- Reject ride
- Navigation
- Start ride
- Complete ride
- Earnings/history

**Status:** BLOCKED / PENDING AUTHENTICATION

---

## 10. Negative & Edge Case Testing

Pending:

- No internet
- API timeout
- API error
- Invalid location
- Empty fields
- Invalid inputs
- Repeated button taps
- Back navigation
- App restart
- Session expiry
- Network reconnect

Authentication timeout has already provided one real timeout scenario for further investigation.

---

## 11. Current Test Summary

| Category | Status |
|---|---|
| Environment setup | PASS |
| Android emulator | PASS |
| Application build | PASS |
| Application installation | PASS |
| Application launch | PASS |
| Landing screen | PASS |
| Email authentication UI | PASS |
| Email OTP request | FAIL/BLOCKED |
| Phone authentication | PENDING |
| Successful authentication | BLOCKED |
| Customer flow | BLOCKED |
| Driver flow | BLOCKED |
| Negative/edge testing | PENDING |
| Final regression | PENDING |

**Note:** A final numeric pass/fail percentage is intentionally not reported yet because the complete functional test suite has not been executed.

---

## 12. Issues / Observations

### Issue 1 — Email OTP request timeout

- **Module:** Authentication
- **Severity:** High / Blocker for email OTP
- **Status:** Failed/Blocked
- **Expected:** OTP should be sent or a backend/API error should be returned.
- **Actual:** Connection timeout message displayed.
- **Evidence:** Emulator screenshot captured during authentication testing.
- **Next check:** Verify backend availability, API base URL, network connectivity, and OTP service configuration.

### Observation 2 — Notification service initialization timeout

- **Module:** Notifications
- **Status:** Needs verification
- **Log:** Notification service initialization timed out after 5 seconds.

### Observation 3 — Heavy startup frame skipping

- **Module:** Application startup/performance
- **Status:** Needs verification
- **Evidence:** Large number of skipped frames and approximately 12.5-second frame duration reported in Android logs.

### Setup Issue — Initial Kotlin/Gradle cache failure

- **Module:** Android build environment
- **Status:** Resolved locally
- **Impact:** Initially prevented APK build.
- **Final result:** APK successfully built and installed after local build-cache/configuration remediation.

---

## 13. Day 6 Conclusion

The Flutter frontend environment was successfully configured and the Infurnus application was successfully built, installed, and launched on an Android 16/API 36 emulator.

Initial landing-screen testing passed. Authentication testing reached the email OTP stage, where an OTP request timeout blocked further authenticated testing.

Customer and Driver functional flows remain pending until a working authentication/QA account or test authentication path is available.

The observed notification timeout and startup frame skipping require additional verification before being classified as confirmed functional/performance defects.

---

## 14. Next Actions

1. Test the Phone authentication tab.
2. Verify empty/invalid phone input validation.
3. Verify OTP flow using approved QA/test credentials.
4. Verify backend/API connectivity for the email OTP timeout.
5. Continue Customer/Passenger booking lifecycle testing.
6. Continue Driver ride lifecycle testing.
7. Execute negative and edge-case tests.
8. Complete full regression.
9. Update final pass/fail/blocked counts.
10. Commit and push the completed Day 6 QA report.
