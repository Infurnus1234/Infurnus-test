# DAY 9 & DAY 10 – QA BACKEND TESTING REPORT

## Project: INFURNUS

**Role:** QA Tester  
**Testing Type:** Backend API Testing / Authentication Testing  
**Testing Environment:** Local Development Environment  
**Backend:** Node.js / TypeScript  
**Database:** PostgreSQL + PostGIS  
**Cache:** Redis  
**Testing Days:** Day 9 & Day 10

---

# DAY 9 – Backend Setup, Troubleshooting & Authentication Testing

## 1. Objective

The main objective of Day 9 was to:

- Run the INFURNUS backend successfully.
- Verify that the backend APIs were working correctly.
- Test authentication-related APIs.
- Test login and OTP functionality using multiple email accounts.
- Verify Forgot Password and Reset Password functionality.
- Identify errors occurring during backend execution.
- Check whether previously reported authentication bugs were fixed.

---

## 2. Initial Backend Environment

The INFURNUS project was available locally at:

```text
D:\INFURNUS\Infurnus-new
```

The backend used:

- Node.js
- TypeScript
- PostgreSQL/PostGIS
- Redis
- Docker
- REST APIs
- OTP-based authentication

PostgreSQL was running inside Docker using:

```text
Container: infurnus-postgres
Image: postgis/postgis:17-3.5
Host Port: 5433
Container Port: 5432
```

Redis was running using:

```text
Container: infurnus-redis
Port: 6379
```

---

# 3. Initial Problem – Application Was Not Running

At the beginning of testing, the application/backend was not running successfully.

The backend was checked and the required services were verified.

Docker containers were checked using:

```powershell
docker ps
```

The PostgreSQL and Redis containers were found running.

However, the application could not initially be accessed properly from the testing environment.

---

# 4. Backend Connection Issue

One of the issues encountered during the earlier backend setup was a PostgreSQL connection problem.

The backend initially showed a connection error similar to:

```text
ECONNREFUSED 127.0.0.1:5433
```

This indicated that the backend was unable to establish a connection with PostgreSQL through the configured port.

The PostgreSQL Docker container was checked and the port mapping was verified:

```text
0.0.0.0:5433 -> 5432
```

After correcting the required configuration and ensuring PostgreSQL was running properly, the backend was able to connect to the database.

---

# 5. Backend Started Successfully

After resolving the database connection issue, the backend was started using the project development command.

The backend finally displayed:

```text
INFURNUS API listening on port 3000
```

The health endpoint was also checked:

```text
http://localhost:3000/health
```

The API returned a successful response, confirming that the backend was running correctly.

---

# 6. Network / IP Testing

During testing, the local machine IP address was also checked because the application/backend needed to be accessed from another device/environment.

The IPv4 address identified during testing was:

```text
10.2.20.208
```

The Windows firewall was also configured to allow incoming connections on port `3000`.

The following command was used:

```powershell
netsh advfirewall firewall add rule name="Infurnus API 3000" dir=in action=allow protocol=TCP localport=3000
```

The command returned:

```text
Ok.
```

This allowed the backend API running on port `3000` to receive the required network requests.

---

# 7. Authentication Testing

After the backend was successfully started, authentication APIs were tested.

The following flows were tested:

1. Signup
2. Signup OTP verification
3. Login
4. Login OTP verification
5. Forgot Password
6. Reset Password
7. Logout
8. Multiple login/logout attempts
9. Multiple Forgot Password attempts

The main purpose was to verify that OTP-based authentication worked correctly for multiple users.

---

# 8. Five Email Accounts Used for Testing

Backend authentication testing was performed using five test email accounts.

The accounts were used only for QA/testing purposes.

Testing was performed individually for each email account to verify that one user's authentication data did not interfere with another user's authentication flow.

For every test email, the following flows were checked where applicable:

```text
Signup
   ↓
Signup OTP
   ↓
Login
   ↓
Login OTP
   ↓
Logout
   ↓
Login Again
   ↓
Forgot Password
   ↓
Reset Password
   ↓
Login With New Password
```

---

# 9. Login Testing

