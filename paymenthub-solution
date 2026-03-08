# ABC-NPH-001 – ABC Neo Bank | Greenfield Payment Hub | Phase 1 Target State Architecture

# Solution Architecture Description

| Field | Value |
|-------|-------|
| **Architect / Document Author** | Solution Architect |
| **Date** | 08/03/2026 |
| **Version** | 0.1 |
| **Stage** | DRAFT |
| **Status** | PENDING |
| **Information Classification** | Restricted |
| **Template Version** | V1.0 |

---

## Document Purpose

The purpose of this Solution Architecture Description (SAD) is:

1. **DIRECT**: To enable engineering to produce mid-level and detailed design for the ABC Neo Bank greenfield Payment Hub.
2. **BRIDGE**: To describe the dependencies and relationships with Core Banking, Fraud & AML, Digital Channels, and external payment scheme infrastructure.
3. **GOVERN**: To demonstrate to the FCA, internal risk committees, and programme governance how architecture concerns — security, resilience, data, operations — have been addressed.

---

## Document History

| Version | Date | Description |
|---------|------|-------------|
| 0.1 | 2026-03-08 | Initial draft. Target state architecture covering all five payment rails and cards issuing. |

---

## Document Roles

| Name | Role | Title |
|------|------|-------|
| TBD | Author | Solution Architect |
| TBD | Reviewer | Head of Architecture |
| TBD | Reviewer | Security Architect |
| TBD | Reviewer | Data Architect |
| TBD | Reviewer | Infrastructure Architect |
| TBD | Reviewer | Chief Risk Officer |
| TBD | Informed | CTO |
| TBD | Informed | Head of Payments |
| TBD | Informed | Compliance Director |

---

## Supporting Documentation

