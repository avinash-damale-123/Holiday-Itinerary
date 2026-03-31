# Project Scope

## Objective
Transform Holiday Itinerary into a complete **Non-Air Travel** operations + commercial platform for Satguru Travels.

## Scope Boundary
### Included
- Non-air tourism services lifecycle
- Internal branch-led query operations
- Quote creation and approval
- Booking conversion and servicing
- Finance baseline and operational analytics

### Excluded
- Air ticketing workflows
- Airline PNR/ticket fulfillment

## Primary Functional Domains
1. Authentication and access control
2. Admin + dynamic masters
3. User management for routing/escalation
4. Query management
5. Supplier RFQ and sourcing
6. Quote builder
7. Approvals
8. Booking lifecycle orchestration
9. Finance module
10. Analytics

## Phase-1 Locked Masters
- Type of Services
- Countries
- Branch Master
- Service-wise SLA/TAT

## Critical Business Rules
- Admin opens on Master tab
- Assignment fallback to branch default user
- Parent Query ID links inter-branch records
- Business-hours SLA with shift/timezone and Sunday exclusion
- Explicit status models for query/quote/booking
- Quote and booking remain separate lifecycle aggregates

## Required Outputs
- Application skeleton
- Schema + migrations
- Seed data
- Core API set
- Initial UI views
- Business and technical documentation
