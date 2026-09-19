# 🔐 JWT Security Testing Methodology

JSON Web Tokens (JWTs) are commonly used to implement authentication and authorization in modern web applications and APIs.

JWTs can be useful for stateless authentication, but insecure implementation or validation can introduce serious security risks.

This guide presents a practical methodology for understanding and testing JWT-based authentication during **authorized security assessments**.

> ⚠️ Test only applications, APIs, labs, and systems where you have explicit permission.

---

## 🧠 What Is a JWT?

A JSON Web Token generally consists of three Base64URL-encoded components:

```text
HEADER.PAYLOAD.SIGNATURE
```

Example structure:

```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.
eyJzdWIiOiIxMjMiLCJyb2xlIjoidXNlciJ9
.
SIGNATURE
```

The three components are:

### 1️⃣ Header

Contains information about the token, such as the signing algorithm.

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### 2️⃣ Payload

Contains claims.

```json
{
  "sub": "123",
  "role": "user"
}
```

### 3️⃣ Signature

Used by the server to verify that the token was generated and signed correctly.

---

# 🔎 JWT Security Testing Methodology

## 1️⃣ Identify JWT Usage

First determine whether the application uses JWTs.

Look for tokens in:

```text
Authorization headers
Cookies
Local storage
Session storage
API requests
```

A common example:

```http
Authorization: Bearer <JWT>
```

---

## 2️⃣ Decode the Token

JWTs can be decoded because their header and payload are encoded rather than encrypted.

For example:

```text
HEADER
   ↓
Base64URL Decode
   ↓
JSON

PAYLOAD
   ↓
Base64URL Decode
   ↓
JSON
```

Look for claims such as:

```text
sub
iss
aud
exp
iat
nbf
role
user
scope
```

Remember:

> Decoding a JWT does not mean its signature has been verified.

---

# 🧪 3️⃣ Analyze the Header

Check the `alg` value.

Example:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

During an authorized assessment, determine whether the server properly enforces its expected signing algorithm.

Do not assume that changing the header alone makes a token valid.

The server must correctly validate the token's signature and algorithm.

---

# 🔍 4️⃣ Examine Claims

Review security-sensitive claims such as:

```json
{
  "sub": "123",
  "role": "user",
  "exp": 1790000000
}
```

Questions to investigate:

* Is the subject validated?
* Is the expiration enforced?
* Is the issuer validated where appropriate?
* Is the audience validated where appropriate?
* Are role or privilege claims trusted without proper validation?
* Are tokens accepted after expiration?

---

# ⏰ 5️⃣ Test Token Expiration

Check whether expired tokens are rejected.

Conceptually:

```text
Valid Token
     ↓
Before expiration → Accepted

Expired Token
     ↓
After expiration → Should be rejected
```

If an application continues accepting expired authentication tokens, investigate the session-management implementation.

---

# 👤 6️⃣ Test Authorization Separately

Authentication answers:

> **Who are you?**

Authorization answers:

> **What are you allowed to access?**

A valid JWT does not automatically mean that every requested resource should be accessible.

For example:

```text
User Token
     ↓
/api/user/profile       → Allowed
/api/admin/settings     → Should require appropriate authorization
```

Always test authorization independently from token validity.

---

# 🔄 7️⃣ Test Token Reuse

During an authorized assessment, determine how the application handles:

* Logout
* Password changes
* Account disablement
* Session expiration
* Credential changes

A security-conscious implementation should have an appropriate strategy for invalidating or otherwise limiting the usefulness of tokens when required.

---

# 🛡️ 8️⃣ Check Sensitive Data in the Payload

JWT payloads are readable by whoever possesses the token.

Avoid placing sensitive information directly inside the payload unless the design specifically protects it.

For example, avoid unnecessary inclusion of:

```json
{
  "password": "...",
  "credit_card": "...",
  "secret_key": "..."
}
```

A JWT payload should not be treated as a secure storage mechanism.

---

# 🚨 Common JWT Security Issues

During an authorized assessment, pay attention to issues such as:

* Weak token validation
* Improper algorithm handling
* Missing expiration validation
* Incorrect claim validation
* Excessive token lifetime
* Sensitive information in tokens
* Improper authorization
* Insecure token storage
* Failure to handle token revocation appropriately
* Weak signing-key management

---

# 🧪 Example Testing Workflow

A simple workflow:

```text
Identify JWT
     ↓
Capture Authentication Request
     ↓
Decode Header & Payload
     ↓
Analyze Claims
     ↓
Understand Signature Validation
     ↓
Review Expiration
     ↓
Test Authorization
     ↓
Review Token Lifecycle
     ↓
Document Findings
```

---

# 🛡️ How Developers Can Improve JWT Security

### Validate the signature

The server should properly verify the JWT signature before trusting its claims.

### Enforce expected algorithms

Do not blindly trust the algorithm specified by an untrusted token.

### Validate important claims

Depending on the application, validate claims such as:

```text
iss
aud
exp
nbf
```

### Keep token lifetimes appropriate

Short-lived access tokens can reduce the window of opportunity if a token is exposed.

### Protect signing keys

Signing keys should never be hard-coded into public repositories.

### Avoid sensitive data

Do not treat JWT payloads as encrypted storage.

### Enforce authorization

A valid JWT should not automatically grant access to every resource.

---

# 📋 JWT Security Checklist

```text
[ ] JWT usage identified
[ ] Header analyzed
[ ] Algorithm reviewed
[ ] Payload claims reviewed
[ ] Signature validation understood
[ ] Expiration behavior tested
[ ] Issuer validation reviewed
[ ] Audience validation reviewed
[ ] Authorization tested
[ ] Token lifecycle reviewed
[ ] Sensitive data checked
[ ] Token storage reviewed
[ ] Findings documented
```

---

# 💡 Key Takeaways

JWT security isn't simply about decoding a token or changing its claims.

The important questions are:

> **Is the token authentic?**

> **Is it still valid?**

> **Was it issued for this application?**

> **Is this user authorized to perform this action?**

A secure JWT implementation requires proper **signature verification, claim validation, token lifecycle management, and authorization controls**.

---

## 📚 Further Learning

Recommended topics to study next:

* Authentication
* Authorization
* Session Management
* OAuth 2.0
* OpenID Connect
* API Security
* Access Control
* Broken Object Level Authorization
* Web Application Security

---

### ⚠️ Ethical Testing Notice

The techniques discussed in this guide should only be used against systems where you have explicit authorization.

For hands-on practice, use intentionally vulnerable applications, CTFs, and authorized security labs.

#Cybersecurity #JWT #WebSecurity #APISecurity #BugBounty #AppSec #Pentesting #EthicalHacking #OWASP #BurpSuite

