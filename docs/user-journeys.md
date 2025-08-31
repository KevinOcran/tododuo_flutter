# User Journeys

This document outlines detailed step-by-step flows for each user role: Buyer, Seller, Manager, Admin.  
Each journey describes entry points, actions, and expected outcomes.

---

## 1. Buyer Journey

**Primary Goal**: Discover products → Purchase → Receive order and receipt.

1. Open the app → presented with **Login/Signup** (Firebase Auth).
2. Authenticate → redirected to **Home Feed** (Firestore query).
3. Browse/search products → filter by category/price/location.
4. Select a product → view details, seller profile, reviews.
5. Add to **Cart** → Firestore stores cart items.
6. Proceed to **Checkout**:
   - Confirm delivery address (location field).
   - Select payment method (mobile money, card, wallet).
7. Complete payment → Payment Gateway → Cloud Function updates **Order Status** = "Paid".
8. Buyer receives **confirmation + digital receipt** (Firestore + Cloud Storage).
9. Order enters **Dispatch & Delivery** workflow → buyer tracks progress.
10. Buyer confirms delivery → feedback option enabled.

---

## 2. Seller Journey

**Primary Goal**: Manage product listings → Fulfill orders → Get paid.

1. Open app → **Login/Signup** (role = Seller).
2. On dashboard, view:
   - Current listings.
   - Pending orders.
   - Notifications.
3. Add/Edit products:
   - Upload product images (Cloud Storage).
   - Enter details (name, price, stock, category).
   - Firestore saves product.
4. Receive order notification → confirm stock.
5. Accept order → update status to "Preparing/Dispatched".
6. Handover to delivery/logistics → status updated in Firestore.
7. Payment release triggered after buyer confirms receipt.
8. Seller views **transaction history** + receipts.

---

## 3. Manager Journey

**Primary Goal**: Operational oversight, approvals, dispute resolution.

1. Login via **Dashboard**.
2. Access overview panel:
   - Active buyers & sellers.
   - Disputed orders.
   - Pending seller approvals.
3. Approve/reject seller onboarding requests.
4. Review flagged/disputed transactions.
5. Intervene in disputes (update order status, initiate refunds).
6. Escalate policy violations to Admin.

---

## 4. Admin Journey

**Primary Goal**: Full system governance, security, and compliance.

1. Login with **Admin credentials** (highest role).
2. Access full control dashboard:
   - Manage Managers, Sellers, Buyers.
   - Create/modify user groups.
   - Define system policies (data retention, security rules).
3. Audit system logs (login attempts, suspicious activity).
4. Update global settings (commission %, retention times, payment configs).
5. Override manager decisions if required.
6. Generate compliance reports (financial + operational).

---

# End of User Journeys

