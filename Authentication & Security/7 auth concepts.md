# Authentication

Before a system can authorize or restrict anything, it first needto know the **identity of the requester**, whether a **user**, a **service**, a **device** or a **third-party application**. This process of identifying the requester is called **authentication**.

"Authentication is the first step in any security process and is crucial for ensuring that only legitimate users or entities can access resources."

"Authentication verifies that the user or service trying to access our system is who they claim to be."

```mermaid
flowchart LR
        U[User]
        M[Mobile App]
        T[Third-party Service]
        A[Auth Server]
        G[API Gateway]

    subgraph Internal["Internal Services"]
        US[User Service]
        PS[Payment Service]
        DB[Database]
        NS[Notification Service]
    end

    %% Authentication flow
    U --> A
    M --> A
    T --> A

    A -- Auth ✅ --> G
    A -. Not Auth ❌ .-> R[Rejected]

    %% Internal services
    G --> US
    G --> PS
    G --> DB
    G --> NS
```
---

 # Most Common Authentication WRONG concepts

- Authentication methods are not the same as authentication frameworks. Authentication methods refer to the specific techniques used to verify identity (e.g., password, token, biometric), while authentication frameworks are structured systems that implement these methods (e.g., OAuth2, OpenID Connect, SAML).

- JWT (JSON Web Token) is not an authentication framework; it is a token format that can be used within various authentication methods. It is often used for stateless authentication, where the server does not need to store session information.

- Bearer authentication is a method of transmitting access tokens (like JWTs) in HTTP headers. It is not an authentication framework itself but a way to present credentials.

- OAuth2 is not an authentication method; it is an authorization framework that allows third-party applications to access user resources without exposing credentials. It can be used in conjunction with authentication methods but does not perform authentication on its own.

- SSO (Single Sign-On) is a user authentication process that allows a user to access multiple applications with one set of login credentials. It is not an authentication method but rather a user experience feature that can be implemented using various authentication methods and frameworks.

---

# Summary of Authentication Concepts

1. **Basics**: Authentication is the process of verifying the identity of a user or entity. It ensures that the requester is who they claim to be before granting access to resources. This can be done through various methods, such as *passwords*, *biometrics*, or *tokens*. It is the first step in any security process and is crucial for maintaining the integrity and confidentiality of a system.

2. **Digest**: Digest authentication is a method that uses a challenge-response mechanism to verify the identity of a user. It involves hashing the credentials and sending them over the network, providing better security than basic authentication. It helps prevent attacks like *man-in-the-middle*, *replay attacks*, and *eavesdropping* by using *nonces* and *timestamps*.

3. **API Keys**: API keys are unique identifiers used to *authenticate requests* made to an API. They are often used for server-to-server communication and can be included in request headers or query parameters. While they provide a simple way to authenticate, they should be kept secret and rotated regularly to maintain security.

4. **Session**: Session-based authentication involves creating a session on the server after a user successfully logs in. The server generates a *unique session ID*, which is sent to the client and *stored in cookies*. This session ID is used to *authenticate subsequent requests*. Sessions can be managed with expiration times and can be invalidated upon logout or after a period of inactivity.

5. **Bearer & JWT**: Bearer authentication is a method where the client sends a *token* (like a JWT) in the HTTP header to authenticate requests. JWTs are *self-contained tokens* that include claims about the user and can be verified without server-side storage. They are often used in stateless authentication systems, allowing for scalable and efficient authentication.

6. **Access & Refresh Tokens**: Access tokens are short-lived tokens used to access protected resources, while refresh tokens are long-lived tokens used to obtain new access tokens without requiring the user to re-authenticate. This mechanism *enhances security* by *limiting the exposure of access tokens* and allowing for seamless user experiences.

7. **OAuth2**: OAuth2 is an *authorization framework* that allows third-party applications to access user resources without exposing credentials. It defines various *grant types* (like authorization code, implicit, client credentials, and resource owner password credentials) to accommodate different use cases. OAuth2 is often used in conjunction with authentication methods to provide secure access to APIs.

8. **OpenID Connect**: OpenID Connect is an *authentication layer* built on top of OAuth2. It allows clients to verify the identity of users based on the authentication performed by an authorization server. It provides *ID tokens* that contain user information and can be used for *single sign-on* (SSO) and identity federation.

9. **SSO (SAML, OIDC, OAuth2)**: Single Sign-On (SSO) is a user authentication process that allows users to access multiple applications with one set of login credentials. It can be implemented using various protocols like *SAML* (Security Assertion Markup Language), *OpenID Connect* (OIDC), and *OAuth2*. SSO enhances user experience by reducing the need for multiple logins and improves security by centralizing authentication.