Login API testing was performed using the test accounts.

The login flow was checked multiple times to verify:

- Correct email and password were accepted.
- Incorrect credentials were rejected.
- OTP was generated for valid login attempts.
- OTP verification worked.
- Login could be performed again after logout.
- Authentication tokens were generated correctly.

During testing, an issue was encountered where the application showed:

```text
Invalid email or password
```

This was investigated by checking the backend/database and the test credentials.

The issue was treated as an authentication/data validation issue rather than assuming that the frontend was the only cause.

---

# 10. OTP Testing

OTP functionality was tested for the authentication flows.

The main issue observed during testing was related to OTP reuse/OTP generation.

During repeated login attempts, an OTP-related problem was observed where the OTP flow did not behave correctly for another login attempt with the same email.

The issue was investigated by checking the backend authentication flow, database records and OTP-related data.

The testing confirmed that OTP handling needed to be checked carefully for:

- Multiple login attempts.
- OTP expiration.
- OTP verification.
- Repeated requests.
- Re-login after logout.
- Different test accounts.

---

# 11. Forgot Password Testing

Forgot Password functionality was tested for the test accounts.

The flow tested was:

```text
Forgot Password
      ↓
OTP received
      ↓
OTP verification
      ↓
New password
      ↓
Password reset
      ↓
Login with new password
```

Multiple Forgot Password attempts were performed for the same test account to verify whether the system generated and accepted OTPs correctly.

The reset-password validation was also checked against the backend schema.

---

# 12. Reset Password Testing

After receiving the Forgot Password OTP, the Reset Password API was tested.

The following points were verified:

- Valid OTP.
- Invalid OTP.
- Password validation.
- Confirm password validation.
- Successful password reset.
- Login using the newly created password.

The backend validation schemas were also checked to verify the expected request format.

---

# DAY 9 RESULT

At the end of Day 9:

- Backend setup was checked.
- PostgreSQL connection issue was resolved.
- Redis was running.
- Backend successfully started on port `3000`.
- Health API was verified.
- Network/IP configuration was checked.
- Authentication APIs were tested.
- Login and OTP issues were identified and investigated.
- Forgot Password and Reset Password flows were tested.
- Testing was performed using multiple test email accounts.

---

# DAY 10 – Detailed Multi-Account Authentication Testing

## 13. Objective

The objective of Day 10 was to perform repeated authentication testing using multiple test accounts and verify whether the previously observed issues were reproducible.

Special focus was given to:

- Login
- Logout
- Login OTP
- Forgot Password
- Forgot Password OTP
- Reset Password
- Repeated authentication requests
- Backend database records
- Token handling

---

# 14. Repeated Login and Logout Testing

For each test email, login and logout operations were repeated.

The purpose was to check whether:

- A user could log in multiple times.
- OTP could be generated for subsequent login attempts.
- Logout correctly invalidated the session/token.
- A new login could be performed after logout.
- One user's session affected another user's session.

Repeated testing helped identify authentication state and token-related issues.

---

# 15. Multiple Forgot Password Testing

Forgot Password was also tested repeatedly for the same email account.

The objective was to verify that:

- Forgot Password could be requested more than once.
- A new OTP could be generated.
- OTP verification worked correctly.
- Reset Password worked after valid verification.
- Previous OTPs did not incorrectly interfere with the new request.

The testing was repeated across the test email accounts.

---

# 16. Backend Database Verification

During troubleshooting, the PostgreSQL database was checked directly to verify whether test users were actually present.

The following query was used:

```sql
SELECT id, first_name, last_name, email FROM users;
```

The database contained the test accounts that had been created during authentication testing.

This database-level verification helped confirm whether authentication failures were caused by missing users or by API/authentication logic.

---

# 17. Test Account Cleanup

After completing the backend testing, the test accounts were removed from the database.

Initially, deleting the users directly produced a foreign-key constraint error because records in `refresh_tokens` were still associated with the users.

The error was:

```text
ERROR: update or delete on table "users" violates foreign key constraint
"refresh_tokens_user_id_fkey" on table "refresh_tokens"
```

