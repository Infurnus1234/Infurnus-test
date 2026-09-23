# DAY 8 — Backend Authentication Testing & Android Application Testing

**Project:** Infurnus  
**Testing Type:** Backend API Testing + Android Application Testing  
**Platform:** Android  
**Date:** 23 September 2026  
**Status:** In Progress / API Connectivity Issue Under Investigation

---

# 1. Objective

The objective of today's work was to:

1. Test the complete authentication flow from the backend.
2. Verify Forgot Password functionality.
3. Verify Reset Password functionality.
4. Verify Login functionality.
5. Verify Login OTP generation and verification.
6. Confirm that OTPs are generated and delivered successfully.
7. Remove the previous local application setup.
8. Clone the existing Infurnus repository from GitHub.
9. Set up the backend and database environment.
10. Set up the Flutter frontend.
11. Run the application on a physical Android device.
12. Verify the application's current functionality.
13. Identify any issues between the Android frontend and backend.
14. Document the current blockers and required next steps.

---

# 2. Backend Authentication Testing

## 2.1 Authentication Flow

The following complete authentication flow was tested:

```text
Forgot Password
       ↓
OTP Generation
       ↓
OTP Verification
       ↓
Reset Password
       ↓
Login
       ↓
Login OTP
       ↓
OTP Verification
```

The purpose of testing this flow was to verify that the backend authentication APIs work correctly when used sequentially.

---

# 3. Forgot Password Testing

## Test Objective

To verify that a user can initiate the Forgot Password process and receive an OTP successfully.

## Steps Performed

1. Opened the Forgot Password flow.
2. Entered the authorized test email address.
3. Submitted the Forgot Password request.
4. Verified the backend response.
5. Checked whether the OTP was generated.
6. Checked whether the OTP was received successfully.
7. Used the received OTP for the next step of the authentication flow.

## Result

**PASS**

The Forgot Password request was processed successfully and the OTP was received.

---

# 4. Reset Password Testing

## Test Objective

To verify that the password can be reset successfully using the OTP received through the Forgot Password flow.

## Steps Performed

1. Initiated the Forgot Password flow.
2. Received the OTP.
3. Entered the OTP.
4. Verified the OTP.
5. Entered the new password.
6. Submitted the password reset request.
7. Verified the backend response.
8. Confirmed that the password reset process completed successfully.

## Result

**PASS**

The Reset Password flow worked successfully during backend testing.

---

# 5. Login Testing

## Test Objective

To verify that the user can log in using the updated credentials after successfully resetting the password.

## Steps Performed

1. Used the credentials associated with the test email.
2. Entered the updated password.
3. Submitted the login request.
4. Verified the backend response.
5. Confirmed that the login process proceeded to OTP verification.

## Result

**PASS**

The login request was processed successfully.

---

# 6. Login OTP Testing

## Test Objective

To verify that an OTP is generated and delivered during the login authentication process.

## Steps Performed

1. Entered valid login credentials.
2. Submitted the login request.
3. Waited for the OTP.
4. Verified that the OTP was generated.
5. Confirmed that the OTP was received.
6. Entered the OTP.
7. Completed OTP verification.

## Result

**PASS**

The Login OTP was generated and received successfully.

---

# 7. Repeated OTP Verification

The authentication flow was tested repeatedly to verify OTP consistency.

The following sequence was performed multiple times:

```text
Forgot Password
       ↓
OTP Received
       ↓
Reset Password
       ↓
Login
       ↓
Login OTP Received
       ↓
OTP Verification
```

## Observation

The OTP was received successfully during the backend authentication tests.

No issue was observed with OTP generation or OTP delivery at the backend level during these tests.

---

# 8. Backend Testing Summary

| Functionality | Status | Observation |
|---|---|---|
| Forgot Password | PASS | OTP generated and received |
| Forgot Password OTP | PASS | OTP received successfully |
| Reset Password | PASS | Password reset completed |
| Login | PASS | Login request processed |
| Login OTP | PASS | OTP generated and received |
| OTP Verification | PASS | OTP verification completed |
| Complete Authentication Flow | PASS | Backend flow completed successfully |