---

# What is Authentication?

Authentication is the process of verifying the identity of a user or entity. It ensures that the requester is who they claim to be before granting access to resources. This can be done through various methods, such as *passwords*, *biometrics*, or *tokens*. It is the first step in any security process and is crucial for maintaining the integrity and confidentiality of a system.

# Authentication flow

When a user or service attempts to access a system, the first step is to determine their identity. This is done through an authentication process that verifies the credentials provided by the requester.

```mermaid
flowchart LR
    LR([Login Request])

    LR --> D{Who is this user?}

    D -->|✅| V[VALID IDENTITY]
    D -->|❌| I[INVALID IDENTITY]

    %% Invalid path
    V --> VC[Identity Confirmed]
    VC --> GA[Grant Access]

    %% Invalid path
    I --> IC[Identity Not Confirmed]
    IC --> RJ[Reject with 401 Unauthorized]
```

# Enter System

Once the identity is confirmed, the system can then proceed to authorize access to specific resources based on predefined permissions.

```mermaid
flowchart LR
    G[API Gateway]

    subgraph API_Layer["API Layer"]
        UA[User API]
        DA[Data API]
        AA[Admin API]
    end

    subgraph Services["Services"]
        PS[Payment Service]
        NS[Notification Service]
        AS[Analytics Service]
    end

    subgraph DataStorage["Data Storage"]
        UDB[(User Database)]
        ADB[(Application Database)]
        C[(Cache)]
    end

    %% Principal flow
    G --> UA
    G --> DA
    G --> AA

    UA --> PS
    DA --> NS
    AA --> AS

    %% Connections with Data Storage
    PS --> UDB
    PS --> ADB
    NS --> ADB
    NS --> C
    AS --> UDB
    AS --> C
```

---

# 1. Basic Authentication Flow

In this flow, the client first makes a request to the server. If the server requires authentication, it responds with a 401 Unauthorized status and a WWW-Authenticate header indicating that Basic authentication is required. The client then resends the request with the Authorization header containing the Base64-encoded credentials. The server validates the credentials and either grants access or denies it based on their validity.

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    participant DB as Database

    C->>S: 1. GET /api/users (without credentials)
    S-->>C: 2. 401 Unauthorized WWW-Authenticate: Basic

    C->>C: 3. Prompt user to enter username/password
    C->>S: 4. GET /api/users Authorization: Basic <credentials>

    S->>DB: 5. Validate credentials in Database
    DB-->>S: 6. Validation result

    alt Valid credentials ✅
        S-->>C: 7.1 200 OK User data returned
    else Invalid credentials ❌
        S-->>C: 7.2 401 Unauthorized WWW-Authenticate: Basic
    end
```

The problem with this method is that they're easily reversible, only secure with HTTPS and rarely used in production, this means that the credentials are sent with every request, which can be intercepted if not properly secured. Additionally, it does not provide a way to manage sessions or tokens, making it less flexible for modern applications that require more complex authentication mechanisms.

---

# 2. Digest Authentication Flow

Digest authentication is a more secure method than Basic authentication. It uses a challenge-response mechanism to verify the identity of the user without sending the password in plaintext. The server sends a nonce (a unique value) to the client, which the client uses to create a hashed response based on the password and other parameters. This hashed response is then sent back to the server for verification.

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    participant DB as Database

    C->>S: 1. GET /api/users (without credentials)
    S-->>C: 2. 401 Unauthorized WWW-Authenticate: Digest
    S-->>C: 3. Challenge sent Includes realm, nonce, qop

    C->>C: 3.1 Client calculates hash response (username, password, realm, nonce, qop)
    C->>S: 4. GET /api/users Authorization: Digest <hashed response>

    S->>DB: 5. Validate credentials and hash
    DB-->>S: 6. Validation result

    alt Invalid credentials ❌
        S-->>C: 7.1 401 Unauthorized (retry or fail)
    else Valid credentials ✅
        S-->>C: 7.2 200 OK User data returned
    end
```

This is better than Basic authentication as it does not send the password in plaintext and uses MD5 hashing to create a response. However, it is still considered outdated and is rarely used in modern applications due to vulnerabilities in the MD5 algorithm and the complexity of implementing it correctly.

---

# 3. API Key Authentication Flow