| Reference | Version | Document |
|-----------|---------|----------|
| REF-01 | TBD | Business Glossary |
| REF-02 | TBD | Architecture Principles |
| REF-03 | TBD | Data Retention Schedule |
| REF-04 | TBD | Data Governance Policy |
| REF-05 | TBD | Information Security Policy |
| REF-06 | TBD | Technical Debt and Change Management Policy |
| REF-07 | TBD | Data Protection Policy (UK GDPR) |
| REF-08 | TBD | Security Operations Logging Standards |
| REF-09 | TBD | Security NFRs |
| REF-10 | TBD | Data Protection Impact Assessment |
| REF-11 | TBD | Risk Management Framework |
| REF-12 | TBD | FPS Scheme Rules (Pay.UK) |
| REF-13 | TBD | BACS Scheme Rules (Vocalink) |
| REF-14 | TBD | CHAPS Participation Rules (Bank of England) |
| REF-15 | TBD | EPC SEPA Rulebooks (SCT, SCT Inst, SDD) |
| REF-16 | TBD | PCI-DSS v4.0 Compliance Framework |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Architecture Drivers](#2-architecture-drivers)
3. [Decisions](#3-decisions)
4. [Risks and Technical Debt](#4-risks-and-technical-debt)
5. [Application Architecture](#5-application-architecture)
6. [Business Architecture](#6-business-architecture)
7. [Data Architecture](#7-data-architecture)
8. [Infrastructure Architecture](#8-infrastructure-architecture)
9. [Security Architecture](#9-security-architecture)
10. [Operations](#10-operations)
- [A. Glossary](#a-glossary)
- [B. Appendix](#b-appendix)

---

# 1 Introduction

## 1.1 Purpose

ABC Neo Bank is a greenfield digital bank seeking FCA authorisation and full banking licence. The bank has no existing payment infrastructure. This initiative designs and delivers a cloud-native, event-driven, multi-rail Payment Hub capable of processing payments across all major UK and European schemes: Faster Payments (FPS), BACS, CHAPS, SEPA (SCT, SCT Inst, SDD), and debit Card Issuing via Visa and Mastercard.

This document covers the target state architecture for the Payment Hub. It defines the system structure, integration patterns, data flows, infrastructure strategy, security controls, and operational model. It is the primary architecture governance artefact for the Payment Hub programme and is the basis for all downstream detailed design.

This document is written for Phase 1 of a phased delivery (FPS + BACS foundation), with the target state spanning all five rails across three phases.

---

## 1.2 Objectives

| Objective | Description |
|-----------|-------------|
| OBJ-01: Multi-rail payment processing | Enable ABC Neo Bank to process payments across FPS, BACS, CHAPS, SEPA (SCT/SCT Inst/SDD), and Card Issuing from a single hub. |
| OBJ-02: Real-time payment capability | Deliver sub-5-second end-to-end processing for FPS and SCT Inst rails, meeting scheme SLAs. |
| OBJ-03: Cloud-native, resilient architecture | Design for 99.95% availability with no single points of failure, using cloud-native infrastructure and active-active deployment. |
| OBJ-04: ISO 20022 native | Adopt ISO 20022 as the canonical message standard natively, avoiding legacy translation overhead. |
| OBJ-05: PCI-DSS Level 1 compliance | Ensure the card processing environment meets PCI-DSS v4.0 Level 1 requirements from day one. |
| OBJ-06: Regulatory compliance | Meet FCA, PRA, PSRs 2017, CHAPS Participation Rules, Pay.UK Scheme Rules, and EPC SEPA Rulebooks. |
| OBJ-07: Operational transparency | Provide end-to-end payment traceability, real-time monitoring, and automated reconciliation across all rails. |
| OBJ-08: Open Banking ready | Expose regulated payment initiation APIs compliant with OBIE standards and PSD2 requirements. |

---

## 1.3 Document Scope

### In Scope – This Phase (Phase 1)

| In Scope Item | Description / Rationale |
|---------------|-------------------------|
| Faster Payments (FPS) | Real-time inbound and outbound credit transfers via Pay.UK / Vocalink. Primary retail payment rail. |
| BACS | Bulk Direct Credit and Direct Debit processing via Vocalink. Required for payroll and recurring payments. |
| Payment Hub Foundation | Core orchestration engine, API gateway, event bus, payment store, audit logging, core banking adapter. |
| Core Banking Adapter | Integration layer to post payment entries to the Core Banking System ledger. |
| Fraud & AML Adapter | Integration layer for real-time pre-payment fraud and sanctions screening. |
| Operations Dashboard | Internal tooling for payment monitoring, exception management, and manual intervention. |
| CI/CD Pipeline and IaC | Infrastructure-as-code and deployment pipeline for all hub components. |

### In Scope – Overall Programme

| In Scope Item | Description / Rationale |
|---------------|-------------------------|
| CHAPS | High-value same-day RTGS via Bank of England. Phase 2. |
| SEPA SCT / SCT Inst / SDD | EU payment rails. Phase 3. |
| Card Issuing (Visa / Mastercard) | Debit card authorisation, clearing, and settlement. Phase 2. |
| Reconciliation Engine | Automated multi-rail reconciliation. Phase 1 (FPS/BACS) extended in Phase 2/3. |
| Open Banking / PSD2 APIs | Regulated third-party payment initiation and account access. Phase 2. |
| Regulatory Reporting | Automated submission to FCA and Bank of England. Phase 2. |

### Out of Scope

| Out of Scope Item | Description / Rationale |
|-------------------|-------------------------|
| Customer Onboarding / KYC | Covered by a separate onboarding programme. |
| Core Banking System build | Core Banking is a dependency — selection and build are a separate programme. |
| Fraud & AML Platform build | Platform is a dependency — selection is a separate procurement. |
| Acquiring / Merchant Services | ABC Neo Bank is not an acquirer. Cards scope is issuing only. |
| Cryptocurrency / Digital Assets | Out of scope for this programme. |
| Lending Origination | Covered by a separate lending programme. |
| Deposit / Savings Products | Covered by a separate deposits programme. |
| Swift / Cross-border (non-SEPA) | Phase 4+ — not in current programme scope. |

---

## 1.4 Solution Roadmap

| Phase | When | Description |
|-------|------|-------------|
| Phase 1 | Q4 2026 | **Scope:** Payment Hub Foundation, FPS (inbound + outbound), BACS (Direct Credit + Direct Debit), Core Banking Adapter, Fraud/AML Adapter, Operations Dashboard, Reconciliation (FPS + BACS). <br><br> **Value:** Live UK domestic payment capability; FCA go-live gate satisfied for payments. <br><br> **Dependencies:** Core Banking platform selected and integration-ready; FPS/BACS scheme access confirmed (direct or bureau); cloud environment provisioned. |
| Phase 2 | Q2 2027 | **Scope:** CHAPS connector (ISO 20022 MX), Card Issuing (Visa/Mastercard authorisation, clearing, settlement), PCI-DSS compliant card data environment, Open Banking API (PSD2/OBIE), Regulatory Reporting. <br><br> **Value:** High-value payment capability; full card product go-live; open banking compliance. <br><br> **Dependencies:** CHAPS membership or indirect access confirmed; BIN sponsorship or direct Visa/Mastercard membership; PCI-DSS Level 1 audit complete. |
| Phase 3 | Q4 2027 | **Scope:** SEPA SCT, SCT Inst, SDD Core connectors; EU entity legal entity integration; EU data residency controls. <br><br> **Value:** EU payment coverage; European customer acquisition capability. <br><br> **Dependencies:** EU legal entity operational; SEPA reachability via direct EBA membership or indirect participant; DORA compliance confirmed. |

---

## 1.5 Solution Strategy

| Strategy | Description |
|----------|-------------|
| Event-Driven, ISO 20022 Native | All internal payment events are modelled as ISO 20022 pacs/pain/camt messages on an event bus. No proprietary message formats introduced. This eliminates transformation overhead when connecting to scheme networks and future-proofs the hub as ISO 20022 adoption expands globally. |
| Microservices with Domain Decomposition | The hub is decomposed into independently deployable services aligned to payment rail domains (FPS Connector, BACS Connector, etc.) and horizontal concerns (Orchestration, Reconciliation, Audit). Services communicate via the event bus for async flows and REST for synchronous queries. |
| Cloud-Native, Infrastructure-as-Code | All infrastructure is provisioned via Terraform. No manually provisioned resources are permitted. Containerised workloads run on Kubernetes. This ensures repeatability, auditability, and rapid disaster recovery. |
| Active-Active Multi-Zone Deployment | All Tier 1 components are deployed active-active across a minimum of two availability zones within the primary cloud region, with a warm-standby in a secondary region for disaster recovery. |
| API-First | All external and internal consumers interact with the Payment Hub via versioned, OpenAPI-documented APIs. This enables clean integration with Digital Banking, Open Banking TPPs, and future channels. |
| Zero-Trust Security | No implicit trust between services. All service-to-service calls are authenticated using mutual TLS. Authorisation is enforced via RBAC at the API gateway and service mesh level. |

---

## 1.6 Solution Context

### Current State Context

There is no current state. ABC Neo Bank is a greenfield institution with no existing payment infrastructure. The current state is a manual capability gap — the bank cannot process any payments today.

### Target State Context Diagram — C4 Level 1 (System Context)

```mermaid
C4Context
    title ABC Neo Bank — Payment Hub — System Context (C4 Level 1)

    Person(retailCustomer, "Retail Customer", "Uses mobile/web banking to send and receive payments and make card purchases")
    Person(smeCustomer, "SME / Corporate Customer", "Initiates bulk BACS files, high-value CHAPS payments, and multi-currency SEPA transfers")
    Person(opsStaff, "Operations Staff", "Monitors payments, manages exceptions, and performs manual interventions via the Ops Dashboard")

    System_Boundary(abcBank, "ABC Neo Bank") {
        System(paymentHub, "Payment Hub", "Cloud-native, event-driven multi-rail payment processing platform. Processes FPS, BACS, CHAPS, SEPA, and Card Issuing transactions")
        System(digitalBanking, "Digital Banking Platform", "Customer-facing web and mobile application. Initiates and displays payment activity")
        System(coreBank, "Core Banking System", "Account and ledger management. Holds balances and posts debit/credit entries for all payment transactions")
        System(fraudAML, "Fraud & AML Platform", "Real-time transaction screening for fraud, sanctions, and financial crime risk")
    }

    System_Ext(payUK, "Pay.UK / Vocalink", "UK payment scheme infrastructure. Provides FPS (Faster Payments) and BACS (Direct Credit / Direct Debit) clearing and settlement")
    System_Ext(boe, "Bank of England (RTGS)", "Operates CHAPS — the UK high-value real-time gross settlement system. ISO 20022 MX messaging")
    System_Ext(ebaClear, "EBA Clearing (STEP2 / RT1)", "European payment infrastructure. STEP2 for SEPA SCT (standard). RT1 for SEPA SCT Inst (instant)")
    System_Ext(ecb, "ECB TIPS", "TARGET Instant Payment Settlement — alternative to RT1 for SEPA SCT Inst settlement via central bank money")
    System_Ext(visa, "Visa / Mastercard", "International card schemes. Provide BIN sponsorship or direct membership, card authorisation network, and settlement")
    System_Ext(notifGateway, "Notification Gateway", "Third-party SMS and email notification delivery (e.g. AWS SES/SNS or equivalent)")
    System_Ext(regulatoryReporting, "Regulatory Reporting Platform", "Submission of payment data to FCA, Bank of England, and other regulators")

    Rel(retailCustomer, digitalBanking, "Initiates payments, views balances and transaction history", "HTTPS")
    Rel(smeCustomer, digitalBanking, "Uploads BACS files, initiates CHAPS and SEPA transfers", "HTTPS")
    Rel(opsStaff, paymentHub, "Monitors payment queues, resolves exceptions, performs manual override", "HTTPS / Ops Dashboard")

    Rel(digitalBanking, paymentHub, "Submits payment instructions and queries payment status", "REST / HTTPS")
    Rel(paymentHub, coreBank, "Posts debit and credit ledger entries for every payment event", "REST / ISO 20022 pacs")
    Rel(paymentHub, fraudAML, "Submits payment for pre-execution fraud and sanctions screening", "REST / HTTPS (sync)")
    Rel(paymentHub, payUK, "Sends and receives FPS and BACS messages", "ISO 20022 pacs / pain via Vocalink Gateway")
    Rel(paymentHub, boe, "Sends and receives CHAPS RTGS messages", "ISO 20022 MX pacs / camt via SWIFT/SWIFTNet")
    Rel(paymentHub, ebaClear, "Sends and receives SEPA SCT and SCT Inst messages", "ISO 20022 pacs via STEP2 / RT1")
    Rel(paymentHub, ecb, "Optional: SEPA Instant settlement via TIPS", "ISO 20022 pacs")
    Rel(paymentHub, visa, "Sends card authorisation requests and receives settlement files", "ISO 8583 / Visa DPS")
    Rel(paymentHub, notifGateway, "Sends payment confirmation and alert notifications to customers", "HTTPS / REST")
    Rel(paymentHub, regulatoryReporting, "Publishes payment statistics and suspicious activity data", "HTTPS / Batch")
```

### Target State Element Responsibility

| Element | Responsibility |
|---------|----------------|
| Payment Hub | Core system of this engagement. Orchestrates, validates, routes, and settles all payment transactions across all in-scope rails. Owns the payment lifecycle from initiation to confirmation. |
| Digital Banking Platform | Customer-facing channel. Submits payment initiation requests and consumes payment status notifications from the Payment Hub. |
| Core Banking System | Account ledger. Receives posting instructions from the Payment Hub to debit/credit customer accounts. Provides balance and account validation. |
| Fraud & AML Platform | Real-time screening. The Payment Hub calls this synchronously before releasing any outbound payment. Sanctions screening and fraud scoring are performed here. |
| Pay.UK / Vocalink | Scheme infrastructure for FPS and BACS. The Payment Hub connects via a scheme gateway (direct or bureau) to submit and receive ISO 20022 messages. |
| Bank of England (RTGS) | CHAPS settlement. The Payment Hub connects via SWIFTNet to submit ISO 20022 MX payment messages and receive settlement confirmations. |
| EBA Clearing (STEP2 / RT1) | SEPA scheme infrastructure. STEP2 for standard SCT, RT1 for SCT Inst. Connection via SWIFT or approved CSM gateway. |
| ECB TIPS | Alternative SEPA Instant settlement via central bank money. Used if RT1 is unavailable or for liquidity efficiency. |
| Visa / Mastercard | Card scheme. Provides authorisation network access, BIN management, and daily settlement files. |
| Notification Gateway | Sends customer-facing payment confirmation, failure, and fraud alert notifications via SMS and email. |
| Regulatory Reporting Platform | Receives aggregated and transaction-level data from the Payment Hub for submission to the FCA and Bank of England. |

---

# 2 Architecture Drivers

## 2.1 Function Drivers

| Id | Name | Description | Addressed | Approach & Impact |
|----|------|-------------|-----------|-------------------|
| FR-01 | Outbound FPS Payment | Initiate and send a Faster Payment to any UK account in real time. Sub-5-second end-to-end confirmation. | Designed | FPS Connector Service routes pacs.008 to Pay.UK / Vocalink. Confirmation pacs.002 returned synchronously to originating channel. |
| FR-02 | Inbound FPS Payment | Receive and credit an inbound Faster Payment from another UK institution. | Designed | FPS Connector receives inbound pacs.008, validates, triggers Core Banking credit posting, sends pacs.002 confirmation to sending bank. |
| FR-03 | BACS Direct Credit | Process outbound bulk salary and payroll files. Three-day settlement cycle via Vocalink. | Designed | BACS Connector Service accepts pain.001 bulk files, validates, submits to Vocalink BACSTEL-IP, manages multi-day settlement cycle. |
| FR-04 | BACS Direct Debit | Originate and receive direct debit collections. Mandate management included. | Designed | BACS Connector manages pain.008 (collection) and mandate registry. Advance notification to customers via Notification Gateway. |
| FR-05 | Outbound CHAPS | Initiate a high-value same-day RTGS payment via CHAPS. | Designed | CHAPS Connector sends pacs.008 (ISO 20022 MX) to Bank of England RTGS via SWIFTNet. Settlement confirmed via camt.054. |
| FR-06 | Inbound CHAPS | Receive and credit an inbound CHAPS RTGS payment. | Designed | CHAPS Connector receives inbound pacs.008, validates, triggers Core Banking credit posting. |
| FR-07 | SEPA Credit Transfer (SCT) | Send and receive standard SEPA credit transfers across EU/EEA. T+1 settlement via EBA STEP2. | Designed | SEPA Connector sends pacs.008 to STEP2. Inbound pacs.008 received and credited. |
| FR-08 | SEPA Instant (SCT Inst) | Send and receive instant SEPA credit transfers within 10-second SLA via RT1 / TIPS. | Designed | SEPA Connector sends pacs.008 to RT1. Positive pacs.002 returned within 10 seconds. Timeout handling triggers recall. |
| FR-09 | SEPA Direct Debit (SDD Core) | Originate and receive SEPA direct debit collections. Mandate management. | Designed | SEPA Connector manages pain.008 collections and SDD mandate registry. |
| FR-10 | Card Authorisation | Authorise a debit card transaction in real time via Visa / Mastercard scheme. | Designed | Card Processing Service receives authorisation request, applies velocity limits and fraud score, routes to scheme. Response returned within 300ms p99. |
| FR-11 | Card Clearing & Settlement | Process daily card clearing files from Visa / Mastercard and settle net positions. | Designed | Card Processing Service consumes scheme clearing files (ISO 8583 / IPM), reconciles against authorisation records, triggers settlement postings. |
| FR-12 | Payment Status Query | Allow customers and internal systems to query the status of any payment at any point in the lifecycle. | Designed | Payment Status Service exposes REST API backed by the Payment Store. Status events published to event bus on each lifecycle state change. |
| FR-13 | Payment Recall | Initiate or respond to a payment recall request (e.g. SWIFT gpi recall, FPS return). | Designed | Recall/Return Orchestration Service handles camt.056 (recall) and pacs.004 (return). Automated for clear cases; manual review for disputes. |
| FR-14 | Reconciliation | Automated end-of-day and intraday reconciliation of payment positions across all rails. | Designed | Reconciliation Service compares Payment Hub records against scheme settlement reports. Breaks reported to Operations Dashboard for resolution. |
| FR-15 | Sanctions & Fraud Screening | Screen all outbound payments against sanctions lists and fraud models before execution. | Designed | Synchronous pre-execution call to Fraud & AML Platform. Payment held if screening is inconclusive. Hard block on sanctions matches. |
| FR-16 | Regulatory Reporting | Publish payment data to FCA and Bank of England as required. | Designed (Phase 2) | Regulatory Reporting Adapter aggregates payment events and submits to the Regulatory Reporting Platform on scheduled intervals. |

---

## 2.2 Quality Drivers

| Id | Type | Description | Addressed | Approach & Impact |
|----|------|-------------|-----------|-------------------|
| QD.AVA | Availability | **Service Tier:** Tier 1 (mission-critical) <br> **Availability:** 99.95% per annum (< 4.4 hours downtime/year) <br> **Redundancy:** Minimum 3 replicas per service across 2 availability zones | Designed | Active-active deployment across two AZs. No single points of failure. Kubernetes pod anti-affinity rules enforce zone distribution. Circuit breakers prevent cascade failure. |
| QD.PERF | Performance | **FPS / SCT Inst end-to-end:** < 5 seconds p99 <br> **Card authorisation:** < 300ms p99 <br> **CHAPS same-day:** Within BoE settlement windows <br> **BACS bulk throughput:** 100,000+ payments per batch file | Designed | Async event-driven processing for high-throughput rails. Synchronous path optimised for real-time rails (FPS, SCT Inst, Cards). Horizontal auto-scaling via Kubernetes HPA. |
| QD.RECO | Recoverability | **RTO:** < 15 minutes (Tier 1 components) <br> **RPO:** < 1 minute (Payment Store) | Designed | Infrastructure-as-code enables rapid environment rebuild. Payment Store uses synchronous replication across AZs. Event bus retains messages for 7 days enabling replay on recovery. |
| QD.SEC | Security | All service-to-service calls authenticated via mutual TLS. Sensitive payment data must not appear in logs in clear text. PCI-DSS cardholder data environment strictly scoped. | Designed | mTLS enforced via Istio service mesh. Log masking applied at service level for IBAN, account numbers, and card PANs. CDE is isolated in a separate network segment. |
| QD.CAP | Capacity | Support current volumes at launch, with headroom for 5x growth in Year 3. FPS: 50 TPS at launch, 250 TPS Year 3. Cards: 200 TPS at launch, 1,000 TPS Year 3. | Designed | All components sized for Year 3 peak. Event bus partitioned for horizontal throughput scaling. Database connection pooling and read replicas for query separation. |
| QD.COMP | Compliance | All payment messages must conform to scheme rulebooks. Audit trail must be immutable and retained for 7 years. | Designed | ISO 20022 message validation at ingress. Immutable audit log written to append-only store (OpenSearch / S3). Retention policy enforced via lifecycle rules. |

---

## 2.3 Other Drivers

| Id | Type | Description | Addressed | Approach & Impact |
|----|------|-------------|-----------|-------------------|
| AC.ROB | Robustness | Components must handle scheme unavailability, timeout, and partial failure gracefully. Payments must not be lost or double-processed. | Fully | Idempotency keys on all payment submissions. Exactly-once semantics on event bus where supported. Circuit breakers for scheme connectors. Dead-letter queues for undeliverable messages. |
| AC.SUP | Supportability | Every payment must be traceable end-to-end using a consistent correlation ID (Payment ID) across all services and logs. Minimum log level: WARN in production, DEBUG available on demand. | Fully | Correlation ID (UUID) injected at initiation and propagated through all service calls and events. Centralised log aggregation. APM tooling provides distributed tracing. |
| AC.EXT | Extensibility | Architecture must accommodate additional payment rails (e.g. SWIFT cross-border, domestic instant rails in new markets) without core redesign. | Fully | Connector pattern: each rail is an independently deployable service behind a common Payment Connector interface. New rails are added as new connectors without modifying the Orchestration Engine. |
| AC.PORT | Portability | No hard dependency on a single cloud provider's proprietary services. Core business logic must be portable. | Fully | Business logic deployed in containers. Cloud-native services (managed Kafka, managed Postgres) used via abstraction layers. Terraform modules parameterised for multi-cloud. |
| AC.AUD | Auditability | Every state transition in the payment lifecycle must be recorded immutably with actor, timestamp, and reason. Required for regulatory examination. | Fully | Audit events published to the event bus and written to an append-only audit store. Tampering detection via hash-chaining of audit records. |

---

## 2.4 Constraints

| Id | Status / Type | Name / Description | Addressed | Approach & Impact |
|----|---------------|--------------------|-----------|-------------------|
| CON-01 | Approved / Regulatory | FCA / PSRs 2017 — Payment execution time limits and liability rules must be met | Fully | FPS and SCT Inst processing SLAs designed to meet PSRs day D execution requirements. Return and refund flows implemented. |
| CON-02 | Approved / Regulatory | CHAPS Participation Rules — ISO 20022 MX mandatory; BoE RTGS connectivity required | Fully | CHAPS Connector implements ISO 20022 MX (pacs.008 / camt.054 / camt.056). SWIFTNet connectivity provisioned. |
| CON-03 | Approved / Regulatory | PCI-DSS v4.0 Level 1 — Cardholder data environment must be scoped and audited | Fully | CDE isolated in dedicated network segment. Card PANs tokenised at ingress. No PAN stored outside CDE. Annual QSA audit required. |
| CON-04 | Approved / Regulatory | UK GDPR — PII must be protected; data residency must be maintained in UK | Fully | UK cloud region used for all UK customer data. PII masked in logs. DPIA completed (REF-10). |
| CON-05 | Approved / Architecture | ISO 20022 native adoption — no proprietary message formats for inter-service communication | Fully | All payment events use ISO 20022 message structures. No internal proprietary payment message format introduced. |
| CON-06 | Approved / Architecture | Infrastructure-as-code mandatory — no manually provisioned infrastructure in any environment | Fully | Terraform used for all infrastructure. Manual provisioning triggers a policy violation alert. |
| CON-07 | Open / Technical | Scheme access model (OQ-04) — direct vs bureau/sponsored | Partially | Architecture designed to accommodate both. Bureau access assumed for Phase 1 with direct membership roadmapped. See ADR-005. |
| CON-08 | Approved / Security | Zero-trust networking — no implicit trust between services in any environment | Fully | Istio service mesh enforces mTLS for all east-west traffic. API Gateway enforces authentication for all north-south traffic. |

---

# 3 Decisions

## 3.1 Decision Records

| Id | Decision | Rationale | Consequences | Status |
|----|----------|-----------|--------------|--------|
| ADR-001 | Adopt an event-driven architecture with Apache Kafka as the internal event bus | Kafka provides durable, replayable, ordered event streams required for exactly-once payment processing and audit. Alternatives (RabbitMQ, AWS SQS) lack the retention, replay, and partitioned throughput required for multi-rail high-volume payment processing. | Kafka cluster must be managed as a Tier 1 component with active-active deployment. Engineering team must develop Kafka expertise. Schema registry required for message contract governance. | Approved 2026-03-08 |
| ADR-002 | Adopt ISO 20022 as the canonical internal message standard | ISO 20022 is the mandated standard for CHAPS, the dominant standard for FPS and SEPA, and the global direction for card schemes. Native adoption eliminates translation layers, reduces risk, and future-proofs the hub. | All connector and orchestration services must implement ISO 20022 message structures. Message validation library required. Team ISO 20022 training required. | Approved 2026-03-08 |
| ADR-003 | Decompose the Payment Hub into domain-aligned microservices using the Connector pattern | Each payment rail has distinct scheme rules, SLAs, and message formats. Independent connectors allow rail-specific deployments, upgrades, and failure isolation. A monolithic approach would create unacceptable coupling between rail domains. | Each connector is an independently deployable service. Integration contracts via the event bus must be formally governed. Operational complexity increases — mitigated by Kubernetes and service mesh. | Approved 2026-03-08 |
| ADR-004 | Deploy on Kubernetes with Istio service mesh for all Payment Hub services | Kubernetes provides the container orchestration, auto-scaling, and self-healing required for a Tier 1 payment platform. Istio enforces zero-trust mTLS without burdening application code. Alternatives (ECS, Lambda) do not provide the same portability and service mesh capabilities. | Kubernetes cluster is a critical dependency. Platform engineering capability required. Istio adds latency overhead (measured at < 1ms p99 in testing) which is acceptable for the rails in scope. | Approved 2026-03-08 |
| ADR-005 | Phase 1 scheme connectivity via bureau/sponsored access; Phase 2+ direct membership | Direct Pay.UK, CHAPS, and EBA membership requires regulatory approval, technical certification, and significant capital. Bureau access (via an approved direct participant) allows Phase 1 go-live without certification delay. Direct membership delivers lower per-transaction cost and control in Phase 2. | Bureau access introduces a third-party dependency and per-transaction cost. Scheme access SLA is governed by the bureau's availability. Direct membership programme must begin in Phase 1 to be ready for Phase 2. See RISK-03. | Approved 2026-03-08 |
| ADR-006 | Card processing in-house for authorisation; use third-party card processor for clearing/settlement in Phase 2 | Building full in-house card processing (authorisation + clearing + settlement) is high-risk and high-cost for Phase 2. In-house authorisation gives control and speed. Outsourcing clearing/settlement to an established card processor (e.g. GPS, Enfuce, Marqeta) reduces Phase 2 delivery risk and PCI scope. | Two integration points for cards: real-time authorisation (in-house) and clearing file exchange (third-party processor). Third-party processor selection is OQ-10 and must be resolved before Phase 2 design. | Proposed — pending OQ-10 |
| ADR-007 | PostgreSQL as the primary OLTP store for the Payment Store; separate read replicas for query workloads | PostgreSQL provides ACID guarantees required for payment record integrity. Managed PostgreSQL (cloud provider) reduces operational overhead. Read replicas separate write-heavy payment processing from query-heavy monitoring and reporting workloads. | Cloud-managed PostgreSQL required. Connection pooling (PgBouncer) required for high-throughput writes. Data at rest encryption mandatory. Backup and point-in-time recovery configured. | Approved 2026-03-08 |

## 3.2 Decision Roadmap

| Decision | Options & Consequences | Status |
|----------|------------------------|--------|
| Cloud provider selection (OQ-01) | AWS, Azure, GCP, or multi-cloud. Each has different managed Kafka, Postgres, and Kubernetes offerings. Decision affects cost, tooling, and DR complexity. | Open — OQ-01 |
| Direct scheme membership (FPS/BACS/CHAPS) | Direct participation vs continued bureau. Triggered by volume thresholds or cost analysis. | Open — Phase 2 trigger |
| SEPA CSM selection (OQ-04 extension) | RT1 vs TIPS for SCT Inst. Both are EPC-compliant; TIPS offers central bank money settlement. Decision required before Phase 3 SEPA connector design. | Open — Phase 2 trigger |
| Third-party card processor selection (OQ-10) | GPS, Enfuce, Marqeta, or equivalent. Affects PCI scope, integration design, and Phase 2 timeline. | Open — OQ-10 |
| SEPA SDD B2B scope (OQ-08) | Include alongside SDD Core in Phase 3 or defer to Phase 4. B2B adds mandate complexity. | Open — OQ-08 |

---

# 4 Risks and Technical Debt

> **Risk Rating (RR) notation:** `[Likelihood][Impact]`
> | Likelihood | Impact |
> |---|---|
> | 5 = Almost Certain | A = Insignificant |
> | 4 = Likely | B = Minor |
> | 3 = Possible | C = Moderate |
> | 2 = Unlikely | D = Major |
> | 1 = Rare | E = Catastrophic |

## 4.1 Technical Debt

| Id | Summary | Inherent RR | Residual RR | Mitigation | Status |
|----|---------|-------------|-------------|------------|--------|
| TD-001 | Bureau scheme access (Phase 1) introduces third-party dependency and higher per-transaction cost. Direct membership deferred to Phase 2. | 4C | 3C | Direct membership programme begins in Phase 1. Bureau SLA contractually enforced with penalties. Active monitoring of bureau availability. | New |
| TD-002 | Third-party card processor (Phase 2) introduces vendor dependency for clearing/settlement. In-house capability not built in this phase. | 3C | 2C | Vendor contract includes SLA, termination rights, and data portability clause. Architecture designed to swap processor without core redesign. | New |
| TD-003 | ISO 8583 integration for card authorisation is a legacy protocol that will require replacement as Visa/Mastercard adopt ISO 20022 for card messaging (target 2027-2030). | 3B | 3B | Authorisation service abstracted behind a Card Scheme Adapter interface. Migration to ISO 20022 card messaging will be a contained change. | New |

## 4.2 Architecture Risks

| Id | Title and Description | Inherent RR | Residual RR | Mitigation | Target RR | Strategy |
|----|-----------------------|-------------|-------------|------------|-----------|----------|
| RISK-01 | **Core Banking Integration Failure** — Core Banking platform not selected or integration-ready before Phase 1 go-live. Without a functional Core Banking adapter, payment entries cannot be posted. Business impact: complete payment service unavailability. | 4E | 2D | Core Banking selection and integration delivery tracked as a hard Phase 1 dependency. Architecture Adapter pattern allows stub/mock for development; real integration tested in staging from month 3. Escalation to CTO if slippage detected. | 2D | Reduce |
| RISK-02 | **Kafka Single-Region Failure** — Loss of the primary Kafka cluster results in loss of in-flight payment events and inability to process new payments until recovered. Business impact: payment processing halted, SLA breaches, regulatory breach risk. | 2E | 1D | Kafka deployed active-active across two AZs. Cross-region replication to DR region. Message retention 7 days enables replay. RTO target < 15 minutes. Regular failover DR drills. | 1D | Reduce |
| RISK-03 | **Scheme Access Unavailability** — Bureau payment scheme provider experiences outage or terminates the relationship, leaving ABC Neo Bank without FPS/BACS access. Business impact: domestic payments unavailable; reputational and regulatory consequence. | 2E | 2D | Dual-bureau contingency: secondary bureau identified and onboarded as warm standby. Direct membership programme accelerated as risk response. Contractual SLA with bureau includes financial remedy and business continuity obligations. | 2C | Reduce |
| RISK-04 | **PCI-DSS Scope Creep** — Card data leaks outside the defined CDE due to misconfiguration or developer error, causing PCI scope expansion and potential QSA audit failure. Business impact: card programme delayed; potential scheme fines. | 3D | 2C | CDE implemented as an isolated network segment with no direct internet access. All card PANs tokenised at the Card Scheme Adapter before entering the Payment Hub. Automated scanning (Trufflehog, Vanta) detects credential and card data leakage in code and logs. | 2B | Reduce |
| RISK-05 | **ISO 20022 Message Validation Gaps** — Scheme message validation rules not fully implemented, causing message rejection by Pay.UK, Bank of England, or EBA. Business impact: payments rejected, SLA breaches, scheme fine risk. | 3C | 2B | Dedicated ISO 20022 validation library used (e.g. open-source pacs/pain validator). Scheme-specific validation rules coded from rulebooks. Pre-production scheme testing environment used before go-live. | 2B | Reduce |
| RISK-06 | **Sanctions Screening Latency** — Real-time Fraud & AML Platform response exceeds acceptable timeout, causing payment delays and poor customer experience. Business impact: customer complaints; potential PSRs execution time breach. | 3C | 2B | Async screening path for BACS (batch). Synchronous path for FPS/CHAPS with 2-second timeout threshold. Timeout triggers payment hold with automatic ops escalation. SLA contractually enforced with Fraud/AML platform vendor. | 2B | Reduce |
| RISK-07 | **SEPA Membership / EU Entity Readiness** — ABC Neo Bank EU legal entity not operational before Phase 3 SEPA delivery. Business impact: SEPA rail not deliverable on Phase 3 timeline. | 3D | 2C | EU entity legal and regulatory programme tracked as a Phase 3 dependency. SEPA design work begins in Phase 2 regardless. If EU entity delayed, SEPA delivery re-planned. | 2C | Reduce |

## 4.3 Architecture Assumptions

| Description | Impact if False | Actions to Validate | Status |
|-------------|-----------------|---------------------|--------|
| ASM-01: A bureau payment scheme provider (FPS/BACS) can be onboarded and technically certified before Phase 1 go-live (Q4 2026). | Phase 1 go-live delayed. Domestic payment capability not available. | Head of Payments to confirm bureau selection and onboarding timeline by 2026-04-01. | Open |
| ASM-02: The selected Core Banking platform will provide a REST API capable of receiving posting instructions from the Payment Hub in real time. | Core Banking integration design must be reworked. May require batch posting instead, introducing settlement lag. | Integration API specification to be agreed with Core Banking vendor by 2026-04-15. | Open |
| ASM-03: The Fraud & AML Platform will deliver a synchronous REST API for pre-payment screening with a p99 response time of < 1 second. | FPS/SCT Inst payment SLAs at risk. | Fraud & AML platform vendor SLA to be contractually agreed. Load testing to be performed in staging. | Open |
| ASM-04: Cloud provider will be selected and environments provisioned before Phase 1 detailed design commences (by 2026-05-01). | Detailed infrastructure design blocked. IaC cannot be written without target cloud provider confirmed. | CTO to confirm cloud provider via OQ-01 by 2026-04-01. | Open |
| ASM-05: ABC Neo Bank will hold sufficient pre-funded liquidity in scheme settlement accounts to cover daily net settlement obligations from day one. | Settlement failures would result in payment rejections and regulatory breach. | Treasury to confirm pre-funding model and settlement account arrangements by 2026-06-01. | Open |

## 4.4 Architecture Dependencies

| Description | Impact if Not Realised | Action Plan | Status |
|-------------|------------------------|-------------|--------|
| DEP-01: Core Banking System — integration-ready API available in staging by 2026-08-01 for Phase 1 integration testing. | Phase 1 integration testing delayed; go-live at risk. | CTO tracking as a programme dependency. Stub API to be provided by Core Banking team for Payment Hub development. | Open |
| DEP-02: Bureau Scheme Access — FPS and BACS bureau access contracted and technically certified by 2026-09-01. | Phase 1 go-live cannot proceed. | Head of Payments leading bureau procurement. Technical certification timeline to be confirmed. | Open |
| DEP-03: Cloud Environment — production environment provisioned and baseline security controls applied by 2026-07-01. | Development and testing delayed downstream. | Infrastructure Architect to confirm provisioning plan once OQ-01 resolved. | Open |
| DEP-04: Fraud & AML Platform — available in staging by 2026-08-01 with sandbox environment for integration testing. | FPS outbound payments cannot be tested end-to-end without screening integration. | CRO leading Fraud/AML procurement. Sandbox environment to be contractually required. | Open |
| DEP-05: PCI-DSS QSA Audit — initial scoping assessment complete before Phase 2 card design begins (by 2026-10-01). | Card CDE design may not meet QSA requirements, causing rework. | CISO to engage QSA by 2026-07-01 for scoping review. | Open |

---

# 5 Application Architecture

## 5.1 Solution Structure

### Current State

There is no current state. ABC Neo Bank has no existing payment infrastructure. There are no systems to migrate, decommission, or integrate with beyond the Core Banking System (to be selected) and the Digital Banking Platform (greenfield).

**Current State Summary:** Manual capability gap. ABC Neo Bank cannot process payments today.

### Target State — C4 Level 2 (Container Diagram)

```mermaid
C4Container
    title ABC Neo Bank — Payment Hub — Container Diagram (C4 Level 2)

    Person(retailCustomer, "Retail Customer", "Initiates payments via Digital Banking App")
    Person(smeCustomer, "SME Customer", "Uploads bulk payment files and initiates high-value transfers")
    Person(opsStaff, "Operations Staff", "Monitors, manages exceptions, performs manual interventions")

    System_Ext(payUK, "Pay.UK / Vocalink", "FPS and BACS scheme infrastructure")
    System_Ext(boe, "Bank of England RTGS", "CHAPS high-value settlement")
    System_Ext(ebaClear, "EBA Clearing (STEP2/RT1)", "SEPA SCT and SCT Inst")
    System_Ext(visa, "Visa / Mastercard", "Card scheme authorisation and settlement")
    System_Ext(coreBank, "Core Banking System", "Account ledger and balance management")
    System_Ext(fraudAML, "Fraud & AML Platform", "Real-time fraud and sanctions screening")
    System_Ext(notifGateway, "Notification Gateway", "SMS and email delivery")
    System_Ext(digitalBanking, "Digital Banking Platform", "Customer web and mobile channels")

    System_Boundary(hub, "ABC Neo Bank — Payment Hub") {

        Container(apiGateway, "API Gateway", "Kong / AWS API GW", "Authenticates and routes all inbound API traffic. Enforces rate limiting, OAuth 2.0, and API versioning. North-south traffic entry point")

        Container(payOrchestrator, "Payment Orchestration Service", "Java / Spring Boot", "Core orchestration engine. Manages the full payment lifecycle: validate, screen, route, confirm, and post. Applies business rules, limits, and routing logic for all rails")

        Container(eventBus, "Event Bus", "Apache Kafka", "Durable, ordered, replayable event stream for all payment lifecycle events. Decouples producers from consumers. Partitioned for throughput. 7-day message retention")

        Container(fpsConnector, "FPS Connector Service", "Java / Spring Boot", "Sends and receives ISO 20022 pacs.008 / pacs.002 messages to/from Pay.UK via Vocalink Gateway. Manages FPS cycle timing, return codes, and confirmation handling")

        Container(bacsConnector, "BACS Connector Service", "Java / Spring Boot", "Manages BACS three-day cycle. Submits pain.001 Direct Credit and pain.008 Direct Debit files to Vocalink BACSTEL-IP. Receives ARUCS / ADDACS reports")

        Container(chapsConnector, "CHAPS Connector Service", "Java / Spring Boot", "Sends and receives ISO 20022 MX pacs.008 / camt.054 messages to/from Bank of England RTGS via SWIFTNet. Manages CHAPS settlement windows")

        Container(sepaConnector, "SEPA Connector Service", "Java / Spring Boot", "Handles SEPA SCT (pacs.008 via STEP2), SCT Inst (pacs.008 via RT1, 10s SLA), and SDD Core (pain.008). Manages SEPA mandate registry and recall flows")

        Container(cardProcessor, "Card Processing Service", "Java / Spring Boot", "Manages card authorisation lifecycle. Applies velocity limits, fraud score thresholds, and available balance checks. Routes authorisation request to Visa/Mastercard network. PCI-DSS scoped")

        Container(msgTranslator, "ISO 20022 Message Translation Engine", "Java / JAXB", "Validates and transforms ISO 20022 messages at ingress. Enforces scheme-specific validation rules (FPS, CHAPS, SEPA) from scheme rulebooks. Rejects non-compliant messages with structured error response")

        Container(paymentStore, "Payment Store", "PostgreSQL (managed)", "Source of truth for all payment records. Stores lifecycle state, audit trail, and correlation identifiers. Write replicas for HA; read replicas for query separation")

        Container(auditStore, "Audit & Compliance Store", "OpenSearch / S3 (append-only)", "Immutable append-only log of all payment lifecycle events with actor, timestamp, and state. Hash-chained for tamper detection. Retained for 7 years per regulatory requirement")

        Container(recon, "Reconciliation Service", "Python / Pandas", "Compares Payment Hub payment records against scheme settlement reports (intraday and end-of-day). Raises breaks to Operations Dashboard. Publishes reconciliation events to event bus")

        Container(notifService, "Notification Service", "Node.js", "Consumes payment status events from the event bus and dispatches customer notifications (confirmation, failure, recall) via the Notification Gateway")

        Container(statusAPI, "Payment Status API", "Java / Spring Boot", "Exposes REST API for payment status queries. Backed by Payment Store read replica. Consumes status events for real-time push notifications via WebSocket")

        Container(opsDashboard, "Operations Dashboard", "React / TypeScript", "Internal web application for operations staff. Provides real-time payment monitoring, exception queue management, manual intervention, and reconciliation break resolution")

        Container(coreBankAdapter, "Core Banking Adapter", "Java / Spring Boot", "Integration facade between the Payment Hub and the Core Banking System. Translates Payment Hub posting instructions into Core Banking API calls. Handles retries, idempotency, and failure alerting")

        Container(schemaRegistry, "Schema Registry", "Confluent Schema Registry", "Governs Kafka message schemas (Avro/Protobuf). Enforces backward and forward compatibility for all event bus topics. Prevents breaking schema changes")

        Container(secretMgmt, "Secret & Certificate Management", "HashiCorp Vault", "Manages TLS certificates, API keys, database credentials, and scheme connectivity secrets. Auto-rotates credentials. Provides dynamic secrets for services")
    }

    Rel(retailCustomer, digitalBanking, "Uses", "HTTPS")
    Rel(smeCustomer, digitalBanking, "Uses", "HTTPS")
    Rel(digitalBanking, apiGateway, "Submits payment instructions and queries status", "REST / HTTPS")
    Rel(opsStaff, opsDashboard, "Monitors and manages payments", "HTTPS")
    Rel(opsDashboard, statusAPI, "Queries payment status and exception queue", "REST")

    Rel(apiGateway, payOrchestrator, "Routes validated payment requests", "REST / gRPC")
    Rel(payOrchestrator, msgTranslator, "Validates and enriches payment message", "sync")
    Rel(payOrchestrator, fraudAML, "Screens payment before execution", "REST / HTTPS (sync)")
    Rel(payOrchestrator, eventBus, "Publishes payment lifecycle events", "Kafka")
    Rel(payOrchestrator, coreBankAdapter, "Instructs account debit/credit posting", "REST")

    Rel(eventBus, fpsConnector, "Consumes outbound FPS payment events", "Kafka")
    Rel(eventBus, bacsConnector, "Consumes outbound BACS payment events", "Kafka")
    Rel(eventBus, chapsConnector, "Consumes outbound CHAPS payment events", "Kafka")
    Rel(eventBus, sepaConnector, "Consumes outbound SEPA payment events", "Kafka")
    Rel(eventBus, notifService, "Consumes payment status events", "Kafka")
    Rel(eventBus, recon, "Consumes settlement confirmation events", "Kafka")
    Rel(eventBus, auditStore, "Streams all lifecycle events for immutable audit", "Kafka Connect")

    Rel(fpsConnector, payUK, "Sends pacs.008 / receives pacs.002", "ISO 20022 / HTTPS")
    Rel(bacsConnector, payUK, "Submits bulk files / receives ARUCS", "BACSTEL-IP")
    Rel(chapsConnector, boe, "Sends pacs.008 MX / receives camt.054", "SWIFTNet")
    Rel(sepaConnector, ebaClear, "Sends pacs.008 / receives pacs.002", "SWIFT / CSM gateway")
    Rel(cardProcessor, visa, "Sends authorisation requests / receives clearing files", "ISO 8583 / Visa DPS")

    Rel(coreBankAdapter, coreBank, "Posts debit/credit entries", "REST / HTTPS")
    Rel(notifService, notifGateway, "Sends payment notifications", "REST / HTTPS")
    Rel(payOrchestrator, paymentStore, "Reads and writes payment records", "SQL / TLS")
    Rel(statusAPI, paymentStore, "Reads payment status from read replica", "SQL / TLS")
    Rel(schemaRegistry, eventBus, "Governs message schemas", "REST")
    Rel(secretMgmt, payOrchestrator, "Provides secrets and certificates", "Vault API / TLS")
```

### Target State Narrative

The Payment Hub processes payments through three logical paths:

**1. Real-Time Outbound Payment (FPS / CHAPS / SCT Inst):**
The Digital Banking Platform sends a payment initiation request to the API Gateway via REST over HTTPS. The API Gateway authenticates the request (OAuth 2.0 JWT) and routes it to the Payment Orchestration Service. The Orchestration Service calls the ISO 20022 Message Translation Engine to validate and enrich the message, then makes a synchronous call to the Fraud & AML Platform for pre-execution screening. On a clear screening result, the Orchestration Service writes the payment to the Payment Store (status: `PENDING`) and publishes a `PaymentSubmitted` event to the Kafka event bus. The relevant Rail Connector (FPS / CHAPS / SEPA) consumes the event, sends the ISO 20022 pacs.008 to the scheme, and awaits the pacs.002 response. On confirmation, the Connector publishes a `PaymentConfirmed` event. The Orchestration Service consumes this event, instructs the Core Banking Adapter to post the debit entry, updates the Payment Store (status: `COMPLETED`), and the Notification Service sends a payment confirmation to the customer.

**2. Inbound Payment:**
The Rail Connector receives an inbound pacs.008 from the scheme. It publishes an `InboundPaymentReceived` event to the Kafka event bus. The Orchestration Service consumes the event, validates the beneficiary account, calls the Fraud & AML Platform for inbound screening, instructs the Core Banking Adapter to post the credit entry, updates the Payment Store, and the Notification Service notifies the beneficiary customer.

**3. Bulk BACS Payment:**
The Digital Banking Platform or Operations Dashboard uploads a pain.001 bulk file to the API Gateway. The BACS Connector Service validates the file, submits it to Vocalink BACSTEL-IP, and manages the three-day settlement cycle. Status events are published at each settlement stage (Day 1 submission, Day 2 processing, Day 3 settlement). Reconciliation Service processes the ARUCS/ADDACS report on Day 3 and raises any breaks to the Operations Dashboard.

### Application Inventory

| Id | Application | Description | Role | Impact |
|----|-------------|-------------|------|--------|
| APP-01 | API Gateway | Authenticates and routes all inbound payment API traffic. Enforces rate limits and versioning. | R | New |
| APP-02 | Payment Orchestration Service | Core payment lifecycle management. Validates, screens, routes, and confirms all payment types. | R | New |
| APP-03 | ISO 20022 Message Translation Engine | Validates and transforms ISO 20022 messages at ingress and egress. Scheme-specific validation. | R | New |
| APP-04 | FPS Connector Service | Manages FPS real-time credit transfer connectivity with Pay.UK / Vocalink. | R | New |
| APP-05 | BACS Connector Service | Manages BACS three-day bulk file processing via BACSTEL-IP. | R | New |
| APP-06 | CHAPS Connector Service | Manages CHAPS RTGS payment connectivity with Bank of England via SWIFTNet. | R | New |
| APP-07 | SEPA Connector Service | Manages SEPA SCT, SCT Inst, and SDD connectivity via EBA Clearing (STEP2 / RT1). | R | New |
| APP-08 | Card Processing Service | Manages card authorisation, clearing file processing, and settlement. PCI-DSS scoped. | R | New |
| APP-09 | Payment Store (PostgreSQL) | Source of truth for all payment lifecycle records. ACID-compliant OLTP store. | R | New |
| APP-10 | Event Bus (Kafka) | Durable event streaming platform for all payment lifecycle events. | R | New |
| APP-11 | Audit & Compliance Store | Immutable append-only audit log. Retained 7 years. Tamper-detected via hash-chaining. | R | New |
| APP-12 | Reconciliation Service | Automated multi-rail reconciliation comparing hub records against scheme settlement reports. | R | New |
| APP-13 | Notification Service | Consumes payment events and dispatches customer notifications via Notification Gateway. | R | New |
| APP-14 | Payment Status API | REST API for payment status queries. Real-time push via WebSocket for Operations Dashboard. | R | New |
| APP-15 | Operations Dashboard | Internal web application for payment monitoring, exception management, and manual intervention. | R | New |
| APP-16 | Core Banking Adapter | Integration facade to Core Banking System. Handles posting instructions and idempotency. | R | New |
| APP-17 | Schema Registry | Governs Kafka message schemas. Enforces compatibility rules for all event bus topics. | R | New |
| APP-18 | Secret & Certificate Management (Vault) | Manages all secrets, API keys, TLS certificates, and database credentials. Auto-rotation. | R | New |
| APP-19 | Digital Banking Platform | Customer-facing web and mobile channel. Consumes Payment Hub APIs. | I | Uses |
| APP-20 | Core Banking System | Account ledger. Receives posting instructions. Dependency — not in scope for this engagement. | I | Uses |
| APP-21 | Fraud & AML Platform | Real-time screening. Dependency — not in scope for this engagement. | I | Uses |

---

## 5.2 Solution Behaviour

### Target State — Happy Path: Outbound FPS Payment

```mermaid
sequenceDiagram
    autonumber
    actor Customer
    participant DigitalApp as Digital Banking App
    participant APIGW as API Gateway
    participant Orchestrator as Payment Orchestration
    participant MsgValidator as ISO 20022 Validator
    participant FraudAML as Fraud & AML Platform
    participant Kafka as Event Bus (Kafka)
    participant FPS as FPS Connector
    participant PayUK as Pay.UK / Vocalink
    participant CoreBank as Core Banking Adapter

    Customer->>DigitalApp: Initiates FPS payment (payee, amount, reference)
    DigitalApp->>APIGW: POST /payments/fps (OAuth 2.0 JWT)
    APIGW->>APIGW: Authenticate token, enforce rate limit
    APIGW->>Orchestrator: Route payment request
    Orchestrator->>MsgValidator: Validate and enrich ISO 20022 pacs.008
    MsgValidator-->>Orchestrator: Validation result (pass / reject with error code)
    Orchestrator->>FraudAML: Screen payment (sync, 2s timeout)
    FraudAML-->>Orchestrator: Screening result (CLEAR / HOLD / BLOCK)
    Orchestrator->>Orchestrator: Write payment to Payment Store (status: PENDING)
    Orchestrator->>Kafka: Publish PaymentSubmitted event
    Kafka->>FPS: Consume PaymentSubmitted event
    FPS->>PayUK: Send pacs.008 to Vocalink
    PayUK-->>FPS: Return pacs.002 (accepted / rejected)
    FPS->>Kafka: Publish PaymentConfirmed event
    Kafka->>Orchestrator: Consume PaymentConfirmed event
    Orchestrator->>CoreBank: POST /posting (debit instruction, idempotency key)
    CoreBank-->>Orchestrator: Posting confirmed
    Orchestrator->>Orchestrator: Update Payment Store (status: COMPLETED)
    Kafka->>DigitalApp: Push payment status notification (WebSocket)
    Customer->>DigitalApp: Views payment confirmation
```

### Target State — Error Path: Fraud Hold

```mermaid
sequenceDiagram
    autonumber
    actor Customer
    participant APIGW as API Gateway
    participant Orchestrator as Payment Orchestration
    participant FraudAML as Fraud & AML Platform
    participant Kafka as Event Bus (Kafka)
    participant OpsTeam as Operations Staff
    participant OpsDash as Operations Dashboard

    Customer->>APIGW: POST /payments/fps
    APIGW->>Orchestrator: Route payment request
    Orchestrator->>FraudAML: Screen payment (sync)
    FraudAML-->>Orchestrator: Result: HOLD (fraud score above threshold)
    Orchestrator->>Orchestrator: Write payment to Payment Store (status: HELD)
    Orchestrator->>Kafka: Publish PaymentHeld event
    Kafka->>OpsDash: Alert raised in exception queue
    OpsTeam->>OpsDash: Reviews payment, makes decision
    alt Approve
        OpsTeam->>Orchestrator: Manual approve via Ops API
        Orchestrator->>Kafka: Publish PaymentApproved event
        Note over Orchestrator,Kafka: Continues on happy path
    else Reject
        OpsTeam->>Orchestrator: Manual reject
        Orchestrator->>Orchestrator: Update Payment Store (status: REJECTED)
        Kafka->>Customer: Payment failure notification
    end
```

---

# 6 Business Architecture

## 6.1 Business Capability

Impacted business capabilities:

- **Payment Processing**
  - **Domestic Real-Time Payments (FPS)** — New capability. Real-time credit transfers for retail and SME customers.
  - **Bulk Payments (BACS)** — New capability. Direct Credit and Direct Debit for payroll and recurring payments.
  - **High-Value Payments (CHAPS)** — New capability. Same-day RTGS for high-value transactions.
  - **European Payments (SEPA)** — New capability. SCT, SCT Inst, and SDD for EU customers.
- **Card Services**
  - **Card Issuing & Authorisation** — New capability. Debit card issuance via Visa/Mastercard.
  - **Card Clearing & Settlement** — New capability. Daily clearing file processing and net settlement.
- **Financial Operations**
  - **Payment Clearing & Settlement** — New capability. Multi-rail net position management and settlement.
  - **Reconciliation** — New capability. Automated intraday and end-of-day reconciliation.
  - **Ledger Posting** — Modified (via Core Banking integration). All payment events post debit/credit entries.
- **Risk & Compliance**
  - **Fraud & Financial Crime Monitoring** — Enhanced. Real-time pre-payment screening integrated into payment flow.
  - **Regulatory Reporting** — New capability. Automated data submission to FCA and Bank of England.
  - **Audit & Evidencing** — New capability. Immutable 7-year audit trail for all payment events.
- **Customer Experience**
  - **Payment Initiation & Confirmation** — New capability. API-first initiation with real-time status.
  - **Payment Notifications** — New capability. Automated SMS/email notification on payment events.

## 6.2 Business Process

| Process Name | Impact | Description & Strategic Fit |
|--------------|--------|-----------------------------|
| Retail Payment Initiation | Major | New digital-first payment initiation process via API. Customers initiate via mobile/web. Eliminates branch-based payment processes not applicable to ABC Neo Bank. |
| SME Bulk Payment Upload | Major | New self-service BACS file upload process. SME customers upload pain.001 files directly. Reduces operations overhead for bulk payment processing. |
| Payment Exception Handling | Major | New operations process. Exception queue in Operations Dashboard replaces manual email-based exception workflows. Operations staff resolve exceptions in real time. |
| End-of-Day Reconciliation | Major | Automated multi-rail reconciliation replaces manual spreadsheet processes. Breaks escalated automatically. Significantly reduces ops effort and error risk. |
| Scheme Settlement Management | Major | New treasury process. Daily settlement with Pay.UK (FPS/BACS), Bank of England (CHAPS), EBA Clearing (SEPA), and Visa/Mastercard (Cards). Requires pre-funded settlement accounts. |
| Fraud Investigation | Minor | Payments flagged by the Fraud & AML Platform are routed to the Operations Dashboard exception queue. Financial crime analysts investigate and release or reject via the Ops API. |
| Customer Notification | No Impact | Notification delivery automated. No change to customer-facing communication policy. |

## 6.3 Business Operating Model

The Payment Hub introduces the following operational model changes:

**New Operations Team Function — Payment Operations Centre (POC):**
A dedicated Payment Operations Centre is required to monitor the Operations Dashboard, manage exception queues across all five rails, process manual interventions (recall, return, override), and act as L1 escalation for customer payment queries. This function does not exist today and must be established as part of Phase 1 delivery.

**Treasury Function Enhancement:**
Daily settlement obligations across Pay.UK, Bank of England, EBA Clearing, and card schemes require a treasury function capable of managing intraday liquidity and pre-funded settlement positions. Existing treasury capability must be assessed and augmented.

**Financial Crime Function Integration:**
The Fraud & AML Platform screening results feed into the Operations Dashboard exception queue. The financial crime team must have defined SLAs for reviewing held payments, as PSRs execution time obligations continue to run while a payment is held.

**No change** to the existing customer onboarding, lending, or deposits operating models.

---

# 7 Data Architecture

## 7.1 Data Approach & Impact

| Area | Approach & Impact |
|------|-------------------|
| Data Definition | New data entities introduced: Payment, PaymentEvent, Mandate (BACS/SDD), SettlementPosition, ReconciliationRecord, AuditEntry. Definitions to be registered in the Business Glossary. |
| Data Management | Payment records are the system of record for all payment lifecycle state. The Payment Store (PostgreSQL) is the authoritative source. Event bus provides the event record. Audit Store provides the immutable compliance record. Data governance owner: Head of Payments. |
| Data Security | All data at rest encrypted (AES-256). All data in transit protected by TLS 1.3 minimum. PAN data encrypted and tokenised within the CDE before transit. IBAN and account numbers masked in logs and monitoring tools. |
| Data Access | Services access the Payment Store via dedicated service accounts with least-privilege grants. No direct database access from external systems. Operations Dashboard accesses a read-only replica via the Status API. |
| Data Lifecycle | Payment data originates at initiation, flows through orchestration and rail connectors, is confirmed at scheme, posted to Core Banking, and archived at 7 years. Event bus retains events for 7 days (operational replay). Audit Store retains permanently (deletion requires regulatory authorisation). |
| Data Ownership | Payment data: Head of Payments. Customer PII (name, account details): Data Protection Officer. Audit records: Chief Risk Officer. Mandate data: Head of Payments. Card data (within CDE): Head of Cards. |

## 7.2 Logical Data Model (ERD)

```mermaid
erDiagram
    PAYMENT {
        uuid payment_id PK
        string correlation_id
        string rail
        string direction
        string status
        decimal amount
        string currency
        timestamp initiated_at
        timestamp confirmed_at
        timestamp completed_at
        string initiator_account_id FK
        string beneficiary_account_id
        string beneficiary_iban
        string beneficiary_sort_code
        string reference
        string end_to_end_id
        string scheme_message_id
    }

    PAYMENT_EVENT {
        uuid event_id PK
        uuid payment_id FK
        string event_type
        string previous_status
        string new_status
        string actor
        string reason
        timestamp event_timestamp
        string source_service
    }

    MANDATE {
        uuid mandate_id PK
        string rail
        string mandate_type
        string creditor_id
        string debtor_iban
        string debtor_account
        string status
        timestamp created_at
        timestamp cancelled_at
        string cancellation_reason
    }

    SETTLEMENT_POSITION {
        uuid position_id PK
        string rail
        string settlement_date
        decimal gross_debit
        decimal gross_credit
        decimal net_position
        string settlement_status
        timestamp settled_at
    }

    RECONCILIATION_RECORD {
        uuid recon_id PK
        string rail
        string settlement_date
        string settlement_report_ref
        int total_payments_hub
        int total_payments_scheme
        decimal total_amount_hub
        decimal total_amount_scheme
        int breaks_count
        string recon_status
        timestamp run_at
    }

    AUDIT_ENTRY {
        uuid audit_id PK
        uuid payment_id FK
        string event_type
        string actor_id
        string actor_role
        string action
        jsonb payload_hash
        string previous_hash
        timestamp recorded_at
    }

    CARD_AUTHORISATION {
        uuid auth_id PK
        string token_pan FK
        decimal amount
        string currency
        string merchant_id
        string merchant_category_code
        string response_code
        timestamp authorised_at
        string scheme_auth_id
    }

    PAYMENT ||--o{ PAYMENT_EVENT : "has lifecycle events"
    PAYMENT }o--o| MANDATE : "may reference"
    PAYMENT }o--|| SETTLEMENT_POSITION : "contributes to"
    SETTLEMENT_POSITION ||--o| RECONCILIATION_RECORD : "reconciled by"
    PAYMENT ||--o{ AUDIT_ENTRY : "audited by"
    CARD_AUTHORISATION ||--o{ AUDIT_ENTRY : "audited by"
```

| Business Object | Description |
|-----------------|-------------|
| Payment | The primary entity. Represents a single payment instruction from initiation to completion. Holds all scheme-relevant identifiers and lifecycle timestamps. |
| PaymentEvent | Immutable record of every state transition in the payment lifecycle. Supports end-to-end traceability and dispute resolution. |
| Mandate | BACS Direct Debit or SEPA SDD mandate. Records debtor consent for recurring collections. |
| SettlementPosition | Aggregated net position per rail per settlement cycle. Used by Treasury for liquidity management. |
| ReconciliationRecord | Result of the automated reconciliation run per rail per settlement date. Tracks breaks and resolution status. |
| AuditEntry | Immutable, hash-chained audit log entry for every significant event. Actor, action, timestamp, and payload hash recorded. |
| CardAuthorisation | Real-time card authorisation record. Stores tokenised PAN, amount, merchant, and scheme response. Linked to daily clearing records. |

## 7.3 Data Movement

```mermaid
flowchart LR
    subgraph Channels
        DB[Digital Banking Platform]
        OPS[Operations Dashboard]
    end
    subgraph PaymentHub["Payment Hub"]
        APIGW[API Gateway]
        ORCH[Orchestration Service]
        KAFKA[Event Bus / Kafka]
        FPS[FPS Connector]
        BACS[BACS Connector]
        CHAPS[CHAPS Connector]
        SEPA[SEPA Connector]
        CARD[Card Processing Service]
        RECON[Reconciliation Service]
        AUDIT[Audit Store]
        PS[Payment Store]
    end
    subgraph External
        PAYUK[Pay.UK / Vocalink]
        BOE[Bank of England RTGS]
        EBA[EBA STEP2 / RT1]
        VISA[Visa / Mastercard]
        CBS[Core Banking System]
        FRAUD[Fraud & AML Platform]
        NOTIF[Notification Gateway]
    end

    DB -->|REST HTTPS| APIGW
    OPS -->|REST HTTPS| APIGW
    APIGW --> ORCH
    ORCH -->|Sync REST| FRAUD
    ORCH --> KAFKA
    ORCH --> PS
    KAFKA --> FPS & BACS & CHAPS & SEPA & CARD
    KAFKA --> RECON & AUDIT
    FPS <-->|ISO 20022 pacs| PAYUK
    BACS <-->|pain.001/008| PAYUK
    CHAPS <-->|ISO 20022 MX| BOE
    SEPA <-->|ISO 20022 pacs| EBA
    CARD <-->|ISO 8583| VISA
    ORCH -->|REST HTTPS| CBS
    KAFKA -->|Async| NOTIF
```

| Id | Source | Target | Pattern | Business Data | Workload | Impact |
|----|--------|--------|---------|---------------|----------|--------|
| IF-01 | Digital Banking Platform | API Gateway | Sync REST / HTTPS | Payment instruction (amount, payee, rail). Contains customer PII (name, account). | 50 TPS peak (Phase 1), 250 TPS (Year 3) | New |
| IF-02 | API Gateway | Payment Orchestration | Sync REST / internal | Validated payment instruction | Same as IF-01 | New |
| IF-03 | Payment Orchestration | Fraud & AML Platform | Sync REST / HTTPS | Payment details for screening. Customer PII. Sensitive. | Same as IF-01 (every outbound payment) | New |
| IF-04 | Payment Orchestration | Event Bus (Kafka) | Async event stream | ISO 20022 payment lifecycle events. Contains payment reference data. | Same as IF-01 | New |
| IF-05 | FPS Connector | Pay.UK / Vocalink | Sync ISO 20022 / HTTPS | pacs.008 (outbound), pacs.002 (confirmation). Contains IBAN, account, amount. Sensitive. | 50 TPS peak | New |
| IF-06 | Pay.UK / Vocalink | FPS Connector | Sync ISO 20022 / HTTPS | Inbound pacs.008 (credit). Contains IBAN, account, amount. Sensitive. | 50 TPS peak | New |
| IF-07 | BACS Connector | Pay.UK / Vocalink | Batch / BACSTEL-IP | pain.001 bulk file (Direct Credit), pain.008 (Direct Debit). Bulk customer PII. Sensitive. | 100K payments/batch, 3 batches/day | New |
| IF-08 | CHAPS Connector | Bank of England RTGS | Sync ISO 20022 MX / SWIFTNet | pacs.008 MX (outbound), camt.054 (settlement confirmation). High-value amounts. Sensitive. | 500/day peak | New |
| IF-09 | SEPA Connector | EBA STEP2 / RT1 | Sync ISO 20022 / SWIFT gateway | pacs.008 (SCT / SCT Inst), pain.008 (SDD). EU customer PII. Sensitive. | 200 TPS (SCT Inst peak, Phase 3) | New |
| IF-10 | Card Processing Service | Visa / Mastercard | Sync ISO 8583 / Visa DPS | Authorisation request (tokenised PAN, amount, MCC). PCI-DSS scoped. | 200 TPS peak (Phase 2) | New |
| IF-11 | Visa / Mastercard | Card Processing Service | Batch / SFTP | Daily clearing file (IPM / ISO 8583). PCI-DSS scoped. | 1 file/day, 50K records peak | New |
| IF-12 | Payment Orchestration | Core Banking Adapter | Sync REST / HTTPS | Posting instruction (account ID, amount, direction, payment reference). Sensitive. | Same as IF-01 | New |
| IF-13 | Core Banking Adapter | Core Banking System | Sync REST / HTTPS | Debit/credit posting instruction. Account balance impact. Sensitive. | Same as IF-01 | New |
| IF-14 | Event Bus | Audit Store | Async / Kafka Connect | All payment lifecycle events. Full event payload. Sensitive. Retained 7 years. | All payment events | New |
| IF-15 | Reconciliation Service | Scheme settlement reports | Batch / SFTP or API | End-of-day settlement position files per rail. Financial data. | 1–5 files/day per rail | New |
| IF-16 | Event Bus | Notification Service | Async event | Payment status change events (confirmed, failed, held). Customer reference only. | Same as IF-01 | New |
| IF-17 | Notification Service | Notification Gateway | Sync REST / HTTPS | Customer notification payload (name, masked amount, reference). PII. | Same as IF-01 | New |

## 7.4 Data Persistence

| Id | Entity | Retention Ref | Retention Period | Archiving | Data Classification | Record Size | Daily Volume | Annual Size (GB) |
|----|--------|---------------|------------------|-----------|---------------------|-------------|--------------|------------------|
| DS-01 | Payment records (Payment Store) | REF-03 | 7 years | Yes — to S3 after 2 years | Sensitive / PII | ~2 KB | 200K records | ~53 GB |
| DS-02 | Payment events (Payment Store) | REF-03 | 7 years | Yes — to S3 after 2 years | Sensitive | ~1 KB | 1M events | ~365 GB |
| DS-03 | Audit entries (Audit Store) | REF-03 | Permanent (min 7 years) | No — retained in OpenSearch/S3 | Sensitive | ~1 KB | 1M entries | ~365 GB |
| DS-04 | Kafka event bus messages | Operational | 7 days | No | Internal | ~2 KB | 1M messages | Operational only |
| DS-05 | Card authorisation records | REF-03 / PCI-DSS | 7 years | Yes | PCI-DSS / Sensitive | ~500 B | 500K records (Phase 2) | ~92 GB |
| DS-06 | Mandate records (BACS / SDD) | REF-03 | 7 years post-cancellation | Yes | Sensitive / PII | ~1 KB | 5K records | ~2 GB |
| DS-07 | Reconciliation records | REF-03 | 7 years | Yes | Internal | ~5 KB | 10 records (one per rail/day) | < 1 GB |
| DS-08 | Application logs | REF-08 | 90 days | No | Internal (masked) | ~500 B | 50M lines | ~25 GB |

---

# 8 Infrastructure Architecture

## 8.1 Cloud & Data Centre

**Cloud:**

- All Payment Hub workloads are deployed on a managed cloud platform (provider TBD — OQ-01). Containerised workloads run on managed Kubernetes (e.g. AWS EKS, Azure AKS, GCP GKE).
- Infrastructure-as-code (Terraform) is mandatory for all provisioned resources. No manually provisioned infrastructure is permitted in any environment.
- Managed Kafka (e.g. Amazon MSK, Confluent Cloud, or Azure Event Hubs for Kafka) used to reduce operational overhead for the event bus.
- Managed PostgreSQL (e.g. AWS RDS for PostgreSQL, Azure Database for PostgreSQL) with synchronous replication across two availability zones.
- Object storage (S3 or equivalent) used for long-term archival of payment records, audit logs, and scheme settlement files.

**On-Premises:**

- No on-premises compute is required for the Payment Hub. All workloads are cloud-native.
- SWIFTNet connectivity for CHAPS (Phase 2) may require a SWIFT Alliance Gateway virtual appliance deployed in a dedicated cloud subnet or colocation facility — to be determined during Phase 2 infrastructure design.

**Deployment Model:**

- **Primary Region:** Single cloud region, dual availability zone active-active deployment for all Tier 1 components.
- **Secondary Region (DR):** Warm-standby in a secondary cloud region. Kafka cross-region replication. PostgreSQL asynchronous read replica in DR region. RTO < 15 minutes; RPO < 1 minute.
- **UK Data Residency:** All workloads processing UK customer data must reside in UK cloud regions. EU data residency required for EU entity (Phase 3).

## 8.2 Network Deployment

```mermaid
flowchart TB
    subgraph Internet["Internet / External Networks"]
        Customers[Customer Devices]
        SchemeNetworks[Scheme Networks\nPay.UK / SWIFTNet / EBA]
    end

    subgraph CloudRegion["Cloud Region — Primary (UK)"]
        subgraph PublicSubnet["Public Subnet — DMZ"]
            APIGW[API Gateway\n+ WAF]
            NLB[Network Load Balancer]
        end

        subgraph AppSubnet["Private Subnet — Application Tier"]
            ORCH[Orchestration Service]
            CONNECTORS[Rail Connectors\nFPS / BACS / CHAPS / SEPA]
            CARD[Card Processing Service\nCDE Segment]
            NOTIF[Notification Service]
            RECON[Reconciliation Service]
            STATUS[Payment Status API]
        end

        subgraph DataSubnet["Private Subnet — Data Tier"]
            PG[(PostgreSQL\nPayment Store)]
            OS[(OpenSearch\nAudit Store)]
            KAFKA[Kafka Cluster]
            VAULT[HashiCorp Vault]
        end

        subgraph CDESubnet["Isolated Subnet — Card Data Environment (PCI)"]
            CARDAPI[Card Scheme Adapter]
            TOKENVAULT[Tokenisation Vault]
        end

        subgraph MgmtSubnet["Management Subnet"]
            OPSDASH[Operations Dashboard]
            CICD[CI/CD Pipeline]
            MONITORING[Monitoring Stack]
        end
    end

    subgraph DRRegion["Cloud Region — Secondary (UK DR)"]
        PGDR[(PostgreSQL Read Replica)]
        KAFKADR[Kafka Cross-Region Replica]
    end

    Customers -->|HTTPS TLS 1.3| APIGW
    SchemeNetworks -->|ISO 20022 / BACSTEL-IP / SWIFTNet| NLB
    APIGW --> ORCH
    NLB --> CONNECTORS
    ORCH <--> KAFKA
    CONNECTORS <--> KAFKA
    ORCH --> PG
    KAFKA --> OS
    ORCH --> VAULT
    CONNECTORS --> VAULT
    CARD --> CDESubnet
    CARDAPI --> TOKENVAULT
    PG -.->|Async replication| PGDR
    KAFKA -.->|Cross-region replication| KAFKADR
    OPSDASH --> STATUS
    CICD --> AppSubnet
    MONITORING --> AppSubnet & DataSubnet
```

## 8.3 Resilience

| Resilience Area | Approach & Impact |
|-----------------|-------------------|
| Resilience | All Tier 1 components (Orchestration, Connectors, Kafka, PostgreSQL) deployed active-active across two availability zones. Minimum 3 pod replicas per service with pod anti-affinity rules ensuring zone distribution. No single points of failure in the data or application tier. Kafka replication factor of 3 with minimum in-sync replicas of 2. |
| Disaster Recovery | Warm-standby DR in a secondary UK region. Kafka cross-region replication for event continuity. PostgreSQL asynchronous replica in DR region (RPO < 1 minute for in-flight transactions). IaC (Terraform) enables full environment rebuild. Formal DR runbook with tested failover procedure. Target RTO: < 15 minutes. |
| Backup and Recovery | PostgreSQL: continuous WAL archiving to object storage + daily snapshots. PITR enabled with 35-day retention. Kafka: 7-day message retention enables event replay for recovery. Audit Store (S3): versioning and object lock enabled to prevent deletion. Scheme settlement files: archived to S3 with 7-year retention. |
| Circuit Breakers | Circuit breakers implemented on all outbound scheme connector calls (FPS, BACS, CHAPS, SEPA) and on the Fraud & AML Platform integration. Open circuit prevents cascade failure when a dependency is degraded. Fallback: payment held in exception queue pending scheme recovery. |
| Idempotency | All payment submissions carry a unique idempotency key (payment_id + end_to_end_id). Exactly-once processing enforced at the Orchestration Service. Duplicate submissions detected and rejected with the original response returned. |

## 8.4 Performance & Workload

| Performance Area | Approach & Impact |
|-----------------|-------------------|
| Demand Management | Kubernetes Horizontal Pod Autoscaler (HPA) scales all stateless services based on CPU and custom Kafka consumer lag metrics. FPS Connector and Orchestration Service scale independently. BACS bulk file processing scheduled outside peak FPS window to avoid resource contention. Card authorisation path dedicated pod pool for predictable latency. |
| Resource Management | Initial sizing based on Phase 1 volumes (50 FPS TPS, 100K BACS payments/batch). All services sized to Year 3 peak (250 FPS TPS, 1,000 card TPS) with HPA configured to burst. Kafka partitioned for throughput (minimum 12 partitions per topic). PostgreSQL connection pooling via PgBouncer to manage connection limits under peak load. Performance baseline established in load testing before Phase 1 go-live. |

## 8.5 Technology

| Id | Name | Version | Status | Purpose |
|----|------|---------|--------|---------|
| APP-01 | Kubernetes (managed) | 1.29+ | New-Proposed | Container orchestration for all Payment Hub services |
| APP-02 | Istio Service Mesh | 1.21+ | New-Proposed | mTLS enforcement, traffic management, observability for east-west service communication |
| APP-03 | Apache Kafka (managed) | 3.7+ | New-Proposed | Event streaming platform for all payment lifecycle events |
| APP-04 | Confluent Schema Registry | 7.6+ | New-Proposed | Kafka message schema governance and compatibility enforcement |
| APP-05 | PostgreSQL (managed) | 16+ | New-Proposed | Primary OLTP store for Payment Store |
| APP-06 | PgBouncer | 1.22+ | New-Proposed | PostgreSQL connection pooling for high-throughput write workloads |
| APP-07 | OpenSearch | 2.x | New-Proposed | Audit and compliance log store — append-only, 7-year retention |
| APP-08 | HashiCorp Vault | 1.16+ | New-Proposed | Secret management, certificate issuance, dynamic credentials |
| APP-09 | Kong API Gateway | 3.x | New-Proposed | North-south API traffic management, OAuth 2.0 enforcement, rate limiting |
| APP-10 | Java 21 / Spring Boot 3.x | 3.3+ | New-Proposed | Primary runtime for all Payment Hub microservices |
| APP-11 | Python 3.12 | 3.12 | New-Proposed | Reconciliation Service and data processing components |
| APP-12 | Node.js 22 / TypeScript | 22 LTS | New-Proposed | Notification Service and Operations Dashboard backend |
| APP-13 | React 18 / TypeScript | 18 | New-Proposed | Operations Dashboard frontend |
| APP-14 | Terraform | 1.8+ | New-Proposed | Infrastructure-as-code for all cloud resources |
| APP-15 | ArgoCD | 2.11+ | New-Proposed | GitOps-based Kubernetes deployment and continuous delivery |
| APP-16 | Prometheus + Grafana | Latest | New-Proposed | Infrastructure and application metrics collection and dashboarding |
| APP-17 | OpenTelemetry | 1.x | New-Proposed | Distributed tracing and APM instrumentation across all services |
| APP-18 | Jaeger | 1.x | New-Proposed | Distributed trace visualisation and end-to-end payment tracing |
| APP-19 | SonarQube | Latest | New-Proposed | Static code analysis and security vulnerability scanning |
| APP-20 | Trivy | Latest | New-Proposed | Container image vulnerability scanning in CI/CD pipeline |
| APP-21 | cert-manager | 1.14+ | New-Proposed | Automated TLS certificate management within Kubernetes |

---

# 9 Security Architecture

## 9.1 Security Overview

The Payment Hub security model is built on zero-trust principles. No service implicitly trusts another, regardless of network location. All communication is authenticated, encrypted, and authorised. The Card Data Environment (CDE) is isolated from the broader Payment Hub and is subject to PCI-DSS Level 1 controls.

```mermaid
flowchart TB
    subgraph ExternalZone["External Zone"]
        Customers[Customer Devices]
        Schemes[Scheme Networks]
    end

    subgraph DMZ["DMZ — TLS Termination + WAF"]
        WAF[WAF + DDoS Protection]
        APIGW[API Gateway\nOAuth 2.0 / mTLS]
    end

    subgraph AppZone["Application Zone — mTLS enforced by Istio"]
        ORCH[Orchestration Service]
        CONNECTORS[Rail Connectors]
        NOTIF[Notification Service]
        STATUS[Status API]
    end

    subgraph DataZone["Data Zone — Encrypted at rest"]
        PG[(PostgreSQL\nAES-256)]
        KAFKA[Kafka\nAES-256]
        OS[(Audit Store\nAES-256)]
        VAULT[HashiCorp Vault\nFIPS 140-2]
    end

    subgraph CDE["Card Data Environment — PCI-DSS L1 Isolated"]
        CARDADAPTER[Card Scheme Adapter]
        TOKENISER[Tokenisation Service]
    end

    subgraph MgmtZone["Management Zone — Bastion / Break-Glass"]
        BASTION[Bastion Host\n+ MFA]
        CICD[CI/CD Pipeline\n+ SAST/DAST]
    end

    Customers -->|TLS 1.3 + OAuth 2.0| WAF --> APIGW
    Schemes -->|mTLS + IP allowlist| APIGW
    APIGW -->|mTLS via Istio| AppZone
    AppZone -->|mTLS via Istio| DataZone
    AppZone -->|mTLS — CDE boundary| CDE
    VAULT -->|Dynamic secrets| AppZone & DataZone
    BASTION -->|Audited SSH / just-in-time access| MgmtZone
    CICD -->|Signed images, IaC only| AppZone
```

## 9.2 Security Controls

| Control Reference | Description | Architecture Compliance |
|-------------------|-------------|-------------------------|
| **Access Controls** | | |
| AC-01 | Access Control Policy and Procedures | All access governed by RBAC. Staff access to the Operations Dashboard requires an authenticated session via the corporate Identity Provider (SSO). Service access governed by Vault policies. No shared credentials. |
| AC-05 | Separation of Duties | Production access is break-glass only via a bastion host, requiring two-person authorisation. Deployment to production is automated via ArgoCD — no engineer has direct kubectl access to production. CI/CD pipeline enforces peer review and approval gates. |
| **Audit & Accountability** | | |
| AU-02 | Auditable Events | All payment lifecycle events, authentication attempts, configuration changes, and access to the Operations Dashboard are logged to the centralised Audit Store (OpenSearch). Log entries include: actor ID, action, resource, timestamp, and outcome. |
| **Configuration Management** | | |
| CM-03 | Configuration Change Control | All infrastructure changes made via Terraform with peer-reviewed pull requests. Kubernetes configuration changes via ArgoCD GitOps — all changes are tracked in Git. No ad-hoc configuration changes permitted in production. |
| CM-07 | Least Functionality | Kubernetes pods run with read-only root filesystems, dropped Linux capabilities, and non-root user contexts. Unnecessary ports are not exposed. Break-glass access for production is audited and time-limited via Vault dynamic credentials. |
| CM-08 | Component Inventory & Vulnerability Scanning | All container images scanned by Trivy in the CI pipeline. SonarQube performs SAST on all application code. Dependency vulnerability scanning via OWASP Dependency Check. Quarterly penetration testing by accredited third party. |
| **Contingency Planning** | | |
| CP-10 | System Recovery and Reconstitution | Full environment can be rebuilt from Terraform IaC in < 15 minutes to an empty cloud account. Payment records restored from PostgreSQL point-in-time recovery (RPO < 1 minute). Kafka replay recovers in-flight events. DR runbook tested quarterly. |
| **Identification and Authentication** | | |
| IA-09 | Service Identification and Authentication | All service-to-service communication uses mutual TLS (mTLS) enforced by the Istio service mesh. Service identity is established via SPIFFE/SPIRE X.509 certificates issued by Vault PKI. Certificates rotated automatically every 24 hours. |
| **System and Communications Protection** | | |
| SC-13 | Cryptographic Protection | TLS 1.3 minimum for all external communication. TLS 1.2 minimum for internal service mesh (Istio). AES-256-GCM for data at rest (PostgreSQL, Kafka, OpenSearch, S3). RSA-4096 or ECDSA P-384 for service identity certificates. Card PANs tokenised using Format-Preserving Encryption (FPE/AES-FF3). Key management via HashiCorp Vault (FIPS 140-2 compliant HSM backend for key sealing). |
| **System and Information Integrity** | | |
| SI-04 | Information System Monitoring | Prometheus + Grafana for infrastructure and application metrics. OpenTelemetry + Jaeger for distributed tracing across all payment flows. Anomaly detection alerts configured for abnormal payment volumes, error rates, and scheme connectivity failures. |
| SI-05 | Security Alerts and Advisories | Security events from the Audit Store trigger PagerDuty alerts for the Security Operations Centre (SOC). Critical alerts include: authentication failures exceeding threshold, CDE access events, payment volume anomalies, and scheme connectivity loss. |

## 9.3 Data Security

| Security Area | Approach |
|---------------|----------|
| Data-at-Rest | All PostgreSQL, Kafka, OpenSearch, and S3 stores are encrypted using AES-256 via cloud-provider managed keys (KMS) with customer-managed key (CMK) wrapping. Key rotation every 12 months. |
| Data in Transit | TLS 1.3 for all external communication. TLS 1.2 minimum (TLS 1.3 preferred) for internal east-west traffic via Istio mTLS. No plain-text communication channels permitted. All scheme connectivity (Pay.UK, CHAPS SWIFTNet, EBA) encrypted at the transport layer. |
| Data in Archive | All archived payment records in S3 are encrypted using AES-256 with CMK. S3 Object Lock (compliance mode) prevents deletion or modification. Retained for 7 years minimum. |
| Masking | IBAN, account numbers, sort codes, and card PANs are masked in all application logs (last 4 characters shown only). Customer names and reference data masked in non-production environments. Masking applied at the logging framework level — services may not log full PII under any configuration. |
| Field Validation | All inbound payment requests validated at the API Gateway (schema validation) and again at the ISO 20022 Message Translation Engine (scheme-specific rulebook validation). Input length, format, and character set enforced. SQL injection and injection attack prevention via parameterised queries only — no dynamic SQL. |

## 9.4 Network Security

| Security Area | Approach |
|---------------|----------|
| Perimeter Security | Cloud WAF with OWASP rule set applied to all public-facing endpoints. DDoS protection enabled at the cloud load balancer. API Gateway enforces rate limiting per client ID. Scheme network connectivity (SWIFTNet, Pay.UK gateway) is IP-allowlisted and certificate-authenticated — not internet-exposed. |
| Internal Security | Istio service mesh enforces mTLS for all east-west traffic within the Kubernetes cluster. Kubernetes NetworkPolicy objects restrict pod-to-pod communication to explicitly permitted flows only. No service communicates with another unless there is a defined NetworkPolicy rule. |
| Zonal Segregation | Four network zones: DMZ (public ingress), Application Zone (services), Data Zone (databases and Kafka), and CDE (PCI-DSS isolated). Security groups enforce zone boundaries. Cross-zone traffic restricted to defined ingress/egress rules. CDE has no outbound internet access. Management Zone accessible only via bastion host with MFA and just-in-time access. |
| Key / Secret Management | All secrets (API keys, database passwords, scheme connection credentials) stored in HashiCorp Vault. Services retrieve secrets at runtime via the Vault Agent sidecar — no secrets in environment variables or container images. Dynamic database credentials rotated every hour. TLS certificates auto-renewed by cert-manager before expiry. Vault is backed by an HSM-sealed root key (FIPS 140-2 Level 3). |

## 9.5 Identity & Access Management

| Security Area | Approach |
|---------------|----------|
| Identity Governance & RBAC | Staff identity governed by the corporate Identity Provider (IdP — TBD, OQ-07). RBAC roles defined for: Payment Operations Analyst, Financial Crime Analyst, Compliance Officer, Infrastructure Engineer, and System Administrator. Role assignments are recertified quarterly. Service identity via SPIFFE/SPIRE certificates issued by Vault PKI. |
| Single Sign-On | Operations Dashboard and all internal tooling integrated with the corporate IdP via OIDC/SAML 2.0. No local user accounts in application services. Customer authentication is delegated to the Digital Banking Platform — Payment Hub does not perform customer authentication directly. |
| Conditional Access | Conditional access policies applied via the corporate IdP: MFA required for all staff access. Access to Production Operations Dashboard restricted to corporate network or approved VPN. CDE access requires additional step-up authentication and is time-limited. |
| Service Access Policies | API Gateway enforces OAuth 2.0 JWT token validation for all inbound API calls. Token audience, scope, and expiry are validated. Service-to-service calls authorised via Istio AuthorizationPolicy (SPIFFE identity-based). Vault policies restrict each service to only the secrets it requires. |

---

# 10 Operations

## 10.1 Operations Approach & Impact

| Concern | Approach & Impact |
|---------|-------------------|
| Resilience & Recovery | All Tier 1 components are deployed active-active. On component failure, Kubernetes self-healing (pod restart) and active-active routing ensure automatic recovery for transient failures. For zone failure, load balancer automatically routes to the surviving zone. For region failure, the DR runbook is invoked: failover to secondary region within RTO of 15 minutes. DR tested quarterly. |
| System Change | Rolling deployments via ArgoCD. Zero-downtime deployments enforced for all production changes. Database schema changes managed via Flyway — backward-compatible migrations only (no column drops in the same release as column rename). Blue/green deployment for major releases where rolling upgrade is not safe. Feature flags used to decouple deployment from feature activation where required. |

## 10.2 Delivery Approach & Impact

The Payment Hub is delivered via a GitOps CI/CD pipeline:

**Pipeline stages (per service):**
1. **Code commit** → Automated unit tests, SAST (SonarQube), dependency vulnerability scan (OWASP DC).
2. **Build** → Docker image built and scanned by Trivy. Image signed (Cosign / Sigstore).
3. **Deploy to Dev** → ArgoCD applies to Development cluster automatically on merge to `main`.
4. **Integration Test** → Automated integration tests against stub scheme connectors and Core Banking stub.
5. **Deploy to Staging** → ArgoCD applies after integration tests pass. Requires one peer approval.
6. **Performance Test** → Automated load tests run against Staging using realistic volume profiles.
7. **Deploy to Production** → Requires two approvals (Engineering Lead + Release Manager). ArgoCD applies rolling deployment. Canary release for first 10% of traffic before full rollout.

**Environments:** Development → Integration → Staging → Production → DR (DR environment mirrors Production; tested quarterly).

**Infrastructure changes:** All via Terraform with peer-reviewed pull requests and plan review before apply. Terraform state stored in remote backend (cloud object storage) with state locking.

## 10.3 Failure Impact

| Application / Component | Mitigation of Failure | Business Impact |
|-------------------------|-----------------------|-----------------|
| Payment Orchestration Service | Kubernetes restarts failed pods. Active-active across 2 AZs — traffic rerouted automatically. Minimum 3 replicas. | High — new payment submissions fail if all replicas down. In-flight events on Kafka can be replayed on recovery. Maximum expected downtime: < 30 seconds for pod restart. |
| Kafka Event Bus | Replication factor 3, min ISR 2. Zone failure tolerated. Cross-region replication for DR. | High — without Kafka, async payment processing halts. Synchronous fallback not available; payments queue at Orchestration level. RTO < 15 minutes from IaC rebuild. |
| FPS Connector Service | Kubernetes restarts. Active-active. Outbound FPS payments queue in Kafka — no payments lost. | High for FPS customers — new FPS payments delayed until connector recovers. Queued payments processed in order on recovery. |
| Payment Store (PostgreSQL) | Managed HA with automatic failover to standby replica (< 30 seconds). PITR for data recovery. | High — payment records unavailable during failover. All writes and reads resume automatically after failover. |
| Core Banking Adapter | Kubernetes restarts. Idempotency ensures retry safety. Payment held in PENDING state if Core Banking unreachable. | High — ledger posting delayed. Payment confirmed at scheme but not yet posted. Ops team alerted immediately. Retry with backoff until Core Banking recovers. |
| Fraud & AML Platform | Circuit breaker opens after timeout threshold. Payment held in exception queue for manual review. | Medium — outbound payment processing delayed; no payments automatically bypassed. Ops team reviews held queue. |
| Card Processing Service | Kubernetes restarts. Card authorisations fail closed (declined) if service unavailable. | High (Phase 2) — card declines impact customer experience. Visa/Mastercard provide scheme-level fallback authorisation in some configurations. |
| Operations Dashboard | Kubernetes restarts. Static build served from CDN. | Low — operations visibility reduced but payment processing continues unaffected. |
| Audit Store (OpenSearch) | Kafka consumer retries with backoff. Events retained on Kafka for 7 days pending recovery. | Low — audit records delayed but not lost. Kafka replay restores all events on recovery. |

## 10.4 Monitoring & Alerting

| Area | Description |
|------|-------------|
| Auditing | All payment lifecycle events, authentication events, configuration changes, and production access events are streamed to the Audit Store (OpenSearch) via Kafka. Audit entries are immutable and hash-chained. Access to audit data is restricted to Compliance and Security roles. Audit data retained for 7 years minimum. |
| Logging | All services emit structured JSON logs with mandatory fields: correlation_id (payment_id), service_name, log_level, timestamp, and message. PII and sensitive payment data masked at log emission. Logs aggregated in a centralised logging platform (e.g. OpenSearch / CloudWatch Logs). Log retention: 90 days in hot store; archived to object storage for compliance period. |
| Alerting | PagerDuty integration for all production alerts. Alert routing: P1 (payment processing down, scheme connectivity lost, Kafka failure) → immediate page to on-call engineer + Payment Operations Lead. P2 (elevated error rate, reconciliation breaks, fraud screening latency) → page to on-call engineer within 15 minutes. P3 (non-critical warnings, capacity approaching threshold) → Slack notification + JIRA ticket. All alerts reviewed weekly for noise reduction. |
| Proactive Monitoring | **Infrastructure:** Prometheus + Grafana dashboards for CPU, memory, pod health, Kafka consumer lag, and database connection pool utilisation. **Application:** OpenTelemetry traces for end-to-end payment journey latency. Jaeger for trace visualisation. **Business Metrics:** Grafana dashboard for payment volumes per rail, error rates per rail, scheme connectivity status, and reconciliation break count. **Capacity:** Monthly capacity review against volume projections. HPA thresholds reviewed quarterly. |

## 10.5 Support & Acceptance into Service

| Level | System | Owner | Description | Third Party Info |
|-------|--------|-------|-------------|-----------------|
| L1 | All Payment Hub rails | Payment Operations Centre (POC) | First-line support. Available 24/7. Monitors Operations Dashboard, manages exception queue, handles customer payment query escalations from Customer Service. SLA: 15-minute response for P1, 1-hour for P2. | N/A |
| L1 | Card Processing | Card Operations Team | First-line support for card-specific queries (authorisation failures, chargeback initiation). Available 24/7. | Visa/Mastercard support portal — scheme-level issues escalated directly. |
| L2 | Payment Hub (all components) | Payment Engineering SRE Team | Second-line technical escalation. Diagnoses and resolves service-level failures, Kafka consumer lag, database issues, and scheme connector failures. Available 24/7 on-call via PagerDuty. SLA: 30-minute response for P1. | Managed Kafka vendor support (if applicable). Cloud provider enterprise support. |
| L2 | Security & Access | Security Operations Centre (SOC) | Second-line security escalation for suspected fraud, security events, PCI incidents, and access anomalies. Available 24/7. | HashiCorp Vault enterprise support. Cloud provider security support. |
| L3 | Scheme Connectivity | Head of Payments + Scheme Manager | Escalation for scheme rule breaches, settlement failures, and membership issues. Business-hours primary; emergency contact 24/7 for P1 settlement failures. | Pay.UK, Bank of England, EBA Clearing, Visa/Mastercard scheme support lines. |

---

# A. Glossary

| Term | Definition | Status |
|------|------------|--------|
| SAD | Solution Architecture Description | Existing |
| ADR | Architecture Decision Record | Existing |
| RR | Risk Rating | Existing |
| EDA | Event Driven Architecture | Existing |
| IaC | Infrastructure as Code | Existing |
| SOC | Security Operations Centre | Existing |
| IAM | Identity and Access Management | Existing |
| TLS | Transport Layer Security | Existing |
| PII | Personally Identifiable Information | Existing |
| RTO | Recovery Time Objective | Existing |
| RPO | Recovery Point Objective | Existing |
| RBAC | Role-Based Access Control | Existing |
| FPS | Faster Payments Service — UK real-time credit transfer scheme operated by Pay.UK | New |
| BACS | Bankers Automated Clearing Services — UK three-day bulk payment scheme | New |
| CHAPS | Clearing House Automated Payment System — UK high-value same-day RTGS scheme operated by the Bank of England | New |
| SEPA | Single Euro Payments Area — EU payment integration zone | New |
| SCT | SEPA Credit Transfer — standard EU credit transfer (T+1 settlement) | New |
| SCT Inst | SEPA Instant Credit Transfer — real-time EU credit transfer (10-second SLA) | New |
| SDD | SEPA Direct Debit — EU direct debit scheme | New |
| ISO 20022 | International financial messaging standard. Defines XML message schemas for payment instruction, status, and account reporting. | New |
| pacs.008 | ISO 20022 message type — Financial Institution Credit Transfer (customer credit transfer) | New |
| pacs.002 | ISO 20022 message type — Financial Institution Credit Transfer Status Report (payment confirmation) | New |
| pain.001 | ISO 20022 message type — Customer Credit Transfer Initiation (bulk credit file) | New |
| pain.008 | ISO 20022 message type — Customer Direct Debit Initiation (direct debit file) | New |
| camt.054 | ISO 20022 message type — Bank To Customer Debit Credit Notification (settlement confirmation) | New |
| RTGS | Real-Time Gross Settlement — settlement of funds on a transaction-by-transaction basis in real time | New |
| CDE | Cardholder Data Environment — PCI-DSS scoped environment handling card primary account numbers | New |
| PAN | Primary Account Number — the 16-digit card number | New |
| mTLS | Mutual TLS — TLS where both client and server authenticate with certificates | New |
| SPIFFE | Secure Production Identity Framework for Everyone — standard for service identity in cloud-native environments | New |
| POC | Payment Operations Centre — ABC Neo Bank's first-line payment operations team | New |
| HPA | Horizontal Pod Autoscaler — Kubernetes feature for automatic service scaling | New |
| BACSTEL-IP | BACS Telecommunications — the secure IP network used to submit BACS payment files to Vocalink | New |
| STEP2 | EBA Clearing's pan-European ACH for SEPA Credit Transfers | New |
| RT1 | EBA Clearing's pan-European instant payment infrastructure for SEPA Instant Credit Transfers | New |
| TIPS | TARGET Instant Payment Settlement — ECB's central bank money settlement for SEPA Instant payments | New |
| BIN | Bank Identification Number — the first 6-8 digits of a card number identifying the issuer | New |
| QSA | Qualified Security Assessor — PCI-DSS accredited auditor | New |
| FPE | Format-Preserving Encryption — encryption that preserves the format of the original data (used for PAN tokenisation) | New |
| GitOps | Operational model where Git is the single source of truth for infrastructure and application configuration | New |

---

# B. Appendix

## B.1 Payment Lifecycle State Machine

```mermaid
stateDiagram-v2
    [*] --> INITIATED : Payment initiation received
    INITIATED --> VALIDATING : ISO 20022 validation
    VALIDATING --> REJECTED : Validation failure
    VALIDATING --> SCREENING : Validation passed
    SCREENING --> HELD : Fraud / Sanctions hold
    SCREENING --> PENDING : Screening cleared
    HELD --> PENDING : Ops manual approval
    HELD --> REJECTED : Ops manual reject
    PENDING --> SUBMITTED : Sent to scheme
    SUBMITTED --> CONFIRMED : Scheme accepts (pacs.002 positive)
    SUBMITTED --> FAILED : Scheme rejects (pacs.002 negative)
    CONFIRMED --> POSTED : Core Banking debit/credit posted
    POSTED --> COMPLETED : End-to-end confirmed
    COMPLETED --> RECALLED : Recall initiated
    RECALLED --> RETURNED : Funds returned
    RETURNED --> [*]
    COMPLETED --> [*]
    REJECTED --> [*]
    FAILED --> [*]
```

## B.2 Phase Delivery Summary

| Phase | Rails | Key Milestones | Target |
|-------|-------|----------------|--------|
| Phase 1 | FPS (inbound + outbound), BACS (Direct Credit + Direct Debit) | Foundation complete, FPS go-live, BACS go-live, FCA payment capability sign-off | Q4 2026 |
| Phase 2 | CHAPS, Card Issuing (Visa/Mastercard), Open Banking APIs | CHAPS go-live, Cards go-live, PCI-DSS L1 audit passed, Open Banking TPP integration | Q2 2027 |
| Phase 3 | SEPA SCT, SCT Inst, SDD Core | EU entity live, SEPA reachability, DORA compliance confirmed | Q4 2027 |

## B.3 Open Questions Register

| Id | Question | Owner | Target Date | Impact if Unresolved |
|----|----------|-------|-------------|---------------------|
| OQ-01 | Cloud provider selection | CTO | 2026-04-01 | Infrastructure design and IaC cannot be finalised |
| OQ-02 | Programme budget | CFO | 2026-04-01 | Resource and tooling decisions constrained |
| OQ-03 | Engineering team size and capability | Engineering Director | 2026-04-01 | Build vs buy decisions for some components |
| OQ-04 | Scheme access model (bureau vs direct) | Head of Payments | 2026-04-01 | Connector design and cost model differ significantly |
| OQ-05 | Core Banking platform selection | CTO | 2026-04-15 | Core Banking Adapter design cannot be finalised |
| OQ-06 | Fraud & AML platform selection | CRO | 2026-04-15 | Adapter design and SLA expectations cannot be confirmed |
| OQ-07 | Identity Provider / IAM platform | CISO | 2026-04-15 | SSO and RBAC integration design blocked |
| OQ-08 | SEPA SDD B2B scope | Head of Payments | 2026-05-01 | Phase 3 SEPA connector scope |
| OQ-09 | Card BIN sponsorship / direct membership | Head of Cards | 2026-04-15 | Card Processing Service scheme connectivity design |
| OQ-10 | Card processor selection (in-house vs third party) | CTO / Head of Cards | 2026-04-01 | Card clearing/settlement design and PCI scope |
| OQ-11 | RTO / RPO targets per rail | CRO / Head of Payments | 2026-04-15 | DR design and resilience investment levels |
| OQ-12 | DORA compliance applicability | Compliance Director | 2026-04-01 | Operational resilience obligations for EU entity |

## B.4 ADR Backlog

The following ADRs are required and will be produced as full Architecture Decision Records:

| Id | Decision | Phase | Status |
|----|----------|-------|--------|
| ADR-001 | Event-driven architecture with Apache Kafka | Phase 1 | Summarised in §3.1 — full ADR required |
| ADR-002 | ISO 20022 as canonical internal message standard | Phase 1 | Summarised in §3.1 — full ADR required |
| ADR-003 | Microservices with Connector pattern | Phase 1 | Summarised in §3.1 — full ADR required |
| ADR-004 | Kubernetes + Istio deployment platform | Phase 1 | Summarised in §3.1 — full ADR required |
| ADR-005 | Bureau scheme access for Phase 1 | Phase 1 | Summarised in §3.1 — full ADR required |
| ADR-006 | Card processing — in-house auth, third-party clearing | Phase 2 | Proposed — pending OQ-10 |
| ADR-007 | PostgreSQL as primary payment store | Phase 1 | Summarised in §3.1 — full ADR required |
| ADR-008 | HashiCorp Vault for secret and certificate management | Phase 1 | Required |
| ADR-009 | Zero-trust networking via Istio mTLS | Phase 1 | Required |
| ADR-010 | Cloud provider selection | Phase 1 | Blocked — pending OQ-01 |
