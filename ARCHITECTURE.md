# Architecture

## High-Level Style
- Start as a **modular monolith** with strict domain boundaries.
- Keep services, DTOs, repositories, and API contracts separated.
- Expose versioned APIs under `/api/v1/*`.

## Suggested Stack
- Frontend: React/Next.js
- Backend: NestJS (or Spring Boot equivalent)
- Database: PostgreSQL
- Cache: Redis
- Jobs/Queue: BullMQ/RabbitMQ
- Object storage abstraction for attachments

## Core Domains
- Identity/Organization
- Master Management (dynamic framework)
- Query Operations
- Supplier RFQ
- Quote Management
- Booking Management
- Finance
- Analytics
- Audit/Notification

## Core Entity Buckets
- Identity: Organization, Branch, User, Role, Permission, UserPermission
- User Mapping: UserServiceMapping, UserEscalationMatrix
- Masters: MasterDefinition, MasterColumn, MasterItem, MasterItemValue
- Query: Query, QueryMessage, QueryAttachment, QueryAssignmentHistory, QueryEscalationEvent, QueryStatusHistory, QueryAuditEvent
- Quote: QuoteRequest, Quote, QuoteVersion, QuoteItem, QuoteItemPricing, QuoteApproval, QuoteAcceptance
- Supplier: Supplier, SupplierRFQ, SupplierOffer, SupplierNegotiationMessage, SupplierAttachment
- Booking: Booking, BookingItem, BookingPolicySnapshot, BookingDocument, Amendment, Cancellation, Refund
- Finance: Invoice, CreditNote, Payment, RefundTransaction, FXRateSnapshot
- Common: AuditEvent, Notification, NotificationTemplate, ActivityLog

## API Design Principles
- Versioned route groups
- Consistent pagination/filter/sort
- Server-side validation everywhere
- Idempotency for sensitive financial/booking operations
- Audit metadata on key writes

## Security Baseline
- RBAC + object-level authorization
- Attachment access controls
- Audit logs for privileged operations
- Minimal payment data exposure
- Privacy-aware retention-ready structure

## SLA/Escalation Engine Notes
- Business-time evaluator (shift + timezone aware)
- Sunday exclusion
- Configurable holiday calendar extension
- Escalation ladder: L1/L2/L3/L4 based on service TAT multipliers
