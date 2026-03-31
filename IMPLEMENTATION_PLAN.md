# Implementation Plan

## Phase A — Foundation
1. Authentication + authorization
2. Branch/user/master base framework
3. Admin module shell
4. Activity tracking baseline
5. Dynamic master framework

### Exit Criteria
- Login + RBAC functional
- Admin > Master default tab behavior implemented
- Locked masters manageable via config-driven framework

## Phase B — Operational Core
6. User management with routing/escalation fields
7. Query create/list/detail/update APIs + UI
8. Assignment logic with fallback to branch default user
9. SLA/TAT business-time escalation job/service
10. Query dashboard + baseline analytics

### Exit Criteria
- End-to-end query flow works for self and inter-branch modes
- Parent Query ID linkage works across sent/received records
- Escalation events generated correctly by business hours

## Phase C — Commercial Extension
11. Quote request + quote builder + versioning
12. Supplier RFQ/sourcing console
13. Approval flow by configurable policy thresholds
14. Quote-to-booking conversion with separate booking lifecycle
15. Document + finance baseline (invoice/receipt/refund tracking)

### Exit Criteria
- Accepted quote can convert to booking lifecycle
- Approval gating works for threshold cases
- Finance entities linked to booking and quote context

## Phase D — Advanced Capabilities
16. Amendment/cancellation/refund workflows
17. Advanced analytics and reliability KPIs
18. External integrations (supplier, payment, CRM/ERP)
19. Customer portal rollout
20. Supplier extranet rollout

### Exit Criteria
- Post-booking servicing lifecycle fully modeled
- Integration contracts documented and testable
- Reporting supports both operational + commercial KPIs

## Testing Expectations (ongoing)
- Unit tests: routing, escalation, status transitions
- Integration tests: query create/assign/escalate
- Contract/API tests: critical workflow routes
- Seed data validation for services/TAT/branches/countries/roles
