# DAY 12 – QA TESTING REPORT

## INFURNUS DRIVER APPLICATION
### Application Testing & Feature Verification

**Testing Type:** Functional Testing + UI/UX Testing + Navigation Testing  
**Platform:** Android  
**Device:** Samsung SM-A336E  
**Tester:** Manshi  
**Testing Day:** Day 12

---

# 1. Objective

The objective of this Day 12 activity was to verify whether the **Infurnus Driver** application could be successfully set up, built, installed, launched, and tested on a physical Android device.

The testing focused on:

- Application setup and build verification
- APK generation
- APK installation
- Application launch
- Splash screen
- Login and authentication screen
- Dashboard
- Menu and navigation
- Notifications
- Fleet Mode
- Online/Offline status
- Assigned Vehicle
- Vehicle Compliance Documents
- Ride & Booking Requests
- Ride Details
- Call Customer
- Delivery Status
- Back navigation
- Page and section visibility
- UI/UX readability
- Identification and documentation of bugs
- Suggestions for improvements

---

# 2. Scope of Testing

The following application areas were included in testing:

1. Application installation and launch
2. Splash screen and initial screen loading
3. Login screen
4. Dashboard
5. Menu
6. Notifications
7. Driver Mode / Fleet Owner Mode
8. Online / Offline status
9. Assigned Vehicle
10. Vehicle Details
11. Vehicle Compliance Documents
12. Ride & Booking Requests
13. Ride Details
14. Call Customer
15. Delivery Status
16. Back navigation
17. Page/section accessibility
18. UI/UX readability and text contrast

---

# 3. Test Environment

| Item | Details |
|---|---|
| Application | Infurnus Driver |
| Platform | Android |
| Device | Samsung SM-A336E |
| Device ID | RZCTB17K92N |
| Flutter | 3.47.4 |
| Java | JDK 17 |
| Gradle | 8.14 |
| Android Gradle Plugin | 8.11.1 |
| Kotlin | 2.2.20 |
| Build Type | Debug |
| Installation Method | ADB |
| Testing Type | Functional + UI/UX + Navigation |

---

# 4. Application Setup and Build Investigation

The **Infurnus Driver** repository was cloned successfully.

The project was identified as a Flutter application because the repository contains files and folders such as:

- `pubspec.yaml`
- `lib`
- `android`
- `ios`
- `web`
- `windows`
- Other Flutter project directories

During the initial build, Android/Flutter compatibility issues were identified.

## 4.1 Initial Build Issues

### Issue 1 – Gradle Version

The project initially used:

```text
Gradle 8.5.0
```

The installed Flutter version required a newer compatible Gradle version.

### Fix

Gradle was updated from:

```text
8.5.0
```

to:

```text
8.14
```

---

### Issue 2 – Android Gradle Plugin Version

The project initially used:

```text
Android Gradle Plugin 8.3.2
```

The installed Flutter version required a newer AGP version.

### Fix

The Android Gradle Plugin was updated from:

```text
8.3.2
```

to:

```text
8.11.1
```

---

### Issue 3 – Kotlin Version

The project initially used:

```text
Kotlin 2.0.21
```

Flutter required a newer compatible Kotlin version.

### Fix

Kotlin was updated from:

```text
2.0.21
```

to:

```text
2.2.20
```

---

## 4.2 Build Configuration Summary

| Component | Initial Version | Updated Version | Result |
|---|---:|---:|---|
| Gradle | 8.5.0 | 8.14 | Compatibility issue resolved |
| Android Gradle Plugin | 8.3.2 | 8.11.1 | Compatibility issue resolved |
| Kotlin | 2.0.21 | 2.2.20 | Compatibility issue resolved |

---

## 4.3 APK Generation

After updating the required Android build versions, the project was compiled directly using Gradle.

The Gradle build completed successfully with:

```text
BUILD SUCCESSFUL
```

The debug APK was generated at:

