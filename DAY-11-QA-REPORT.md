# DAY-11 QA / DEVELOPMENT REPORT

## Date
27 September 2026

## Project
Infurnus

---

## Today's Work

### 1. Repository Synchronization

- Checked the Git repository status.
- Updated the local `main` branch with the latest changes from `origin/main`.
- Used Git stash to safely preserve local modifications before synchronization.
- Restored the local changes after the remote updates were pulled.
- Resolved the merge conflict in `frontend/android/gradle.properties`.

### 2. Android JDK Configuration

- Updated the Android Gradle configuration to use JDK 17.
- Configured the project to use `D:\Java\jdk-17`.
- Updated `frontend/android/gradle.properties`.

### 3. Flutter Backend API Configuration

- Updated the Flutter development configuration to connect the Android device/emulator with the Infurnus backend.
- Updated both `baseUrl` and `socketUrl`.
- Backend URL used: `http://10.242.241.253:3000`.
- Updated `frontend/lib/core/config/env_config.dart`.

### 4. Unwanted File Cleanup

The following temporary/generated files were identified and removed:
- `frontend/android/java_pid15036.hprof`
- `payments.zip`
- `set`
- Temporary `payments_check` directory

The `payments.zip` file was inspected before removal and contained a copy of the payments module.

### 5. Git Changes Review

- Reviewed the staged changes before committing.
- Unrelated `package-lock.json` changes were removed from the commit.
- Only the required configuration changes were retained.

### 6. Git Commit

The configuration changes were committed successfully.

**Commit ID:** `e2b132c`

**Commit message:** `fix Android JDK and API configuration`

Files committed:
- `frontend/android/gradle.properties`
- `frontend/lib/core/config/env_config.dart`

### 7. Manshi Branch

- Checked remote branches.
- Confirmed that `origin/manshi` exists.
- Created the local `manshi` branch and configured it to track `origin/manshi`.

Command used:

```bash
git checkout -b manshi --track origin/manshi
```

### 8. Cashfree Sandbox Testing

- Verified that Cashfree Sandbox configuration values were present.
- Confirmed the Sandbox environment and Sandbox API URL.
- Tested the Cashfree Sandbox Create Order API directly.
- The API returned an authentication error:

```text
HTTP STATUS = 401
```

Response:

```json
{
  "code": "request_failed",
  "type": "authentication_error",
  "message": "authentication Failed"
}
```

### 9. Cashfree Issue Identified

The authentication failure was reproduced directly against the Cashfree Sandbox API.

Therefore, the current issue is related to Cashfree Sandbox authentication/credentials rather than the Infurnus payment request structure.

Further verification of the Cashfree Sandbox Client ID and Secret is required before completing the Sandbox payment and webhook testing.

---

## Issues / Pending Work

### Cashfree Sandbox Authentication

**Status:** Pending

Cashfree Sandbox is currently returning HTTP 401 authentication failure.

Next steps:
- Verify the Sandbox credentials from the Cashfree dashboard.
- Re-test Create Order after valid credentials are confirmed.
- Continue with Cashfree payment and webhook testing.

---

## Conclusion

Today's work focused on synchronizing the Infurnus repository, resolving the Android JDK configuration conflict, updating the Flutter backend API configuration, cleaning temporary files, and committing the required changes. Cashfree Sandbox testing was also performed, and a 401 authentication issue was identified for further verification.