```mermaid
sequenceDiagram
    participant C as Client/App
    participant S as API Server
    participant D as Database

    C->>S: 1. GET /api/data Authorization: Bearer <API_KEY>
    S->>D: 2. Validate API Key in Database
    D-->>S: 3. Validation result (valid/invalid)

    alt API Key invalid ❌
        S-->>C: 4. 401 Unauthorized Access denied
    else API Key valid ✅
        S->>D: 5. Execute query in Database
        D-->>S: 6. Database returns data
        S-->>C: 7. 200 OK + payload
    end
```

This diagram shows:

- The Client/App sends a request with the API Key.
- The API Server validates the key.
- If invalid → returns 401 Unauthorized.
- If valid → queries the Database, receives the data, and responds with 200 OK.

Using API keys is a simple way to authenticate requests, especially for server-to-server communication. However, they should be kept secret and rotated regularly to maintain security. Additionally, API keys do not provide user-level authentication and are not suitable for scenarios where user identity needs to be verified.

The API key can be similar to JWTs but it is not a token format, in API key authentication, the key is usually a static string that identifies the client or application, while JWTs are dynamic tokens that can carry informations about the user and have expiration times. API keys are often used for service-to-service authentication, while JWTs are more commonly used for user authentication in web applications.

---

# 4. Session-Based Authentication Flow

Session-based authentication is a common method used in web applications. It involves creating a session on the server after a user successfully logs in. The server then sends a session ID to the client, usually stored in a cookie. For subsequent requests, the client sends the session ID, and the server validates it to authenticate the user.

```mermaid
sequenceDiagram
    participant U as User
    participant W as Web Server
    participant S as Session Store
    participant DB as Database

    U->>W: 1. POST /login with credentials (username/password)
    W->>DB: 2. Validate credentials in Database
    DB-->>W: 3. Validation result

    alt Valid credentials ✅
        W->>S: 4. Create session
        S-->>W: 5. Return Session ID
        W-->>U: 6. 200 OK Set-Cookie: session_id=<session_id>

        U->>W: 7. GET /api/users Cookie: session_id=<session_id>
        W->>S: 8. Validate session
        S-->>W: 9. Session valid

        W-->>U: 10. 200 OK User data returned
    else Invalid credentials ❌
        W-->>U: 6.1 401 Unauthorized Login failed
    end

    alt Session expired or invalid ❌
        W-->>U: 11. 401 Unauthorized Redirect to login
    end
```

A session-based authentication flow typically involves the following steps:
1. The user submits their credentials (username and password) to the server.
2. The server validates the credentials and creates a session, generating a unique session ID.
3. The session ID is sent back to the client, usually stored in a cookie.
4. For subsequent requests, the client includes the session ID in the request headers or cookies.
5. The server validates the session ID against its session store to authenticate the user.
6. If the session is valid, the server processes the request and returns the appropriate response.
7. If the session is invalid or expired, the server responds with an error (e.g., 401 Unauthorized) and may redirect the user to the login page.

This method is widely used because it allows the server to maintain state and manage user sessions effectively. However, it requires server-side storage of session data, which can become a scalability concern for large applications. Additionally, session IDs must be protected against theft (e.g., through secure cookies) to prevent unauthorized access.

With the rise of stateless architectures and microservices, session-based authentication is less favored compared to token-based methods like JWTs, which do not require server-side session storage and can be more easily scaled across distributed systems.

---

# 5. Bearer Token & JWT Authentication Flow

Instead of session-based authentication, many modern applications use token-based authentication, such as Bearer tokens and JSON Web Tokens (JWTs). In this flow, after a user successfully logs in, the server issues a JWT, which the client includes in the Authorization header of subsequent requests.

```mermaid
sequenceDiagram
    participant U as User
    participant C as Client/App
    participant S as API Server
    participant DB as Database

    U->>C: 1. Enter credentials (username/password)
    C->>S: 2. POST /login {username, password}
    S->>DB: 3. Validate credentials in Database
    DB-->>S: 4. Valid/Invalid credentials

    alt Valid credentials ✅
        S-->>C: 5. JWT issued Authorization: Bearer <token>
        C->>S: 6. GET /api/resource Authorization: Bearer <token>
        S->>S: 7. Verify JWT signature and validity
        alt Token valid and not expired
            S->>DB: 8. Query data
            DB-->>S: 9. Return data
            S-->>C: 10. 200 OK + payload
        else Token invalid or expired ❌
            S-->>C: 11. 401 Unauthorized Token invalid/expired
        end
    else Invalid credentials ❌
        S-->>C: 5.1 401 Unauthorized Login failed
    end
```

