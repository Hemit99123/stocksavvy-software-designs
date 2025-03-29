## Authentication Flow

This flow contains 2 apps:
- The main website
- A CLI tool used by developers to assign users to admin roles

The flow contains logic for a session-based authentication and a role based authentication system with a user and admin role. 

### 🚀 Main website:
```mermaid 
flowchart 
    A([Start Website]) --> B[OTP Generation]
    B --> C[Store OTP in Redis]
    C --> D([OTP Expiry])
    D --> O([End])
  
    C --> E{User's OTP matches Redis records}
    E -- Yes --> F[Assign Session]
    E -- No --> O
  
    F --> G[Assign Session to Redis]
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

### 🎮 RACT (Role assignment CLI Tool):

```mermaid
flowchart
    Q([Start Role Assignment Script]) --> R[Developer inputs new role + user email]
    R --> G[Find user in database through email identifer]
    G --> K[Update user field with new role]
    K --> T([End])
```
