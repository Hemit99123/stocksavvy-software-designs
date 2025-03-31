## 🎮 RACT (Role Assignment CLI Tool):

All commands run in infinite loops so they will always return to the start of the program. To stop it, you must press Ctrl+C like any other CLI-based tool.

**TO DO: March 31st 2025** Update the redis session querying to include the RediSearch and querying w/ name from json field using index already built in CLI

From this query, we get keys which we can use to delete, revoking users authentication status to switch role

Branch out from the Retrieve Redis session to show its process of using the index for querying so we can query w/ name from JSON body

```mermaid
flowchart 
    Start["Start"] --> CommandChoice{"Developer selects a command"}
    
    CommandChoice -- Add/Update Configuration --> RequestDBCredentials(["Prompt for PostgreSQL and Redis connection URLs"])
    RequestDBCredentials --> CheckConfigFile{"Does 'credentials.json' exist?"}
    CheckConfigFile -- Yes --> LoadAndUpdateConfig(["Load 'credentials.json' and update database fields"])
    CheckConfigFile -- No --> CreateConfigFile(["Create 'credentials.json' and add database fields"])
    LoadAndUpdateConfig --> ReturnToStart(["Return to start"])
    CreateConfigFile --> ReturnToStart

    CommandChoice -- Update User Role --> RequestUserEmail(["Prompt for target user's email"])
    RequestUserEmail --> CheckCredentialsFile{"Does 'credentials.json' exist?"}
    CheckCredentialsFile -- Exists --> ReadCredentials(["Read credentials from file"])
    CheckCredentialsFile -- Does Not Exist --> ReturnToStart2(["Return to start"])
    ReadCredentials --> ConnectToDB(["Use credentials to connect to PostgreSQL and Redis"])
    ConnectToDB --> RetrieveUserData(["Retrieve user sessions from Redis and user record from PostgreSQL"])

    RetrieveUserData --> DeleteUserSessions(["Delete all user sessions via RediSearch index"])
    DeleteUserSessions --> PromptForRoleChange{"Select new role type"}
    PromptForRoleChange -- Admin --> UpdateToAdmin(["Set user role to 'admin' in PostgreSQL"])
    PromptForRoleChange -- User --> UpdateToUser(["Set user role to 'user' in PostgreSQL"])
    UpdateToAdmin --> ReturnToStart
    UpdateToUser --> ReturnToStart
    ReturnToStart --> Start
```