The most common type of token used in this flow is the JWT, which is a self-contained token that includes claims about the user and can be verified without server-side storage. This allows for stateless authentication, making it easier to scale applications across distributed systems. JWT carry withself information about the user, such as their ID, roles, and permissions, in a JSON format, which can be decoded and verified by the server.

The downside of using JWTs is that they can become large in size, especially if they contain many claims, which can impact performance. Additionally, since JWTs are stateless, revoking them before their expiration time can be challenging, requiring additional mechanisms like token blacklisting or short-lived tokens with refresh tokens.

---

# 6. Access & Refresh Token Flow

In this flow, the server issues two tokens upon successful authentication: an access token and a refresh token. The access token is short-lived and used to access protected resources, while the refresh token is long-lived and used to obtain a new access token when the current one expires.

```mermaid
sequenceDiagram
    participant U as User
    participant C as Client/App
    participant S as API Server
    participant DB as Database

    U->>C: 1. Enter with credentials (username/password)
    C->>S: 2. POST /login {username, password}
    S->>DB: 3. Validate credentials in Database
    DB-->>S: 4. Validation result

    alt Valid credentials ✅
        S-->>C: 5. Returns Access Token (short-lived) + Refresh Token (long-lived)

        C->>S: 6. GET /api/resource Authorization: Bearer <Access Token>
        S->>S: 7. Validate Access Token (signature + expiration)

        alt Access Token valid
            S->>DB: 8. Query data
            DB-->>S: 9. Return data
            S-->>C: 10. 200 OK + payload
        else Access Token expired ❌
            C->>S: 11. POST /refresh Authorization: Bearer <Refresh Token>
            S->>S: 12. Validate Refresh Token
            alt Refresh Token valid
                S-->>C: 13. New Access Token issued
                C->>S: 14. Resend request with new Access Token
                S-->>C: 15. 200 OK + payload
            else Refresh Token invalid/expired ❌
                S-->>C: 16. 401 Unauthorized User must re-login
            end
        end
    else Invalid credentials ❌
        S-->>C: 5.1 401 Unauthorized Login failed
    end
```

This flow enhances security by limiting the exposure of access tokens and allowing for seamless user experiences. Access tokens are typically short-lived (e.g., 15 minutes to 1 hour), while refresh tokens can last much longer (e.g., days or weeks). This reduces the risk of token theft and misuse, as stolen access tokens will expire quickly.

*XSS (Cross-Site Scripting)* attacks can be a concern with *refresh tokens*, especially if they are stored in local storage or accessible via JavaScript. To mitigate this risk, refresh tokens should be stored in HTTP-only cookies, which are not accessible via JavaScript, reducing the attack surface for XSS vulnerabilities. The use of secure cookies with the `HttpOnly` and `Secure` flags helps protect refresh tokens from being accessed by malicious scripts, ensuring that they are only sent over HTTPS connections and are not exposed to client-side code.

---

# 7. OAuth2 Flow

OAuth2 is an authorization framework that allows third-party applications to access user resources without exposing credentials. It defines various grant types to accommodate different use cases, such as the authorization code flow, implicit flow, client credentials flow, and resource owner password credentials flow.

It's important to remember that OAuth2 is primarily an *authorization framework*, not an *authentication method*. However, it can be used in conjunction with authentication methods to provide secure access to APIs and resources.

```mermaid
sequenceDiagram
    participant U as User
    participant C as Client/App
    participant O as Google OAuth Server
    participant D as Google Drive API

    U->>C: 1. Connect Google Drive
    C->>O: 2. Redirect to Consent Screen
    O->>U: 3. Show Permission Request (Scopes)
    U->>O: 4. Allow Access
    O-->>C: 5. Return Authorization Code

    C->>O: 6. POST /token {authorization_code}
    O-->>C: 7. Access Token + Refresh Token

    C->>D: 8. GET /files Authorization: Bearer <Access Token>
    D-->>C: 9. Return User Files

    Note over C,O: Access Token = short-lived proof of authorization<br/>Refresh Token = long-lived credential to renew access

    alt Access Token expired ❌
        C->>O: 10. POST /token {refresh_token}
        O-->>C: 11. New Access Token issued
        C->>D: 12. Retry request with new Access Token
        D-->>C: 13. 200 OK + payload
    else Access Token valid ✅
        D-->>C: 14. Continue serving requests
    end

    Note over C,D: OAuth2 is authorization only, not authentication.<br/>Access Token proves the app CAN access resources,<br/>but does NOT identify the user.
```