```text
android\app\build\outputs\flutter-apk\app-debug.apk
```

The APK was also available under:

```text
android\app\build\outputs\apk\debug\app-debug.apk
```

---

## 4.4 Flutter CLI APK Detection Issue

Although the APK was generated successfully, the Flutter CLI reported that it could not find the APK in the expected output location while using `flutter run`.

The generated APK was manually verified and then installed directly using ADB.

This allowed the application to be tested successfully on the physical Android device.

---

# 5. APK Installation and Launch Verification

The debug APK was installed using ADB.

The installation returned:

```text
Success
```

The application package was verified using ADB, and the application's `MainActivity` was launched successfully.

## Test Results

| Test | Expected Result | Actual Result | Status |
|---|---|---|---|
| APK generation | Debug APK should be generated | APK generated | PASS |
| APK installation | APK should install | Installation successful | PASS |
| Package verification | Package should be registered | Package verified | PASS |
| MainActivity launch | MainActivity should launch | MainActivity launched | PASS |
| Application launch | Application should open | Application opened | PASS |

---

# 6. Application Launch Testing

The application was launched on the Samsung Android device.

## Test Cases

| Test Case | Expected Result | Actual Result | Status |
|---|---|---|---|
| Launch application | Application opens | Application opened successfully | PASS |
| Splash screen | Splash screen displays | Splash screen displayed | PASS |
| Login/Signup screen | Authentication screen loads | Screen loaded | PASS |
| Crash check | Application should not crash | No crash observed | PASS |
| Connection check | No server/connection error | No connection error observed | PASS |
| UI loading | UI should load correctly | UI loaded correctly | PASS |

---

# 7. Login and Dashboard Testing

The login screen was opened successfully.

The following controls were visible:

- Mobile Number or Email
- Password / OTP
- Log In with Password
- Send OTP Code
- Register Now

The login process was tested and the application successfully reached the Driver Dashboard.

## Test Results

| Feature | Status |
|---|---|
| Login screen loaded | PASS |
| Mobile/Email field visible | PASS |
| Password/OTP field visible | PASS |
| Log In with Password | PASS |
| Send OTP Code option visible | PASS |
| Register Now option visible | PASS |
| Successful login | PASS |
| Driver Dashboard loaded | PASS |

---

# 8. Dashboard Testing

After successful login, the Driver Dashboard was displayed.

The dashboard was checked for:

- Screen loading
- Main information
- Driver status
- Assigned vehicle
- Ride/booking information
- Navigation options
- Mode switching
- Online/Offline status

The dashboard loaded successfully without an application crash.

---

# 9. Menu Testing

The application menu was tested systematically.

## Test Results

| Check | Status | Observation |
|---|---|---|
| Menu opens | PASS | Menu opened successfully |
| Menu options visible | PASS | Options were visible |
| Overlap/cut-off | PASS | No major overlap/cut-off observed |
| Options clickable | PASS | Menu options responded |
| Close/back | PASS | Menu could be closed/navigated back |

The menu functionality was working during testing.

---

# 10. Notifications Testing

The Notifications screen was opened successfully.

Notifications such as the following were displayed:

- Driver Application Approved
- New Logistics Booking Received
- Vehicle Verification Successful

## Test Results

| Check | Status | Observation |
|---|---|---|
| Notifications screen opens | PASS | Screen opened |
| Notification cards visible | PASS | Cards displayed |
| Notification information visible | PASS | Content displayed |
| Navigation | PASS | Navigation available |
| Text readability | UI ISSUE | Some notification text has low contrast |

## UI Issue

Some notification descriptions appeared very light against the light background.

### Expected

Notification text should have sufficient contrast so that it is easily readable.

### Actual

Some text appeared faded/light.

### Suggested Change

Increase the text contrast and use a more readable text color.

**Priority:** Low

---

# 11. Fleet Mode Testing

