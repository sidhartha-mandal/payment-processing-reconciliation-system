# System Architecture

## 1. Architecture Style

The Enterprise Payment Processing & Reconciliation System follows a **Modular Monolithic Architecture** with a layered application design.

The application runs as a single Spring Boot application, while its business functionalities are separated into independent logical modules.

## 2. High-Level Architecture

The system consists of the following layers:

1. **Client Layer:** Sends HTTP requests through REST APIs.
2. **Controller Layer:** Receives requests and returns HTTP responses.
3. **Service Layer:** Handles business logic and transaction processing.
4. **Repository Layer:** Communicates with the database using Spring Data JPA.
5. **Database Layer:** Stores customers, payments, refunds, ledger entries, and reconciliation records.
6. **Payment Provider Simulator:** Simulates external payment gateway responses and settlement data.

## 3. Main Business Modules

| Module | Responsibility |
|---|---|
| Customer | Customer registration and management |
| Authentication | Login, JWT and role-based access |
| Payment | Payment creation and status tracking |
| Provider | Simulated external payment processing |
| Refund | Full and partial refunds |
| Ledger | Double-entry financial records |
| Reconciliation | Comparison of internal and external transactions |
| Exception Management | Tracking reconciliation discrepancies |
| Admin | Reports and administrative operations |

## 4. Payment Processing Flow

1. Client sends a payment request.
2. Controller receives and validates the request format.
3. Service layer applies business rules.
4. System checks the idempotency key to prevent duplicate processing.
5. Payment record is created with an initial status.
6. Simulated payment provider processes the request.
7. Payment status is updated based on the provider response.
8. Final response is returned to the client.

## 5. Reconciliation Flow

1. System retrieves internal payment and ledger records.
2. External settlement data is imported.
3. Reconciliation engine compares internal and external records.
4. Transactions are classified based on the comparison.
5. Discrepancies are recorded for investigation.
6. Admin users can review reconciliation results.

## 6. Reconciliation Result Types

- **MATCHED:** Internal and external transaction records match.
- **AMOUNT_MISMATCH:** Transaction amounts differ.
- **MISSING_EXTERNAL:** Internal transaction exists, but external settlement record is missing.
- **MISSING_INTERNAL:** External settlement record exists, but internal transaction is missing.

## 7. Architectural Decision

A modular monolith is selected instead of microservices to keep deployment and infrastructure manageable while maintaining clear separation between business modules.

This architecture is suitable for a portfolio project and can be evolved as requirements grow.