The related refresh token records were therefore removed first.

A total of:

```text
13 refresh token records
```

were deleted.

---

# 18. Login Challenge Cleanup

After deleting the refresh tokens, deleting the users produced another foreign-key constraint error because the users were still referenced by `login_challenges`.

The error was:

```text
ERROR: update or delete on table "users" violates foreign key constraint
"login_challenges_user_id_fkey" on table "login_challenges"
```

The related login challenge records were deleted first.

A total of:

```text
9 login challenge records
```

were deleted.

---

# 19. Final Test User Deletion

After removing the dependent records, the four remaining test accounts were successfully deleted.

The command completed with:

```text
DELETE 4
```

This confirmed that the four accounts were successfully removed from the `users` table.

The final cleanup sequence was:

```text
refresh_tokens
      ↓
login_challenges
      ↓
users
```

---

# 20. Important Errors Observed During Testing

## Error 1 – PostgreSQL Connection Error

```text
ECONNREFUSED 127.0.0.1:5433
```

**Cause:** Backend was initially unable to connect to PostgreSQL.

**Action Taken:** PostgreSQL Docker container, port mapping and backend configuration were checked and corrected.

**Result:** Backend database connection was established.

---

## Error 2 – Backend Not Initially Accessible

The backend was initially not available for testing.

**Action Taken:**

- Docker services were checked.
- PostgreSQL and Redis were verified.
- Backend was restarted.
- Port `3000` was checked.
- Windows firewall rule was added.
- Local IPv4 address was checked.

**Result:**

```text
INFURNUS API listening on port 3000
```

---

## Error 3 – Invalid Email or Password

During authentication testing, the application showed:

```text
Invalid email or password
```

**Action Taken:**

- User records were checked in PostgreSQL.
- Test credentials were verified.
- Authentication API behavior was checked.
- Backend/database state was investigated.

---

## Error 4 – OTP Reuse / Repeated Login Issue

During repeated authentication testing, an OTP-related issue was observed where subsequent authentication attempts did not always behave as expected.

**Action Taken:**

- Login OTP flow was repeatedly tested.
- OTP-related backend records were checked.
- Multiple login attempts were performed.
- Different test accounts were used for comparison.

---

## Error 5 – Foreign Key Error While Deleting Users

Direct deletion of users initially failed because related `refresh_tokens` records existed.

```text
refresh_tokens_user_id_fkey
```

**Action Taken:** Related refresh token records were deleted first.

---

## Error 6 – Foreign Key Error From Login Challenges

After removing refresh tokens, user deletion was still blocked by:

```text
login_challenges_user_id_fkey
```

**Action Taken:** Related login challenge records were deleted first.

**Result:** All four test users were successfully deleted.

---

# 21. Final Backend Status

After troubleshooting and testing, the backend was successfully brought into a working state.

The final working flow was:

```text
Docker Services
      ↓
PostgreSQL Running
      ↓
Redis Running
      ↓
Backend Started
      ↓
Port 3000 Accessible
      ↓
Health API Verified
      ↓
Authentication APIs Tested
      ↓
5 Test Emails Tested
      ↓
Login / Logout Tested
      ↓
OTP Tested
      ↓
Forgot Password Tested
      ↓
Reset Password Tested
      ↓
Test Data Cleaned
```

---

# 22. Final Result

The INFURNUS backend was successfully configured and tested after resolving the initial runtime, database connectivity, network accessibility and authentication-related issues.

Backend authentication flows were tested using multiple test email accounts. Login, logout, OTP, Forgot Password and Reset Password functionality were checked through repeated API testing.

The database was also verified directly during troubleshooting, and the temporary QA test accounts and their dependent authentication records were cleaned after testing.

---

# 23. Conclusion

Day 9 and Day 10 focused on backend setup, troubleshooting and detailed authentication testing. Initial issues related to backend availability, PostgreSQL connectivity, network access and authentication were investigated systematically.

After resolving the required issues, the INFURNUS backend was successfully run and the major authentication flows were tested using multiple test accounts. Temporary testing data was cleaned from the database after completion of QA testing.