The `Switch to Fleet Mode` option was tested.

The application successfully switched to Fleet Owner Mode.

The screen displayed:

```text
ACTIVE: FLEET OWNER MODE
```

The option changed to:

```text
Switch to Driver Mode
```

## Test Results

| Check | Status | Observation |
|---|---|---|
| Switch to Fleet Mode | PASS | Mode changed successfully |
| Fleet Owner Mode indicator | PASS | ACTIVE: FLEET OWNER MODE displayed |
| Switch to Driver Mode option | PASS | Option became available |
| Fleet Mode UI | PASS | Interface loaded |
| Online & Ready status | PASS | Status displayed |

## UI Observation

After switching to Fleet Owner Mode, the header continued to display:

```text
Driver Dashboard
```

This should be verified with the expected product/design behavior.

If Fleet Mode is intended to have a separate dashboard title, the heading should be updated accordingly.

**Priority:** Low / Design Verification

---

# 12. Online/Offline Toggle Testing

The Online/Offline status was tested by switching the driver between the two states.

## Test Results

| Test | Expected | Actual | Status |
|---|---|---|---|
| Online → Offline | Status changes to Offline | Changed to OFFLINE | PASS |
| Offline → Online | Status returns to Online & Ready | Returned to ONLINE & READY | PASS |
| Visual toggle | Switch state changes | Switch changed | PASS |
| Status message | Message reflects state | Message changed | PASS |
| Crash/error | No crash | No crash observed | PASS |

When Offline, the application displayed:

```text
Toggle ON to start accepting rides
```

The application successfully returned to:

```text
ONLINE & READY
```

---

# 13. Assigned Vehicle Testing

The Assigned Vehicle section was opened successfully.

The dashboard displayed the assigned vehicle:

```text
Tata Ace EV (KA 01 EV 8899)
```

The vehicle details screen displayed information including:

- Vehicle registration
- Vehicle status
- Vehicle category
- Vehicle Compliance Documents

## Test Results

| Check | Status | Observation |
|---|---|---|
| Vehicle details screen opens | PASS | Vehicle Details screen opened |
| Vehicle registration | PASS | Visible |
| Vehicle status | PASS | Visible |
| Vehicle category | PASS | Visible |
| Compliance section | PASS/UI ISSUE | Section visible but readability needs improvement |
| Back button | PASS | Back control visible |

---

# 14. Vehicle Compliance Documents

## Location

```text
Driver Dashboard
→ Assigned Vehicle
→ Details
→ Vehicle Compliance Documents
```

The compliance document section was visible.

The document cards displayed approval information such as:

```text
Approved
```

However, the document cards did not open when tapped.

## Functional Bug

### Expected Result

When a user taps a compliance document, the application should:

- Open the document
- Show a document preview
- Or display document details

### Actual Result

When the compliance document card was tapped, nothing happened.

### Status

**FAIL – Functional Bug Confirmed**

### Priority

**High**

This is one of the main functional issues identified during Day 12 testing.

---

## 14.1 Compliance Document UI Issues

Additional UI issues were observed:

### Issue 1 – Heading Contrast

The heading:

```text
Vehicle Compliance Documents
```

has low contrast against the background.

### Issue 2 – Document Labels

The document cards did not clearly show the document names/labels.

The cards mainly showed an icon and approval status.

### Suggested Improvement

Each document card should clearly display:

- Document name
- Document type
- Status
- Action/open option

---

# 15. Ride & Booking Requests Testing

The `See All` option was tested.

It successfully opened:

```text
Ride & Booking Requests
```

Booking cards displayed information including:

- Amount
- Pickup
- Drop-off
- Goods
- Status
- View Details

## Test Results

