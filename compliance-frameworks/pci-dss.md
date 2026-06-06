# PCI-DSS (Payment Card Industry Data Security Standard)

## Plain English Overview

**PCI-DSS** is a security standard that organizations must follow if they process, store, or transmit credit card information. It was created by major payment card companies (Visa, Mastercard, American Express, Discover, JCB) to protect cardholder data.

### Why Exist?

Every year, credit card data breaches expose millions of card numbers. PCI-DSS provides a baseline of security controls that all organizations handling card data must implement.

### Who Must Comply?

- **Any organization** that accepts, processes, or stores credit card data
- E-commerce sites
- Retail stores with payment systems
- Banks and payment processors
- Organizations using cloud payment services
- **Even small businesses** if they accept credit cards (no exceptions!)

### What Happens if You Don't Comply?

- **Fines:** $5,000-$100,000+ per month
- **Card brand penalties:** Additional fees from Visa, Mastercard, etc.
- **Legal liability:** If data breached, liable for damages
- **Reputation damage:** Loss of customer trust
- **Blocked from processing payments:** Can't accept credit cards

## The 12 Core Requirements

PCI-DSS has 12 main requirements organized into 6 focus areas.

### Focus Area 1: Secure Network

#### Requirement 1: Install & Maintain Firewall Configuration

**Goal:** Protect cardholder data from network attacks.

**What You Must Do:**
- Use firewalls to control traffic into/out of cardholder environment
- Block all unnecessary traffic
- Define firewall rules document with business justification
- Review firewall rules regularly

**Real-World Example:**
```
Bad: All ports open, anyone can access payment database
Good: Only payment processing servers can access database on port 3306,
      all other traffic blocked
```

**Implementation:**
- Configure firewall rules (deny by default, allow specific)
- Disable unnecessary ports/protocols
- Review rules quarterly
- Document all firewall changes

#### Requirement 2: Don't Use Vendor-Supplied Defaults

**Goal:** Remove easy entry points for attackers.

**What You Must Do:**
- Change all default passwords immediately
- Remove unnecessary accounts
- Disable default services
- Configure systems securely from the start

**Real-World Example:**
```
Bad: Database admin password still set to default "admin"
     Router password still "password"
Good: Change all passwords to strong, unique values
      Disable default accounts
```

**Implementation:**
- Create strong passwords for all default accounts
- Disable unnecessary default services
- Remove or rename default administrator accounts
- Document all password changes

### Focus Area 2: Data Protection

#### Requirement 3: Protect Stored Cardholder Data

**Goal:** If cardholder data is breached, it's unreadable.

**What You Must Do:**
- Keep cardholder data to minimum necessary (data minimization)
- Encrypt stored data
- Use encryption keys securely
- Destroy data when no longer needed

**Real-World Example:**
```
Bad: Customer credit card numbers stored in spreadsheet in plaintext
Good: Store only last 4 digits of card number
      Full card number encrypted with AES-256
      Kept only as long as needed, then securely deleted
```

**Implementation:**
- Store only: card number, expiration date, CVV (if required)
- Don't store: PIN, magnetic stripe data
- Encrypt all stored cardholder data
- Use strong encryption (AES-128+ or RSA-2048+)

#### Requirement 4: Encrypt Cardholder Data in Transit

**Goal:** Prevent eavesdropping on payment data in transmission.

**What You Must Do:**
- Use HTTPS (TLS 1.2+) for all transmission
- Use VPN for non-web protocols
- Encrypt data over public networks

**Real-World Example:**
```
Bad: Customer sends credit card number over unencrypted HTTP
     Attacker intercepts and steals card number
Good: Customer sends card number over HTTPS (encrypted)
      Only payment processor can read data
```

**Implementation:**
- Use TLS 1.2 or higher
- Install valid SSL/TLS certificates
- Disable SSL 3.0 and TLS 1.0/1.1 (too weak)
- Encrypt remote access (SSH, not Telnet)

### Focus Area 3: Vulnerability Management

#### Requirement 5: Protect Against Malware

**Goal:** Prevent malware from stealing cardholder data.

**What You Must Do:**
- Install anti-malware software on all systems
- Keep anti-malware updated
- Scan systems regularly
- Configure firewall to block malware

**Real-World Example:**
```
Bad: No antivirus installed; malware infects server and steals card data
Good: Antivirus installed, definitions updated daily, real-time scanning enabled
```

