+++
title = "Veylor Relay"
description = "High-performance, multi-tenant notification routing proxy and PII isolation vault for the Veylor ecosystem."
weight = 0
[extra]
link_to = "https://github.com/veylor-hq/relay"
+++

# Veylor Relay — PII Isolation Vault & Notification Proxy

Veylor Relay is a high-performance, multi-tenant notification routing proxy and PII isolation vault. It serves as a centralized privacy boundary across the Veylor service ecosystem—including the core **eGarage GMS** (Garage Management System) and auxiliary billing, booking, and inventory services—to minimize data protection compliance risks.

By decoupling Personally Identifiable Information (PII) from primary client-facing nodes and tokenizing recipient details into an identity-decoupled `recipient_id` (UUID), Veylor Relay significantly reduces the security compliance surface area of downstream microservice infrastructure.

## Key Features

- **Recipient Data Minimization & Tokenization**: Client applications synchronize contact information once, receive a unique `recipient_id` (UUID), and eliminate plain-text PII storage on downstream systems.
- **Double-Read Body Caching**: Employs a custom `HttpServletRequestWrapper` to allow multiple body reads (for signature verification and controller parsing).
- **HMAC-SHA256 Request Verification**: Validates webhook authenticity via signature checks, storing only hashed API keys in the database.
- **Idempotency Protection**: Enforces unique transaction routing via `X-RELAY-Nonce` headers to prevent duplicate sends on network retries.
- **Java Virtual Threads Async Delivery**: Offloads bulk notification sends to lightweight Java Virtual Threads for concurrent, non-blocking SMTP delivery.
- **Tenant Management (Admin CRUD)**: Administrative endpoints for registering, updating, and managing third-party applications and tenant profiles.

## Security & Operational Controls

- **Retention & Purge Policies**: Mandates that downstream clients purge local plain-text PII copies once tokenized, while Relay enforces strict database retention schedules.
- **Strict Access Control**: Zero-trust API key management for tenant isolation and administrative operations.
- **Sanitized Logging**: Prevents logging payload contents or plain-text PII in system logs.
