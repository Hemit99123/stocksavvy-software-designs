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
flowchart 
    A([Start Website]) --> B[OTP Generation]
    B --> KJ[Send email of OTP]
    KJ --> C[Store OTP in Redis]
    C --> D([OTP Expiry])
    D --> O([End])
  
    C --> E{User's OTP matches Redis records}
    E -- Yes --> F[Assign Session]
    E -- No --> O
  
    F --> G[Assign Session to Redis]
    G --> HN[Index of idx:session is automatically attached, meaning users can query this session with session-owner's name]
    G --> H[Session Details<br/>• Email<br/>• Time Created<br/>• Role]
    H --> I[Create Cookie with Session ID]
    I --> J{Check Session ID with Redis?}
    J -- True --> K{Check Role?}
    J -- False --> O
  
    K -- User --> L[User App]
    K -- Admin --> M[Admin App]
    L --> N([Logout])
    M --> N
    N --> P([Delete Sessions])
    P --> O


```


