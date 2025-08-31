# Threat Model — Marketplace App (Stage 0.2)

This document applies STRIDE threat modeling to key flows in the system.  
It also integrates lifecycle practices such as continuous threat modeling, risk prioritization, supply chain dependency awareness, monitoring, and user education.

---

## Threat Modeling in Development Lifecycle
- **Integration:** Threat modeling is embedded into architecture reviews, CI/CD pipelines, and runtime checks.  
- **Automation:** Security validation (unit + integration tests) explicitly covers STRIDE threats (e.g., token tampering, webhook replay).  
- **Iteration:** Models are reviewed every sprint and updated with system changes.  

---

## 1. Authentication & Role Management

**Spoofing**
- Threat: Attacker tries to impersonate a system admin through phishing, credential stuffing, or device compromise.
- Scenarios: Social engineering against admins; stolen refresh token reuse.
- Mitigation: Firebase Authentication with MFA; device fingerprinting; user education on phishing defense.

**Tampering**
- Threat: Manipulating JWT tokens or OAuth scopes.
- Scenarios: Attacker modifies local token payload to escalate privileges.
- Mitigation: Signed JWTs with short expiry, API Gateway signature validation.

**Repudiation**
- Threat: User denies creating/assigning roles.
- Mitigation: Immutable, cryptographically signed logs with non-repudiation guarantees.

**Information Disclosure**
- Threat: Unauthorized access to role/privilege assignments.
- Mitigation: Encrypt role data at rest and in transit; strict RBAC queries; admin-only dashboards.

**Denial of Service**
- Threat: Flooding login endpoint with bot traffic.
- Mitigation: Rate limiting, CAPTCHA, anomaly detection via API Gateway.

**Elevation of Privilege**
- Threat: Seller escalates to manager/admin via crafted API calls.
- Mitigation: RBAC enforced server-side; penetration tests for privilege escalation; anomaly alerts on role changes.

---

## 2. Buyer → Browsing & Checkout

**Spoofing**
- Threat: Fake buyer accounts for promo abuse or scraping.
- Scenarios: Automated bot signups with disposable emails.
- Mitigation: Email/phone verification, recaptcha, velocity checks.

**Tampering**
- Threat: Buyer manipulates cart payload to alter price.
- Mitigation: Server-side price & discount validation; checksum signing of cart payloads.

**Repudiation**
- Threat: Buyer denies placing order.
- Mitigation: Device fingerprint + behavioral biometrics tied to orders; signed order confirmation logs.

**Information Disclosure**
- Threat: Buyer accesses another buyer’s order history.
- Mitigation: Firestore row-level rules by userID; DAST scans for access control gaps.

**Denial of Service**
- Threat: Aggressive scraping or bots empty inventory.
- Mitigation: Rate limiting, API Gateway throttling, monitoring unusual catalog access.

**Elevation of Privilege**
- Threat: Buyer manipulates token scopes to access seller APIs.
- Mitigation: API Gateway enforces scope validation; penetration tests simulate escalation attempts.

---

## 3. Seller → Listing & Order Management

**Spoofing**
- Threat: Attacker impersonates seller to steal order flow.
- Mitigation: KYC onboarding; MFA; device checks.

**Tampering**
- Threat: Seller edits confirmed orders.
- Mitigation: Orders immutable post-confirmation; signed, timestamped receipts.

**Repudiation**
- Threat: Seller denies removing a listing or changing inventory.
- Mitigation: Immutable audit logs with cryptographic signatures.

**Information Disclosure**
- Threat: Seller views another seller’s buyer data.
- Mitigation: Strict partitioning by sellerID in database; RBAC-enforced queries.

**Denial of Service**
- Threat: Seller floods system with spam listings.
- Mitigation: Automated listing review; rate limits; anomaly detection.

**Elevation of Privilege**
- Threat: Seller abuses APIs to assign themselves as admin.
- Mitigation: Role changes only via admin service account; red-team testing.

---

## 4. Payment & Webhooks

**Spoofing**
- Threat: Attacker sends fake payment webhook.
- Mitigation: HMAC verification; allowlist IP ranges; API Gateway enforcement.

**Tampering**
- Threat: Replay attack on webhook for double fulfillment.
- Mitigation: Idempotency keys, timestamp validation, signed receipts.

**Repudiation**
- Threat: User denies payment despite completion.
- Mitigation: Store PSP (Payment Service Provider) receipts with non-repudiation.

**Information Disclosure**
- Threat: Payment tokens logged or exposed.
- Mitigation: Mask sensitive fields in logs; encrypt data at rest.

**Denial of Service**
- Threat: Attacker floods webhook endpoint.
- Mitigation: Early signature validation, rate limiting, autoscaling + anomaly alerts.

**Elevation of Privilege**
- Threat: Seller tampers webhook routing.
- Mitigation: Dedicated payment microservice; strict IAM isolation.

---

## 5. Receipts & Notifications

**Spoofing**
- Threat: Attacker sends fake “order successful” email.
- Mitigation: SPF/DKIM/DMARC; verified notification domain.

**Tampering**
- Threat: Buyer alters stored PDF receipt.
- Mitigation: Digitally sign receipts; verify signatures before rendering.

**Repudiation**
- Threat: Buyer denies receiving receipt/notification.
- Mitigation: Immutable logs of email/SMS push; delivery receipts.

**Information Disclosure**
- Threat: Publicly accessible receipt links.
- Mitigation: Generate signed, short-lived URLs; restrict access by userID.

**Denial of Service**
- Threat: Flooding push notification system.
- Mitigation: Firebase Cloud Messaging quotas, circuit breakers.

**Elevation of Privilege**
- Threat: Seller sends admin-level notifications.
- Mitigation: Notifications controlled only by admin service account.

---

## Supply Chain & Third-Party Dependencies
- Risks: Compromised Firebase SDK, payment gateway exploits, dependency poisoning in NPM/Flutter packages.
- Mitigation: Pin versions; SBOM (software bill of materials); dependency scanning (Snyk, Dependabot).

---

## Advanced Detection & Response
- Monitor unusual login/location patterns.  
- Alert on role changes or abnormal webhook retries.  
- Automated IP blocking + incident response playbooks.

---

## Logging & Forensics
- Use tamper-evident, cryptographically signed logs (e.g., hash chains).  
- Centralized SIEM for correlation across auth, payments, notifications.  
- Ensure logs are compliance-ready (GDPR, PCI DSS).  

---

## Security Testing Alignment
- Penetration testing on role escalation, webhook replay, cart tampering.  
- SAST/DAST pipelines linked to threat model.  
- Fuzzing endpoints tied to high-risk flows.  

---

## User Education & Support
- Training sellers/admins to spot phishing and social engineering.  
- In-app education prompts about suspicious links.  
- Clear escalation channels for security incidents.  

---

# Summary
This threat model expands STRIDE with lifecycle practices, supply chain risk, risk prioritization, and detection.  
**Next step:** Extract top 10 critical risks into `docs/risk-register.csv` with likelihood, impact, mitigation, and owners.

