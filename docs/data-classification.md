# Data Classification

This document enumerates all major data fields in the system and assigns a classification for security, privacy, and compliance handling.

---

## 🔑 Classes

- **PII (Personally Identifiable Information)** → Must be encrypted in transit & at rest, strict retention policy.  
- **Financial** → Must be PCI-DSS aligned, never stored in plaintext.  
- **Location** → Can be sensitive, requires anonymization for analytics.  
- **Public** → Safe for display/logging without restriction.  
- **Derived** → Aggregated or anonymized values, low risk.

---

## 🗂️ Field Mapping

| Field                  | Classification | Notes |
|-------------------------|----------------|-------|
| Full Name              | PII            | Needed for account & receipts |
| Email Address          | PII            | Unique login, must be verified |
| Phone Number           | PII            | Optional for notifications, masked in DB |
| Government ID (if used) | PII            | Optional verification, retain 180 days |
| Password (hash only)   | PII            | Never store plaintext |
| Payment Card Number    | Financial      | Use tokenization, never persist |
| Transaction Records    | Financial      | Store securely, audit logs immutable |
| Cart Items             | Public/Derived | Non-sensitive, linked to user |
| Delivery Address       | Location + PII | Encrypt at rest, purge after delivery (30 days) |
| GPS Coordinates        | Location       | Round or anonymize for analytics |
| Purchase History       | Derived        | Used for recommendations |
| Product Listings       | Public         | Visible to all users |
| Seller Ratings         | Public/Derived | Derived from transactions |

---

## 📦 Retention Policies

- **PII**: Retain for active account; purge within 30 days of account deletion.  
- **Financial**: Retain transaction records 7 years (audit/legal), anonymize card data immediately.  
- **Location**: Retain 30 days max (dispatch/dispute resolution), anonymize thereafter.  
- **Public**: No restrictions, safe for indefinite retention.  
- **Derived**: Retain indefinitely for analytics/insights.  

---

## ✅ Compliance Notes

- Encryption: TLS 1.3 for transit, AES-256 at rest.  
- Logging: No PII or card data in logs.  
- Backups: Encrypted, rotated every 30 days.  
- Access Control: RBAC enforced for all sensitive data.  

---
🔗 **Next:** See [Data Retention Policies](./data-retention-policies.md) for how long each data type is stored and when it is deleted.