In this diagram, the user initiates the OAuth2 flow by connecting their Google Drive account to a third-party application. The application redirects the user to the Google OAuth server, where they are presented with a consent screen requesting permission to access specific resources (scopes). Upon granting access, the OAuth server returns an authorization code to the application. The application then exchanges the authorization code for an access token and a refresh token. To this point, the access token allows the application to access the user's resources on Google Drive without exposing the user's credentials. If the access token expires, the application can use the refresh token to obtain a new access token without requiring the user to re-authenticate.

---

# 8. OpenID Connect Flow

OpenID Connect (OIDC) is an authentication layer *built on top of OAuth2*. It allows clients to verify the identity of users based on the authentication performed by an authorization server. OIDC *provides ID tokens* that contain user information and can be used for single sign-on (SSO) and identity federation.

In this flow, after the user authenticates with the authorization server, an ID token is issued along with the access token. The ID token contains claims about the user, such as their unique identifier, email, and other profile information. The client can use this ID token to authenticate the user and establish a session.

```mermaid
sequenceDiagram
    participant U as User
    participant C as Your App (Client)
    participant G as Google Identity (Auth Server)
    participant B as Your Backend (Resource Server)

    U->>C: 1. Click "Sign in with Google"
    C->>G: 2. Redirect to Authorization Endpoint
    G->>U: 3. Show Login Screen
    U->>G: 4. Enter Credentials + Consent
    G-->>C: 5. Return Authorization Code

    C->>G: 6. POST /token {authorization_code}
    G-->>C: 7. Access Token + ID Token

    C->>B: 8. Send ID Token for Verification
    B->>B: 9. Verify Signature + Claims (iss, aud, exp)
    B-->>C: 10. Identity confirmed, session created
    C-->>U: 11. Logged in successfully

    Note over G,C: Access Token → authorization for APIs<br/>ID Token → authentication (who the user is)
    Note over B,C: Backend validates ID Token using Google's public keys<br/>Ensures token is not expired and issued for this app
```

In this flow, the user initiates the login process by clicking "Sign in with Google." The application redirects the user to Google's authorization endpoint, where they log in and grant consent. Google then returns an authorization code to the application, which exchanges it for an access token and an ID token. The ID token is sent to the backend for verification, confirming the user's identity and allowing the backend to create a session for the user.

---

# 9. SSO (Single Sign-On) Flow

Single Sign-On (SSO) is a user experience feature, not an authentication method. It allows users to access multiple applications with one set of login credentials. SSO can be implemented using multiple services and protocols, such as *SAML* (Security Assertion Markup Language), *OpenID Connect* (OIDC), and *OAuth2*. The goal of SSO is to enhance user experience by reducing the need for multiple logins and improving security by centralizing authentication.

```mermaid
graph LR
    %% Usuário
    U[User]

    %% Identity Provider
    IdP[Identity Provider Google Okta etc]

    %% Sessão Global
    S[Session Storage]

    %% Aplicações
    Gm[Gmail]
    Dr[Google Drive]
    Yt[YouTube]

    %% Fluxo principal
    U -->|1. Login once| IdP
    IdP -->|Set SSO Cookie| U

    %% Sessão armazenada
    IdP --> S

    %% Acesso às aplicações
    U -->|2. Access Gmail| Gm
    Gm -->|Verify session| IdP
    IdP -->|Confirmed ✓| Gm

    U -->|3. Access Drive| Dr
    Dr -->|Verify session| IdP
    IdP -->|Confirmed ✓ no login needed| Dr

    U --> Yt
    Yt -->|Verify session| IdP
    IdP -->|Confirmed ✓| Yt

    %% Notas
    note1["SSO: usuário autentica uma vez e acessa múltiplas aplicações sem relogar."]
    note2["Sessão global é mantida pelo Identity Provider e validada em cada aplicação."]
    IdP --- note1
    S --- note2
```

In this flow, the user logs in once with the Identity Provider (IdP), which sets a global SSO session. When the user accesses different applications (like Gmail, Google Drive, and YouTube), each application verifies the session with the IdP. If the session is valid, access is granted without requiring the user to log in again.

Let's say the user logs in to Gmail first. The IdP sets a session cookie for the user. When the user then tries to access Google Drive, the application checks with the IdP to verify the session. Since the session is valid, the user is granted access without needing to log in again. The same process occurs when accessing YouTube.