| Check | Status | Observation |
|---|---|---|
| See All opens | PASS | Ride & Booking Requests screen opened |
| Booking cards | PASS | Cards displayed |
| Amount | PASS | Amount displayed |
| Pickup | PASS/UI ISSUE | Visible but low contrast |
| Drop-off | PASS/UI ISSUE | Visible but low contrast |
| Goods information | PASS | Displayed |
| Booking status | PASS | Displayed |
| View Details | PASS | Button opened ride details |

---

# 16. Ride Details Testing

The first booking opened:

```text
Ride Details #BK-8001
```

The Ride Details screen displayed:

- Distance
- Route
- Customer phone/action
- Goods
- Weight
- Update Delivery Status

## Test Results

| Feature | Status | Observation |
|---|---|---|
| Ride Details screen | PASS | Opened successfully |
| Distance | PASS | Displayed |
| Route | PASS/UI ISSUE | Displayed but low contrast |
| Customer contact/action | PASS | Available |
| Goods and weight | PASS | Displayed |
| Delivery status actions | PASS | Actions available |

---

# 17. Ride Details UI Issues

The following readability issues were observed:

## 17.1 Route Text

The route text was very light and had low contrast.

### Suggested Improvement

Increase text contrast and make route information easier to read.

---

## 17.2 Customer Phone Number

The customer phone number appeared with low contrast.

### Suggested Improvement

Use a more readable text color and ensure the number is clearly visible.

---

## 17.3 Update Delivery Status Heading

The heading:

```text
Update Delivery Status
```

also appeared with low contrast.

### Suggested Improvement

Increase heading contrast and maintain consistent typography.

---

# 18. Call Customer Testing

The `Call Customer` action was tested.

The application displayed a calling confirmation message:

```text
Calling customer +91 91234 56789
```

## Test Results

| Check | Status |
|---|---|
| Button clickable | PASS |
| Calling action triggered | PASS |
| Confirmation feedback | PASS |
| Application crash | PASS – No crash |

The application successfully provided feedback after the call action was triggered.

---

# 19. Delivery Status Testing

The available delivery status actions were tested.

## 19.1 Mark as In Transit

### Action

```text
Mark as "In Transit"
```

### Result

The application displayed:

```text
Updated trip status to "In Transit"
```

**Status: PASS**

---

## 19.2 Mark as Goods Picked Up

### Action

```text
Mark as "Goods Picked Up"
```

### Result

The application displayed:

```text
Updated trip status to "Goods Picked Up"
```

**Status: PASS**

---

## 19.3 Mark as Delivered

### Action

```text
Mark as "Delivered"
```

### Result

The application displayed:

```text
Updated trip status to "Delivered"
```

**Status: PASS**

---

# 20. Navigation Testing

Navigation was tested across the following areas:

- Dashboard
- Menu
- Notifications
- Fleet Mode
- Assigned Vehicle
- Ride & Booking Requests
- Ride Details

Most tested navigation paths opened the intended screens.

However, a back-navigation issue was identified in the Ride Details flow.

---

# 21. Back Navigation Bug

## Location

```text
Ride & Booking Requests
→ View Details
→ Ride Details
→ Back
```

## Expected Result

When the user taps the top-left back arrow, the application should return to:

```text
Ride & Booking Requests
```

or the immediate previous screen.

## Actual Result

During testing, tapping the back arrow did not return to the expected previous screen.

The same Ride Details screen remained visible.

## Status

**FAIL – Navigation Issue**

## Priority

**Medium**

## Suggested Fix

Check the Flutter navigation stack and the implementation of the Ride Details back button.

The application should correctly pop the current screen and return to the previous route.

---

# 22. Page/Section Visibility Issue

During testing, some pages or sections were observed to be not fully visible or accessible as expected.

## Required Review

The development team should verify:

1. Every intended page is reachable.
2. Every required section is visible.
3. Screen content is not clipped.
4. No important controls are hidden.
5. Navigation routes correctly lead to the intended pages.
6. Users can access all required application features.

## Status

**NEEDS REVIEW**

## Priority

**Medium**

---

# 23. Consolidated Issues Found

