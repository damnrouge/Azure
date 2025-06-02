User->>Client: Clicks "Sign in with Google"
Client->>User: Redirect to OP
User->>OP: Logs in & consents
OP->>User: Redirect with "code"
User->>Client: Passes "code" to Client
Client->>OP: Sends "code" + client_secret
OP->>Client: Returns id_token (JWT) + access_token
Client->>Client: Validates id_token (user identity)