**Implementation:**
- Deploy antivirus/anti-malware on all systems
- Enable real-time scanning
- Update definitions automatically (daily)
- Perform regular scans
- Monitor for malware alerts

#### Requirement 6: Develop & Maintain Secure Systems

**Goal:** Prevent vulnerabilities from being exploited.

**What You Must Do:**
- Keep all software patched and updated
- Follow secure coding practices
- Test for vulnerabilities
- Use Web Application Firewall (WAF) if applicable
- Remove unnecessary features/ports

**Real-World Example:**
```
Bad: Apache server has known vulnerability, not patched
     Attacker exploits vulnerability to access payment database
Good: Patch applied within 30 days
      Vulnerability scanning identifies missing patches
      Development uses secure coding standards
```

**Implementation:**
- Apply all security patches within 30 days
- Use code review/static analysis during development
- Perform penetration testing annually
- Deploy WAF for web applications
- Remove unnecessary features/services

### Focus Area 4: Access Control

#### Requirement 7: Limit Access to Cardholder Data

**Goal:** Only people who need cardholder data can access it.

**What You Must Do:**
- Grant access on need-to-know basis
- Restrict access by job function
- Change access when employees leave
- Track who accessed what data

**Real-World Example:**
```
Bad: All employees can access entire database
     Disgruntled employee steals all card data before leaving
Good: Only payment processors see full card numbers
      Accountants see only transaction totals
      Access removed immediately when employee leaves
```

**Implementation:**
- Define access levels by job role
- Document access requirements
- Grant minimum necessary access
- Review access quarterly
- Remove access immediately upon termination

#### Requirement 8: Identify & Authenticate Access

**Goal:** Verify that users are who they claim to be.

**What You Must Do:**
- Require unique user ID for each person
- Use strong authentication (passwords or multi-factor)
- Implement Multi-Factor Authentication (MFA) for remote access
- Force password changes regularly
- Don't share login credentials

**Real-World Example:**
```
Bad: Multiple employees share single login
     "paymentadmin" / "password123"
     Can't tell who accessed data
Good: Each employee has unique username
      Password + token (MFA) required
      All access logged with username
```

**Implementation:**
- Create unique user ID for each person
- Require strong passwords (12+ chars, complexity)
- Implement MFA for administrative access
- Force password changes every 90 days
- Prevent password reuse (last 4 passwords)
- Lock account after 6 failed login attempts
- Log all access

#### Requirement 9: Restrict Physical Access

**Goal:** Prevent unauthorized physical access to systems/data.

**What You Must Do:**
- Control access to facilities
- Lock rooms with cardholder data
- Use surveillance cameras
- Log all physical access
- Destroy sensitive materials securely

**Real-World Example:**
```
Bad: Server room unlocked, anyone can access payment database server
     Contractor steals hard drive with card data
Good: Server room locked, badge access required
      All entries logged with timestamp
      Card data encrypted on hard drives
```

**Implementation:**
- Control building/room access (badges, security guards)
- Use surveillance cameras
- Maintain access logs
- Shred all documents with cardholder data
- Destroy hard drives securely before disposal

### Focus Area 5: Monitoring & Testing

#### Requirement 10: Track & Monitor Access to Cardholder Data

**Goal:** Detect suspicious activity and attacks.

**What You Must Do:**
- Log all access to cardholder data
- Monitor networks for attacks
- Review logs regularly
- Maintain logs for at least 1 year
- Centralize log management

**Real-World Example:**
```
Bad: No logs; attacker accesses database for weeks undetected
Good: Access logs show multiple failed login attempts
      Logs reviewed daily, suspicious activity investigated immediately
```

**Implementation:**
- Enable logging on all systems
- Centralize logs on secure server
- Retain logs for 1 year minimum
- Review logs regularly (daily for incidents)
- Set up alerts for suspicious activity
- Protect logs from unauthorized modification

#### Requirement 11: Test Security Regularly

**Goal:** Find and fix vulnerabilities before attackers do.

**What You Must Do:**
- Run vulnerability scans quarterly (every 3 months)
- Run penetration tests annually
- Test after major changes
- Fix identified vulnerabilities promptly

**Real-World Example:**
```
Bad: No security testing; vulnerability unknown until after breach
Good: Quarterly scans identify missing patches
      Annual penetration test finds insecure configuration
      Issues fixed before attackers find them
```