| # | Issue | Location | Type | Priority |
|---:|---|---|---|---|
| 1 | Low notification text contrast | Notifications | UI/UX | Low |
| 2 | Compliance heading has low contrast | Assigned Vehicle Details | UI/UX | Low |
| 3 | Document labels are not clearly visible | Vehicle Compliance Documents | UI/UX | Medium |
| 4 | Compliance documents do not open when tapped | Vehicle Details → Documents | Functional | High |
| 5 | Pickup/Drop-off text has low contrast | Booking Requests | UI/UX | Low |
| 6 | Route text has low contrast | Ride Details | UI/UX | Low |
| 7 | Customer phone text has low contrast | Ride Details | UI/UX | Low |
| 8 | Update Delivery Status heading has low contrast | Ride Details | UI/UX | Low |
| 9 | Back navigation does not return to expected previous screen | Ride Details | Navigation | Medium |
| 10 | Some pages/sections are not fully visible or accessible | Application Navigation | Accessibility/Navigation | Medium |
| 11 | Fleet Mode header still displays Driver Dashboard | Fleet Mode | UI/UX / Design Verification | Low |

---

# 24. Detailed Bug Summary

## Bug 1 – Vehicle Compliance Documents Not Opening

**Severity/Priority:** High

**Location:**

```text
Driver Dashboard
→ Assigned Vehicle
→ Details
→ Vehicle Compliance Documents
```

**Expected:** Document should open or show details.

**Actual:** Tapping the document card produces no visible action.

**Required Change:** Implement/fix document-card navigation or document preview functionality.

---

## Bug 2 – Ride Details Back Navigation

**Severity/Priority:** Medium

**Location:**

```text
Ride & Booking Requests
→ View Details
→ Ride Details
→ Back
```

**Expected:** Return to Ride & Booking Requests.

**Actual:** Same Ride Details screen remained during testing.

**Required Change:** Verify the Flutter navigation stack and back-button route handling.

---

## Bug 3 – Low Text Contrast

**Severity/Priority:** Low

**Affected Areas:**

- Notifications
- Assigned Vehicle Details
- Booking Requests
- Ride Details

**Expected:** Text should be clearly readable.

**Actual:** Several headings and information fields appeared very light.

**Required Change:** Improve text/background contrast.

---

## Bug 4 – Compliance Document Labels

**Severity/Priority:** Medium

**Location:**

```text
Assigned Vehicle
→ Vehicle Details
→ Vehicle Compliance Documents
```

**Expected:** Document cards should clearly identify each document.

**Actual:** Document names/labels were not clearly visible.

**Required Change:** Add clear document names and types.

---

## Bug 5 – Page/Section Visibility

**Severity/Priority:** Medium

**Location:** Application navigation

**Expected:** All intended pages and sections should be visible and reachable.

**Actual:** Some pages/sections were reported as not fully visible or accessible during testing.

**Required Change:** Review navigation and screen accessibility.

---

# 25. Suggested Improvements

The following improvements are recommended based on the observed testing results:

1. Increase text contrast for notification descriptions.
2. Improve contrast of headings and labels in Vehicle Details.
3. Improve contrast of Pickup and Drop-off information.
4. Improve contrast of Route information.
5. Improve contrast of Customer Phone information.
6. Improve contrast of the Update Delivery Status heading.
7. Display clear document names on compliance document cards.
8. Make compliance document cards open the corresponding document, preview, or details.
9. Investigate and fix Ride Details back navigation.
10. Review application navigation to ensure all required pages are accessible.
11. Verify that no important screen content is clipped or hidden.
12. Verify whether Fleet Owner Mode should have a different dashboard title.
13. Perform regression testing after fixes.
14. Repeat all affected test cases after fixes.
15. Record screenshots and videos for confirmed bugs.
16. Maintain a bug-tracking list with status such as Open, Fixed, Retested, and Closed.

