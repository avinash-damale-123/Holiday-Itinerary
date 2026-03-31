# Codex Instruction Set

## Project
Holiday Itinerary / Non-Air Travel Quotation, Query, and Booking Portal (Satguru Travels)

## 1) Project Context
Build a **web-based Non-Air Travel Quotation and Booking Portal** using the Holiday Itinerary requirement baseline plus the additional reference for end-to-end quotation and booking workflows.

### In Scope (Non-Air only)
- Hotels
- Packages
- Visa
- Car Rental
- Cruise
- Insurance
- MICE
- Sightseeing
- Train/Rail
- Transfers/Ground Transport
- Activities/Tours
- Ferries
- Future extensible non-air services

### Out of Scope
- Air ticketing
- Airline PNR fulfillment

## 2) Target Product Shape
### A. Operational Query Workflow
- Self Query
- Inter-Branch Query (Sent)
- Inter-Branch Query (Received)
- Assignment, reassignment, escalation, SLA/TAT tracking
- Query conversation + quotation exchange
- Performance analytics

### B. Commercial Quote Workflow
- Quote request intake
- Itinerary build
- Supplier sourcing/RFQ
- Internal approvals
- Customer acceptance
- Booking conversion
- Documents, amendments, cancellations, refunds, invoicing

### C. Architecture Direction
- Modular, API-first
- Internal operations + future customer/supplier/partner channels
- Future integrations: inventory, payments, CRM, ERP, analytics

## 3) Core Business Model
- Branch-led operations model
- Routing depends on service, destination, branch ownership, user-service mapping, branch fallback/default user, escalation hierarchy, shift, timezone
- Commercial flow has 2 distinct phases:
  1. Build & price quote
  2. Convert accepted quote to booking/fulfillment
- **Quote state and booking state must remain separate business objects**

## 4) Mandatory Modules
1. Authentication + RBAC/permissions + auditability (OIDC-ready)
2. Admin module (Master, User Management, Activity Tracking, User Circle placeholder)
3. Dynamic Master Framework (config-driven, not hardcoded)
4. User Management (routing + escalation critical fields)
5. Query Management (operational engine)
6. Supplier RFQ/Sourcing Console
7. Quote Builder
8. Approvals
9. Booking Orchestration
10. Finance
11. Analytics

## 5) Locked Phase-1 Data
### Services
- Car Rental
- Cruise
- Hotel
- Insurance
- MICE
- Packages
- Sight Seeing
- Train
- Visa
- Other

### Base TAT (hours)
- Car Rental: 4
- Cruise: 48
- Hotel: 4
- Insurance: 4
- MICE: 48
- Packages: 24
- Sight Seeing: 8
- Train: 4
- Visa: 48
- Other: 8

### Countries Master Columns
- Country Name
- ISO-2
- ISO-3
- Dialing Code

### Branch Master
Acts as both data master and routing master for user mapping, branch visibility, targeting, analytics, and routing logic.

## 6) Query Routing Rules
- Source Branch autofilled from logged-in user
- Self Query: no target branch at creation
- Inter-Branch Sent: target branch mandatory
- Service from Services Master
- Destination country from Countries Master
- Assignment key: service + target branch + destination country (where applicable)
- If no mapped user: fallback to branch 1st/default user
- Inter-Branch Received must preserve Parent Query ID linkage
- Reassignment must be restricted + audited + update last action timestamps

## 7) SLA / Escalation Rules (Business Time)
Escalation thresholds:
- L1 = base TAT
- L2 = 2× base TAT
- L3 = 4× base TAT
- L4 = 8× base TAT

Business-time computation:
- Count only inside assigned user's shift
- Respect assigned user's timezone
- Carry over remainder to next working day
- Exclude Sundays
- Keep holiday calendar pluggable/configurable

Escalation behavior:
- L2-L4 primarily for alerting/visibility unless explicitly configured as executors later

## 8) Status Models
### Query Status
- New, Acknowledged, In Progress, Waiting Client, Waiting Branch, Completed, Closed

### Query Escalation Level
- L1, L2, L3, L4

### Quote Status
- Draft, Pending Approval, Approved, Sent to Customer, Accepted, Rejected, Expired, Converted to Booking, Closed

### Booking Status
- Draft, Pending Confirmation, On Hold, Confirmed, Partially Confirmed, Failed, Amended, Cancelled, Refund Pending, Refunded

## 9) Pricing / Finance Rules
Support:
- Net/Gross pricing
- Commissionable vs non-commissionable
- Markup/commission engine
- Multi-currency + FX snapshots (quote-time + booking-time)
- Taxes/fees
- Min/max margin thresholds + approval triggers