---

# 9. Android Application Setup

After completing the backend authentication testing, the existing local application setup was removed and the project was cloned again from the existing repository.

## Repository

```text
Infurnus-new
```

The project was cloned from the existing Git repository rather than creating a new project.

---

# 10. Previous Local Application Removal

The previous local Infurnus application was completely removed from the system.

The purpose was to ensure that the application was being tested from a fresh clone of the existing repository.

After removing the previous setup, the repository was cloned again.

---

# 11. Git Repository Setup

The existing repository was cloned successfully.

The project was checked using Git to verify that the working tree was clean and that the project was using the existing `main` branch.

The project was not recreated from scratch.

---

# 12. Backend Environment Setup

After cloning the repository, the backend dependencies were installed.

## Dependency Installation

The following command was used:

```cmd
npm ci
```

The dependencies were installed successfully.

No unnecessary package upgrades were performed.

---

# 13. Environment Configuration

The `.env.example` file was used to create the local environment configuration.

The local `.env` file was created for development.

The backend was configured to use:

```text
Port: 3000
```

The local PostgreSQL database and Redis services were also configured.

---

# 14. Docker Setup

The Infurnus backend uses Docker services for the local development environment.

The following services were configured:

```text
PostgreSQL
Redis
```

Docker Compose was used to start the required services.

---

# 15. Database Migration Issue

During the initial database migration, an existing database state caused a migration conflict.

The conflict was related to an already-existing password reset table.

The issue was identified as stale local database state from the previous setup.

Since this was a fresh local setup and no data needed to be preserved, the local Docker volumes were reset.

The following command was used:

```cmd
docker compose down -v
```

The Docker services were then started again:

```cmd
docker compose up -d
```

---

# 16. Database Migration

After creating a fresh database environment, the database migrations were executed successfully.

The migration process completed through the latest migration.

The final result was:

```text
Database migrations completed successfully
```

This confirmed that the local database schema was successfully initialized.

---

# 17. Backend Server

The backend server was started successfully.

The backend reported:

```text
INFURNUS API listening on port 3000
```

The backend health endpoint was also tested.

The health check returned a successful response indicating that the server was running correctly.

---

# 18. Flutter Frontend Setup

The Flutter frontend was located inside:

```text
frontend
```

The Flutter environment was checked and the required dependencies were installed.

The following command was used:

```cmd
flutter pub get
```

The dependencies were successfully resolved and downloaded.

---

# 19. Android Device Setup

A physical Android device was connected to the development system using USB.

The device was detected successfully by Flutter.

The connected device appeared as:

```text
SM A336E
```

Device ID:

```text
RZCTB17K92N
```

The device was running:

```text
Android 16
```

---

# 20. Android Application Build

The application was initially affected by a Kotlin/Gradle incremental cache issue during the Android build.

The error was related to:

```text
google_maps_flutter_android
```

and Kotlin incremental caches.

The following cleanup actions were performed:

```cmd
flutter clean
```

Gradle daemons were also stopped:

```cmd
gradlew --stop
```

Kotlin incremental compilation was disabled in the Gradle configuration:

```properties
kotlin.incremental=false
```

After cleaning the Flutter and Gradle environment, the application was successfully built.

---

# 21. Android Application Launch

The Flutter application was successfully launched on the physical Android device using:

```cmd
flutter run -d RZCTB17K92N
```

The application opened successfully on the phone.

The initial login screen was visible and the application was able to start.

---

# 22. Android Application Connectivity Issue

Although the application successfully launched, an issue was identified when the application attempted to communicate with the backend.

The application displayed:

```text
Connection error.
The server might be unreachable.
```

This indicates that the application is currently unable to communicate with the locally running backend as expected.

---

# 23. ADB Reverse Port Forwarding

Since the backend is running locally on port `3000`, ADB reverse port forwarding was configured to allow the physical Android device to access the local backend.

The following command was used:

```cmd
"D:\Android\Sdk\platform-tools\adb.exe" -s RZCTB17K92N reverse tcp:3000 tcp:3000
```

The command returned:

