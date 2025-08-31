# Architecture Overview

This document provides a high-level view of the system’s architecture, major components, and how data flows between them. The goal is to ensure scalability, security, and clarity in role responsibilities.

---

## Core Components

### 1. Client Applications
- **Flutter Mobile App** (iOS + Android):
  - Unified codebase built with Flutter/Dart.
  - Provides user interface for **Buyers** and **Sellers**.
  - Handles login, browsing, cart management, orders, payments, chat/notifications.
- **Admin & Manager Dashboard** (Web-based):
  - Built in Flutter Web (or React alternative if needed).
  - Restricted to **System Managers** and **System Admins**.
  - Features: user management, dispute resolution, analytics, escalations.

---

### 2. Backend Services (Firebase Spark Plan)
- **Firebase Authentication**: Handles login/signup with email, phone, or Google.
- **Cloud Firestore**: Stores user profiles, products, carts, orders, delivery statuses.
- **Cloud Storage**: Stores product images, receipts, verification docs.
- **Cloud Functions**:
  - Secure serverless logic.
  - Order validation, payment triggers, notification dispatch.
- **Firebase Hosting**: Serves admin dashboard (if web-based).
- **Firebase Security Rules**: Enforce role-based access control and data privacy.

---

### 3. External Integrations
- **Payment Gateway** (Stripe/Flutterwave/Mobile Money).
- **Dispatch/Logistics API** (if third-party delivery services are used).
- **Analytics/Crashlytics**: Firebase + optional GA for user insights.

---

## Roles and Access Levels

- **System Admin**: Full control; manages roles, policies, platform-wide settings.
- **System Manager**: Operational oversight; reviews sellers, approves listings, monitors disputes.
- **Seller**: Manages product listings, prices, orders received, and chats with buyers.
- **Buyer**: Browses products, adds to cart, pays, tracks delivery, and receives receipts.

---

## Data Flow (High-Level)

[Buyer App] <----> [Firestore / Storage]
| ^
v |
[Payment Gateway] ----> [Cloud Functions] <---- [Seller App]
|
v
[Admin/Manager Dashboard]


1. **Buyer** logs in → requests data from Firestore (products, sellers).
2. Buyer adds items to cart → Firestore updates.
3. Buyer initiates payment → Payment Gateway confirms → Cloud Functions updates Order Status.
4. Seller notified → Accepts/dispatches order.
5. Buyer tracks order → Delivery updates sync back into Firestore.
6. Managers/Admins oversee disputes or escalations.

---

## Non-Functional Considerations
- **Scalability**: Firestore auto-scales with usage.
- **Security**: Authentication + granular Security Rules per role.
- **Resilience**: Cloud Functions retries on failure.
- **Data Privacy**: Sensitive PII is stored securely; retention policies apply.

___