**Implementation:**
- Quarterly internal/external vulnerability scans
- Annual external penetration testing
- Test after any major system changes
- Remediate all vulnerabilities with assigned timelines
- Verify fixes with follow-up scans

### Focus Area 6: Information Security Policy

#### Requirement 12: Maintain Information Security Policy

**Goal:** Establish security culture and accountability.

**What You Must Do:**
- Create comprehensive security policies
- Provide security training to staff
- Establish incident response procedures
- Assign responsibility for security

**Real-World Example:**
```
Bad: No security policy; employees don't know they shouldn't email card numbers
Good: Written policy prohibits sharing cardholder data
      Annual training for all staff
      Clear incident reporting procedures
```

**Implementation:**
- Document security policies
- Train all employees annually
- Require acknowledgment of policies
- Create incident response team
- Establish incident reporting procedures
- Conduct regular drills/tests

## Compliance Levels

PCI-DSS has four compliance levels based on transaction volume:

| Level | Transaction Volume | Assessment |
|-------|-------------------|-----------|
| 1 | 6+ million cards/year | Annual on-site audit by auditor |
| 2 | 1-6 million cards/year | Annual self-assessment questionnaire |
| 3 | 20,000-1 million cards/year | Annual self-assessment + network scan |
| 4 | <20,000 cards/year | Annual self-assessment + network scan |

### Assessment Process

1. **Self-Assessment Questionnaire (SAQ):** Complete questionnaire about compliance
2. **Vulnerability Scanning:** Run approved scanning vendors (ASVs)
3. **Audit Report (Level 1 only):** On-site audit by Qualified Security Assessor (QSA)
4. **Submit to Payment Card Company:** Provide proof of compliance

## Common Misconceptions

### "We use a payment processor, so we don't need PCI-DSS"
**FALSE.** Even if you use a payment processor, if you store/process any cardholder data, you're responsible for PCI-DSS compliance.

### "PCI-DSS only applies to large companies"
**FALSE.** Every organization processing credit cards must comply, regardless of size.

### "Compliance means we're completely secure"
**FALSE.** PCI-DSS is a minimum baseline. Additional security measures are often needed.

### "We had a penetration test, so we're compliant"
**FALSE.** PCI-DSS requires comprehensive controls, not just testing.

## Real-World Breach Examples

### Target (2013)
- **What Happened:** Attackers stole 40 million credit card numbers
- **Root Cause:** Insecure remote access from vendor (not isolated)
- **PCI-DSS Failure:** Requirement 1 (firewall), Requirement 7 (access control)
- **Fine:** $18.5 million settlement

### Home Depot (2014)
- **What Happened:** 56 million card numbers stolen
- **Root Cause:** Vendor credentials used by attackers
- **PCI-DSS Failure:** Requirement 2 (default passwords), Requirement 7 (access control)
- **Fine:** $19.5 million settlement

## Implementation Roadmap

### Phase 1: Assessment (Month 1-2)
- Perform gap analysis against PCI-DSS
- Document current security posture
- Identify required changes

### Phase 2: Planning (Month 2-3)
- Create detailed implementation plan
- Allocate budget and resources
- Assign responsibility

### Phase 3: Implementation (Month 3-6)
- Update firewall rules
- Implement encryption
- Deploy access controls
- Install monitoring
- Update policies

### Phase 4: Testing (Month 6-7)
- Perform vulnerability scans
- Test access controls
- Verify logging
- Conduct penetration test

### Phase 5: Compliance & Maintenance (Month 7+)
- Complete compliance assessment
- Submit to payment card company
- Maintain controls
- Conduct quarterly reviews
- Test annually

---

## Key Takeaways

✅ PCI-DSS is legally required for organizations handling credit cards
✅ 12 requirements span network security, data protection, access control, and monitoring
✅ Compliance requires both technical controls and policies
✅ Regular testing and monitoring are ongoing requirements
✅ Breaches result in fines, reputation damage, and legal liability
✅ Compliance is a baseline—additional security measures often needed

---

## Resources

- Official PCI-DSS Standard: https://www.pcisecuritystandards.org/
- Qualified Security Assessor Directory: https://www.pcisecuritystandards.org/assessors_and_solutions/qualified_security_assessors/
- Self-Assessment Questionnaires: https://www.pcisecuritystandards.org/assessors_and_solutions/self_assessment_questionnaires/