Minimum quote pricing model fields:
- Item net
- Item sell
- Applied markup rule
- Currency
- Taxes
- Total margin
- Discount reason
- Approval reference

## 10) UX Expectations
- Operational dashboard with KPI cards
- Search/filter lists for queries and quotes
- Status/priority/escalation badges
- Detailed case pages with conversation and timeline
- Strong audit visibility
- Attachment handling
- Role-driven controls

Key views:
- Admin > Master
- Admin > User Management
- Admin > Activity Tracking
- Query Dashboard + Query Detail
- Quote Builder
- Supplier RFQ Console
- Approval Queue
- Booking Console
- Finance Console
- Analytics Dashboard

## 11) Technical Direction
Preferred approach: **modular monolith first**, with clear bounded contexts.

Suggested stack (if repo permits):
- Frontend: React/Next.js
- Backend: Node.js (NestJS) or Java/Kotlin (Spring Boot)
- DB: PostgreSQL
- Cache: Redis
- Jobs/Queue: BullMQ/RabbitMQ
- File storage abstraction
- Auth: JWT/session now, OIDC-ready later
- API contracts: OpenAPI-first

## 12) Domain Model Guidance
Identity/Org:
- Organization, Branch, User, Role, Permission, UserPermission, UserServiceMapping, UserEscalationMatrix

Masters:
- MasterDefinition, MasterColumn, MasterItem, MasterItemValue

Query Domain:
- Query, QueryMessage, QueryAttachment, QueryAssignmentHistory, QueryEscalationEvent, QueryStatusHistory, QueryAuditEvent

Quote Domain:
- QuoteRequest, Quote, QuoteVersion, QuoteItem, QuoteItemPricing, QuoteApproval, QuoteAcceptance

Supplier Domain:
- Supplier, SupplierRFQ, SupplierOffer, SupplierNegotiationMessage, SupplierAttachment

Booking Domain:
- Booking, BookingItem, BookingPolicySnapshot, BookingDocument, Amendment, Cancellation, Refund

Finance:
- Invoice, CreditNote, Payment, RefundTransaction, FXRateSnapshot

Audit & Notifications:
- AuditEvent, Notification, NotificationTemplate, ActivityLog

## 13) API Route Groups
- `/api/v1/auth`
- `/api/v1/admin/masters`
- `/api/v1/admin/users`
- `/api/v1/queries`
- `/api/v1/quotes`
- `/api/v1/rfqs`
- `/api/v1/approvals`
- `/api/v1/bookings`
- `/api/v1/finance`
- `/api/v1/analytics`
- `/api/v1/audit`

Include pagination, filtering, sorting, audit metadata, optimistic validation, and idempotency for sensitive operations.

## 14) Security & Compliance Minimums
- RBAC and object-level authorization
- Server-side validations
- Audit logs for sensitive actions
- Attachment access control
- Minimal payment-data exposure
- Privacy-aware data modeling and retention-ready structure
- Never expose admin/master maintenance routes to unauthorized users

## 15) Delivery Order
### Phase A (Foundation)
1. Auth + roles
2. Branch/user/master framework
3. Admin module
4. Activity tracking
5. Dynamic master system

### Phase B (Operational Core)
6. User management routing fields
7. Query create/list/detail/update
8. Query assignment logic
9. Escalation timer engine
10. Dashboard + baseline analytics

### Phase C (Commercial Extension)
11. Quote request + quote builder
12. Supplier RFQ
13. Approvals
14. Booking conversion model
15. Documents + finance baseline

### Phase D (Advanced)
16. Amendments/cancellations/refunds
17. Rich analytics
18. External integrations
19. Customer portal
20. Supplier portal/extranet

## 16) Non-Negotiables
- No air ticketing
- Admin must open on Master tab
- Master framework config-driven
- Branch Master is routing-critical
- User setup controls routing and escalation
- Fallback assignment to branch default user is mandatory
- Parent Query ID linkage mandatory across branches
- SLA must use business hours + shift + timezone + Sunday exclusion
- Explicit valid query statuses and escalation levels always
- Quote and booking lifecycles are separate

## 17) Expected Deliverables
1. Production-grade app skeleton
2. Schema + migrations
3. Seeders for locked master data
4. Admin/master/user/query modules
5. SLA escalation service/job
6. REST APIs with validation
7. Initial frontend screens
8. Documentation: setup, architecture, business rules, API, assumptions/open points

## 18) Configurable Open Points
- Single vs multi-select service type (phase-1)
- Approval thresholds
- Escalation notification behavior
- Whether escalated levels become executors later
- Holiday calendar policy
- External supplier/API connectors
- Hold vs instant confirmation behavior by provider
- Internal-only vs customer-facing rollout sequencing
