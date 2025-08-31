

---

# Data Retention Policies

This document defines how long each class of data is retained in the **ToDoDuo** system, why, and what happens after expiration. Policies balance **legal compliance**, **business needs**, and **user privacy**.

---

## 1. Personally Identifiable Information (PII)

| Data Type                                 | Retention Period                 | Rationale                               | Disposal Method                         |
| ----------------------------------------- | -------------------------------- | --------------------------------------- | --------------------------------------- |
| Name, email, phone                        | Until account deletion + 90 days | Needed for account recovery and support | Secure deletion from database & backups |
| Government ID docs (if collected for KYC) | 180 days unless under dispute    | Compliance & fraud prevention           | Secure deletion and purge from logs     |
| Address (billing/shipping)                | 1 year after last transaction    | Required for dispute handling, audits   | Anonymization & purge                   |

---

## 2. Financial Data

| Data Type                            | Retention Period            | Rationale                     | Disposal Method          |
| ------------------------------------ | --------------------------- | ----------------------------- | ------------------------ |
| Payment tokens (via Stripe/Firebase) | Until subscription canceled | Needed for recurring payments | Token revocation         |
| Transaction receipts                 | 7 years                     | Accounting & tax compliance   | Archived in cold storage |
| Refund/dispute records               | 3 years                     | Legal & compliance            | Secure deletion          |

---

## 3. Location Data

| Data Type                         | Retention Period      | Rationale                      | Disposal Method |
| --------------------------------- | --------------------- | ------------------------------ | --------------- |
| Delivery addresses                | 1 year after delivery | Required for delivery disputes | Anonymization   |
| Real-time GPS (delivery tracking) | 30 days               | Ops + analytics only           | Auto-delete     |

---

## 4. Public Data

| Data Type         | Retention Period     | Rationale              | Disposal Method   |
| ----------------- | -------------------- | ---------------------- | ----------------- |
| Product listings  | Until seller deletes | Marketplace visibility | Immediate removal |
| Reviews & ratings | Until user deletes   | Community trust        | Hard delete       |

---

## 5. Derived / Analytics Data

| Data Type            | Retention Period    | Rationale                   | Disposal Method     |
| -------------------- | ------------------- | --------------------------- | ------------------- |
| User activity logs   | 90 days             | Debugging & fraud detection | Auto-purge          |
| Aggregated analytics | Indefinite (no PII) | Product improvement         | None (safe to keep) |

---

## 6. Backup & Disaster Recovery

* Full system backups retained for **30 days**
* Backups auto-purged beyond 30 days
* Encrypted storage with access logging

📖 This document is part of the [Data Classification Framework](./data-classification.md).  

---
