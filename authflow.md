## 🔐 Authentication Flow

The authentication for StockSavvy is conducted through session based authentication. 

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
### Cookies Communication In-Depth:

The cookies are sent and stored within the frontend client and attached to each response sent by a speific backend client. 
