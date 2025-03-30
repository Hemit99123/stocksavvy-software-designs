## 🔐 Authentication Flow

The authentication for StockSavvy is conducted through Redis-stored sessions. All of these sessions have an associated index for easier retrival when you need to query speific content from the body (name, email, etc). This index and advanced "full-text" querying is acheived through the **RediSearch module**.

### Here is an example of it on Redis-CLI:
- created JSON sessions
- created idx:session index that stores sessions names for retrvial
- retrieving each session built through referencing associated names with idx:session index
  
<img width="1231" alt="Screenshot 2025-03-30 at 6 50 54 PM" src="https://github.com/user-attachments/assets/740c6e26-c4ce-4f69-b988-e602c62322a0" />
<br />
<br />
<br />
This same logic will be applied to the authflow in which all sessions are associated with an index which is configured through CLI-interface (same command as <b>FT.CREATE</b>) meaning all sessions will now have this index attached to it.
<br />

```mermaid 
flowchart TB
    Start["Start Process"] --> GenerateOTP["Generate OTP (one time password)"]
    GenerateOTP --> CacheOTP["Cache OTP in Redis"] & OTPExpiry["OTP reaches expiry time (3mins)"] & SendOTP["Send OTP via Email"]
    OTPExpiry --> End["End Process"]
    SendOTP --> UserSubmit["User submits OTP + email + name data"]
    UserSubmit --> ValidateOTP{"Check if OTP matches Redis records"}
    ValidateOTP -- Yes --> CheckDatabase{Compare email and name in SQL user table}
    ValidateOTP -- No --> End


    CheckDatabase -- Exists --> StoreSession["Store Session Data in Redis (name, email, role)"]
    CheckDatabase -- Does NOT Exist --> CreateUser["Create a user document with email, name, role"]
    CreateUser --> StoreSession

    StoreSession --> AttachIndex["Attach session index to query w/ name"] & CreateCookie["Generate Session Cookie with the session id as the body"]
    CreateCookie --> ValidateCookie{"Validate Session Cookie with Redis through querying session id in the cache"}
    ValidateCookie -- Exists --> CheckRole{"Determine User Role"}
    ValidateCookie -- Does NOT Exist --> End
    CheckRole -- User --> UserApp["Redirect to User Application"]
    CheckRole -- Admin --> AdminApp["Redirect to Admin Dashboard"]
    UserApp --> Logout["User Logout"]
    AdminApp --> Logout
    Logout --> DeleteSession["Clear Session Cookie + Redis key-value pair"]
    DeleteSession --> End
```