---

# 26. Evidence / Proof

Screenshots and screen recordings should be maintained for the following test areas:

- Application launch
- Splash screen
- Login screen
- Dashboard
- Menu
- Notifications
- Fleet Mode
- Online/Offline toggle
- Assigned Vehicle
- Vehicle Details
- Vehicle Compliance Documents
- Compliance document non-opening behavior
- Ride & Booking Requests
- Ride Details
- Call Customer
- In Transit status update
- Goods Picked Up status update
- Delivered status update
- Back navigation issue
- Page/section visibility issue

---

# 27. Testing Summary

The **Infurnus Driver** application was successfully:

- Cloned
- Configured
- Built
- Installed on a physical Android device
- Launched successfully

The direct Gradle build completed successfully, and the generated debug APK was installed using ADB.

The application opened successfully without a crash during the tested flows.

The following major areas were tested:

- Login
- Dashboard
- Menu
- Notifications
- Fleet Mode
- Online/Offline status
- Assigned Vehicle
- Vehicle Details
- Vehicle Compliance Documents
- Ride & Booking Requests
- Ride Details
- Customer calling action
- Delivery status updates
- Navigation
- Page/section visibility
- UI/UX readability

Several features worked successfully. At the same time, functional and UI/UX issues were identified.

The most significant functional issue observed was:

> **Vehicle Compliance Documents do not open when tapped.**

Another important issue was:

> **Back navigation from Ride Details did not return to the expected previous screen during testing.**

Multiple screens also contained low-contrast text, which can affect readability.

Some pages/sections were reported as not fully visible or accessible and require further navigation review.

---

# 28. Recommended Retesting Process

After the development team fixes the identified issues, the following retesting sequence should be followed:

### Step 1 – Build

Generate a new APK from the updated source code.

### Step 2 – Install

Install the new APK on the Android test device.

### Step 3 – Launch

Verify that the application opens without a crash.

### Step 4 – Login

Verify login functionality.

### Step 5 – Assigned Vehicle

Navigate to:

```text
Dashboard
→ Assigned Vehicle
→ Details
→ Vehicle Compliance Documents
```

Tap every document and verify that it opens correctly.

### Step 6 – Ride Details

Navigate to:

```text
Ride & Booking Requests
→ View Details
```

Tap the back arrow and verify that it returns to the previous screen.

### Step 7 – UI Verification

Verify that:

- Notification text is readable.
- Vehicle compliance headings are readable.
- Document labels are visible.
- Pickup/Drop-off text is readable.
- Route text is readable.
- Customer phone number is readable.
- Delivery status heading is readable.

### Step 8 – Navigation

Verify that every required page and section is reachable.

### Step 9 – Regression Testing

Repeat the previously working features to make sure the fixes have not introduced new problems.

### Step 10 – Evidence

Capture screenshots/screen recordings for:

- Fixed bugs
- Remaining bugs
- Successful retests

---

# 29. QA Tracking / Sign-off

| Field | Value |
|---|---|
| Testing Day | Day 12 |
| Application | Infurnus Driver |
| Tester | Manshi |
| Device | Samsung SM-A336E |
| Build Type | Debug |
| Testing Status | Testing completed for the listed scope |
| Known Issues | Document opening, back navigation, UI contrast, page visibility |
| Retesting Required | Yes, after fixes |

---

# 30. Final Conclusion

The Infurnus Driver application was successfully made available for physical-device testing after resolving the Android build compatibility issues.

The application launched successfully and the major driver workflows included in the testing scope were exercised.

The testing identified both functional and UI/UX issues. The highest-priority issue found during the documented testing was the **non-opening Vehicle Compliance Documents**. The **Ride Details back-navigation issue** also requires correction.

The remaining issues mainly concern text readability, document labeling, page/section accessibility, and UI consistency.

All identified issues should be fixed, followed by a complete regression and retesting cycle before final QA closure.
