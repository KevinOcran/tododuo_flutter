# Requirements Document

## 1. Core User Flows
### Buyer
- Browse items/services
- Add items to cart
- Checkout → payment (online / wallet / cash on delivery)
- Order confirmation
- Dispatch → delivery tracking
- Delivery confirmation
- Generate & view receipts

### Seller
- Register & create profile
- Upload/manage items/services (title, description, price, availability)
- Receive orders
- Fulfill orders → update status (processing, dispatched, delivered)
- Manage disputes/returns
- Access transaction history

### System Manager
- Manage platform operations (assign permissions, oversee sellers & buyers)
- Approve/reject seller registrations
- Resolve escalated disputes
- Monitor system activity

### System Admin
- Superuser with all privileges
- Manage system configurations
- Create & assign user groups
- Set/modify access levels
- Handle security and compliance
- Audit logs

---

## 2. Non-Functional Requirements

### Security
- Enforce strong authentication (email, phone, OAuth2 later optional)
- Role-based access control (RBAC)
- Secure data storage (encryption at rest & in transit)
- Input validation & sanitization

### Privacy
- Minimize data collection (only required fields)
- GDPR-like principles: Right to be Forgotten, Data Portability
- Restrict access to sensitive data by role

### Scalability
- Designed for growing user base (thousands → millions)
- Auto-scaling backend with Firebase + optional cloud migration
- Efficient caching of reads

### Reliability
- High uptime target (99.5%+ for MVP)
- Offline support for buyers (browse cached items, cart persists)
- Retry mechanisms for payments/dispatch updates

### Observability
- Error logging (Crashlytics for mobile)
- Basic analytics (Firebase Analytics for user behavior, optional)

---

## 3. Constraints
- MVP built on Firebase Spark (free tier limits apply)
- Cross-platform Flutter app (iOS + Android)
- Focus on speed of delivery first, optimization later

---