```text
3000
```

This confirmed that the port forwarding was configured successfully for the physical Android device.

---

# 24. Current Android Testing Observation

The application itself is successfully building and launching on the physical Android device.

However, the API-dependent functionality is not currently progressing as expected.

The main observed issue is:

```text
Connection error.
The server might be unreachable.
```

Because of this connectivity issue, the following application-level authentication testing is currently blocked:

```text
Send OTP
     ↓
OTP Verification
     ↓
Authentication
```

---

# 25. Important Comparison: Backend vs Android

A difference was observed between backend testing and Android application testing.

### Backend

The following backend flows are working:

```text
Forgot Password
       ↓
OTP Received
       ↓
Reset Password
       ↓
Login
       ↓
Login OTP
       ↓
OTP Verification
```

The OTP was received successfully during backend testing.

### Android Application

The application successfully launches, but the API request from the Android application is currently showing a connectivity error.

Therefore, the issue appears to be related to the communication/configuration between the mobile frontend and the locally running backend rather than the basic OTP generation test performed through the backend.

Further investigation is required to determine the exact cause.

---

# 26. Current Status

| Area | Status |
|---|---|
| Repository Clone | PASS |
| Backend Dependencies | PASS |
| Docker Setup | PASS |
| PostgreSQL Setup | PASS |
| Redis Setup | PASS |
| Database Migration | PASS |
| Backend Server | PASS |
| Backend Health Check | PASS |
| Forgot Password API | PASS |
| Reset Password API | PASS |
| Login API | PASS |
| Login OTP | PASS |
| OTP Verification | PASS |
| Flutter Dependencies | PASS |
| Android Device Detection | PASS |
| Android Build | PASS |
| Android Application Launch | PASS |
| Android Backend Connectivity | BLOCKED |
| Android OTP Flow | BLOCKED |

---

# 27. Issues Identified

## Issue 1 — Android API Connectivity

### Description

The Flutter application launches successfully on the physical Android device, but API requests are showing:

```text
Connection error.
The server might be unreachable.
```

### Current Status

**OPEN / UNDER INVESTIGATION**

### Possible Areas to Check

- Android API base URL configuration
- Frontend environment configuration
- Localhost handling on a physical Android device
- Backend accessibility from the Android device
- Android network configuration
- Android network permissions
- API endpoint configuration
- HTTP/HTTPS configuration
- OTP API integration
- Backend-to-frontend request flow

---

# 28. Recommended Next Steps

The following tasks need to be completed:

1. Check the API base URL used by the Flutter application.
2. Verify whether the application is using `localhost` or another backend address.
3. Confirm that the Android application is pointing to the correct backend.
4. Verify that the backend is accessible from the physical Android device.
5. Check Android network configuration and permissions.
6. Test the authentication API directly from the Android application.
7. Verify the Forgot Password API from the mobile application.
8. Verify OTP generation from the mobile application.
9. Verify OTP verification from the mobile application.
10. Verify Reset Password from the mobile application.
11. Verify Login from the mobile application.
12. Retest the complete authentication flow after the connectivity issue is resolved.

---

# 29. Overall Conclusion

Today's testing covered both backend authentication and Android application setup.

The backend authentication flow was successfully tested, including:

```text
Forgot Password
      ↓
OTP
      ↓
Reset Password
      ↓
Login
      ↓
Login OTP
      ↓
OTP Verification
```

The OTP was successfully generated and received during backend testing.

The existing Infurnus application was also freshly cloned, configured, migrated, built, and successfully launched on a physical Android device.

The remaining blocker is the communication between the Android application and the locally running backend. The application currently displays a server connectivity error when attempting API-dependent operations.

Further investigation of the Android API base URL, network configuration, and frontend-to-backend integration is required before completing the Android authentication testing.

---

# 30. Final Status

**Backend Authentication Testing:** COMPLETED

**Android Application Setup:** COMPLETED

**Android Application Build & Launch:** COMPLETED

**Android API Connectivity Testing:** BLOCKED / UNDER INVESTIGATION

**Overall Day 8 Status:** IN PROGRESS
