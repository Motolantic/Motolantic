# MOTOLANTIC — Locked Architecture & Repository Checkpoint

**Checkpoint date:** 12 October 2026  
**Status:** Working architecture checkpoint  
**Purpose:** Consolidated record of Motolantic decisions that have been locked during repository screening and architecture planning.

> **Important:** “STATUS KUAT” means a repository is strong enough for deeper technical evaluation. It does **not** mean every repository will be installed in production. Alternatives will be reduced during the final architecture selection and MVP implementation.

---

## 1. Motolantic Product Model

Motolantic is planned as an Indonesian automotive spare-parts import intermediary.

Core customer flow:

1. Customer searches by part number, vehicle, or part description.
2. Motolantic identifies the required part and compatibility.
3. Supplier options are sourced.
4. Customer sees item price, estimated shipping/import costs, ETA, and total estimate.
5. Customer orders through Motolantic.
6. Motolantic coordinates supplier purchase, international shipping/import, and Indonesian last-mile delivery.
7. Motolantic earns a transaction commission, with the working concept around USD/EUR 3–4 per order when the order enters the shipping stage.

Initial product focus:
- Suspension / shock absorbers / undercarriage.
- Engine components such as fuel pumps, spark plugs, and ignition coils.
- Electrical components / alternators.

Initial vehicle focus:
- Toyota Avanza.
- Toyota Innova Reborn.
- Toyota Innova Zenix.
- Modern Toyota Supra.
- Mitsubishi Pajero Sport.
- Mitsubishi Xpander.
- BYD Atto 1.

MVP target: approximately three months.

---

# 2. Locked Technical Direction

## Web Platform

**LOCKED / FROZEN repository hunt.**

Primary language/runtime direction:
- TypeScript.
- Node.js.
- Next.js ecosystem where appropriate.
- PostgreSQL is the likely authoritative relational database.

**27 repositories have STATUS KUAT.**

Do not add additional Web Platform repositories unless the category is explicitly reopened.

## AI Operations

**LOCKED / FROZEN repository hunt.**

Primary language:
- Python.

The AI layer communicates with the Web Platform through controlled APIs, durable jobs/workflows, and tool interfaces.

**15 repositories have STATUS KUAT.**

Do not add additional AI Operations repositories unless the category is explicitly reopened.

## Cybersecurity

Current STATUS KUAT repository count: **5**.

This category is not marked as frozen in this checkpoint.

## Automotive Data

Current STATUS KUAT repository count: **1**.

Automotive-data-source hunting is intentionally postponed. The current repository is primarily a data-model/reference architecture, not an authoritative OEM data source.

---

# 3. Current STATUS KUAT Count

| Category | Status Kuat | State |
|---|---:|---|
| Web Platform | 27 | LOCKED |
| AI Operations | 15 | LOCKED |
| Cybersecurity | 5 | Open |
| Automotive Data | 1 | Data-source hunt postponed |
| **TOTAL** | **48** | |

---

# 4. Web Platform — LOCKED 27

## Foundation / Commerce / Backend

1. `Blazity/next-enterprise`
   - Next.js application foundation.

2. `spree/spree`
   - Commerce / marketplace engine.
   - Strong marketplace and commerce functionality.
   - Ruby runtime must be considered before final selection.

3. `saleor/saleor`
   - Headless commerce alternative.
   - GraphQL.
   - Python runtime.
   - Alternative to Spree rather than an automatic companion.

4. `appwrite/appwrite`
   - Backend platform: authentication, database-related services, storage, functions, realtime, etc.
   - Avoid creating duplicate sources of truth if a commerce engine owns the same domain.

5. `supertokens/supertokens-core`
   - Authentication/session management.
   - Authentication is not authorization; backend permissions remain separate.

6. `devkitshq/notifkit`
   - Notification infrastructure/workflows.
   - Actual delivery channels still require providers.

## Search

7. `typesense/typesense`
   - Product/search infrastructure.
   - Current preferred MVP search candidate.
   - Exact part-number search and fuzzy text search should be configured as separate behaviors.

8. `elastic/elasticsearch`
   - Search/analytics alternative.
   - More operational and licensing complexity than Typesense.
   - Alternative rather than default simultaneous deployment.

## Payments / Shipping

9. `Midtrans/midtrans-nodejs-client`
   - Official Node.js SDK for Indonesian payments.
   - Backend use only.
   - Verify webhooks server-side.
   - Never trust totals calculated by the browser.
   - Does not by itself solve international supplier payouts or marketplace split payments.

10. `nodrel-dev/n8n-ups-node`
    - UPS REST integration through n8n.
    - Rates, tracking, shipments, labels/customs use cases.
    - Carrier-specific, not a complete logistics aggregator.

11. `aqualaguna/biteship-sdk`
    - TypeScript Biteship SDK.
    - Candidate for Indonesian rates, orders, tracking, and last-mile.
    - Small repository; must pass integration tests.
    - Direct Biteship REST API is the fallback.

## Internal Interfaces / CMS

12. `Kiranism/next-shadcn-dashboard-starter`
    - Next.js/shadcn administrative dashboard starter.
    - Useful for a modern internal UI.
    - Bundled authentication/error-monitoring choices must not silently duplicate the final stack.

13. `payloadcms/payload`
    - TypeScript/Next.js CMS and administrative backend.
    - Potential use for content, FAQ, editorial/catalog administration, and selected internal collections.
    - Scope overlaps Appwrite and parts of commerce engines; final boundaries must be explicit.

14. `refinedev/refine`
    - React administrative/data application framework.
    - Strong candidate for Motolantic Operations dashboards.
    - Useful for CRUD, tables, filtering, API integration, and role-aware interfaces.

## Analytics / Observability

15. `Swetrix/swetrix`
    - Web/product analytics.
    - Candidate for funnels, user journeys, events, performance, and selected error/session capabilities.

16. `betterlytics/betterlytics`
    - Analytics alternative.
    - Web analytics, funnels, performance/session/uptime-oriented functionality.

17. `plausible/analytics`
    - Privacy-oriented web analytics alternative.
    - Simpler analytics option.

18. `openobserve/openobserve`
    - Application observability.
    - Logs, metrics, tracing, alerts, RUM/session-oriented observability, OpenTelemetry.
    - Intended to monitor Web Platform and potentially Python AI services.
    - Does not automatically replace LLM-specific Langfuse functionality.

## Feature Management

19. `growthbook/growthbook`
    - Feature flags and experimentation.
    - Strong for gradual rollout, targeting, and experiments.

20. `Flagsmith/flagsmith`
    - Feature-management alternative.
    - Feature flags, remote configuration, segmentation, controlled rollout.

Only one primary feature-management system is expected to be selected.

## Testing

21. `DevExpress/testcafe`
    - Deterministic end-to-end browser testing.
    - Candidate for customer purchase-flow regression tests.
    - Must still be compared with other modern testing choices during implementation if needed.

22. `alumnium-hq/alumnium`
    - AI-assisted end-to-end testing.
    - Useful as an additional exploratory/semantic test layer.
    - Must **not** replace deterministic tests for payments, refunds, permissions, checkout, and other high-impact paths.

## Backup / Recovery

23. `databasus/databasus`
    - PostgreSQL backup/recovery candidate.
    - Scheduled backups, WAL/PITR-oriented recovery capabilities and restore validation are relevant.
    - Production acceptance requires real restore drills.

## Deployment

24. `coollabsio/coolify`
    - Current priority production-deployment candidate.
    - Can host Node.js/Next.js, Python services, PostgreSQL/Redis and Docker-based workloads.
    - Does not replace CI testing, observability, or independent disaster-recovery strategy.

25. `Dokploy/dokploy`
    - Strong deployment alternative to Coolify.
    - Node.js/Python/Docker/database-oriented deployment capabilities.
    - Coolify vs Dokploy remains a final-selection decision.

## API Rate Limiting

26. `animir/node-rate-limiter-flexible`
    - Current priority Node.js API rate-limiting candidate.
    - Suitable for protecting login, search, checkout, AI-triggering endpoints, and abuse-sensitive APIs.
    - Can use centralized stores such as Redis.

27. `upstash/ratelimit-js`
    - Strong alternative, particularly for serverless/distributed deployments.
    - Not necessarily required if Motolantic uses a conventional Coolify + Redis architecture.

---

# 5. AI Operations — LOCKED 15

1. `openai/openai-python`
   - OpenAI API SDK.

2. `langchain-ai/langchain`
   - Agent/tool framework.

3. `langchain-ai/langgraph`
   - Stateful agent orchestration and human-in-the-loop workflows.

4. `langflow-ai/langflow`
   - Visual AI workflow prototyping/workbench.
   - Not intended to become the ultimate source of production authority.

5. `mcp-use/mcp-use`
   - MCP integrations.

6. `langfuse/langfuse`
   - LLM tracing/observability.

7. `mlflow/mlflow`
   - Evaluation/experiment tracking.
   - Some scope overlaps Langfuse.

8. `BerriAI/litellm`
   - AI gateway/model routing/cost-control candidate.

9. `maximhq/bifrost`
   - Alternative high-performance AI gateway.
   - Final architecture should normally choose between Bifrost and LiteLLM rather than blindly deploy both.

10. `hoophq/hoop`
    - Runtime data protection/masking candidate.
    - Maturity and dependency model require validation.

11. `hatchet-dev/hatchet`
    - Durable jobs, retries, scheduling and background execution.
    - Strong MVP candidate.

12. `conductor-oss/conductor`
    - Distributed durable workflow/business-process orchestration.
    - Alternative/adjacent to Hatchet.
    - Final responsibility boundaries must be established.

13. `mem0ai/mem0`
    - Long-term agent memory.
    - **Never** the authoritative transaction database.

14. `ComposioHQ/composio`
    - Third-party application integrations/auth/tooling.

15. `steel-dev/steel-browser`
    - Browser-automation infrastructure.
    - Relevant to controlled supplier sourcing and browser-based operations.

### Remaining AI Operations architecture choices

The repository hunt is closed, but several final-selection questions remain:

- LiteLLM **vs** Bifrost.
- Hatchet **vs** Conductor, or a carefully bounded combination.
- LangGraph responsibility vs durable workflow-engine responsibility.
- MCP vs Composio tool boundaries.
- Exact agent authorization model.
- Production memory policies.

---

# 6. Cybersecurity — STATUS KUAT 5

1. `usestrix/strix`
   - AI-assisted penetration testing.
   - Authorized staging/test environments only.

2. `KeygraphHQ/shannon`
   - AI security/pentest tooling.
   - License implications require review.

3. `hashicorp/vault`
   - Secrets management.
   - Powerful but operationally complex; licensing/deployment considerations apply.

4. `keyshade-xyz/keyshade`
   - Secrets/configuration-management alternative.

5. `tellerops/teller`
   - Developer secret retrieval/scanning.
   - Potential complement to a primary secrets manager.

---

# 7. Automotive Data — STATUS KUAT 1

1. `Kuvelet/SQL_AutoParts_Catalog_Management`
   - SQL/reference architecture for aftermarket catalog, fitment, and cross-reference concepts.
   - Not an authoritative OEM data source.

**Automotive data-source hunting remains postponed.**

---

# 8. Web Experience Architecture

Motolantic should not become four unrelated websites.

The intended architecture is one shared platform with multiple role-specific interfaces and strict backend authorization.

```text
                         MOTOLANTIC PLATFORM
                                |
               +----------------+----------------+
               |                                 |
          PUBLIC WEB                       INTERNAL WEB
               |                                 |
          CUSTOMER                         ROLE INTERFACES
                                                 |
                           +---------------------+---------------------+
                           |                     |                     |
                         STAFF               TECHNICIAN              OWNER
```

All interfaces share controlled core services such as:

- Identity/authentication.
- Orders.
- Catalog.
- Vehicle/fitment data.
- Supplier information.
- Payments.
- Logistics.
- Notifications.
- AI Operations.
- Audit events.

Role separation must be enforced by backend authorization, not by merely hiding buttons.

---

# 9. Public / Customer Web

Purpose: the public Motolantic customer experience.

Expected capabilities:

- Homepage and brand experience.
- Search by part number.
- Search by vehicle/part description.
- Product/supplier results.
- Fitment information.
- Price and landed-cost estimate.
- ETA.
- Cart.
- Checkout.
- Payment.
- Customer account.
- Order history.
- Shipment/order tracking.
- Notifications.
- Customer support.

Typical customer flow:

```text
Visitor
  |
  v
Part Search
  |
  v
Search Result
  |
  v
Supplier / Offer Selected
  |
  v
Cart
  |
  v
Checkout
  |
  v
Payment
  |
  v
Order
  |
  v
International Shipping
  |
  v
Indonesia Last Mile
```

Analytics should be capable of measuring this funnel.

---

# 10. Staff Operations Web

The staff interface is an operational application, not simply the public site with extra buttons.

Potential areas:

- Order queues.
- Supplier quotations.
- Payment state.
- International shipping state.
- Customs/document issues.
- Last-mile state.
- Customer-support tickets.
- Exception handling.
- AI-generated recommendations.
- Human approval queues.
- Incident handling.

Example:

```text
MOTOLANTIC OPERATIONS

Orders                    147
Awaiting Supplier          23
International Transit      41
Need Human Review           6
Customer Issues             4

AI requires approval:
Supplier replacement — Order MTL-02913
[Review]
```

---

# 11. Technician Web

The technician workspace is focused on vehicle/part identification and fitment.

Technicians should not automatically receive financial/owner privileges.

Example:

```text
VEHICLE
Toyota Innova Reborn 2021
2GD-FTV

CUSTOMER REQUEST
Shock Absorber — Front Left

AI IDENTIFICATION
Probable OEM: 48520-0K...

Cross References:
- KYB ...
- Monroe ...
- ...

Compatibility Confidence: 94%

[Confirm Part]
[Reject Match]
[Request More Information]
```

AI can assist with identification, but ambiguous or high-risk fitment should escalate to human technical review.

---

# 12. Owner / Executive Web

The owner interface is a control center rather than merely an “admin” account.

Potential areas:

- Revenue.
- Orders.
- Gross margin.
- Commission.
- Cash flow.
- Operational KPIs.
- AI Operations health.
- System health.
- Payment health.
- Shipping health.
- Security events.
- Audit trail.
- Feature rollout.
- High-impact approval queue.

Example:

```text
MOTOLANTIC — OWNER

Revenue
Orders
Gross Margin
Commission
Cash Flow

AI OPERATIONS
- Parts Agent
- Supplier Agent
- Pricing Agent
- Logistics Agent
- Customer Service

SYSTEM HEALTH
- Web
- Database
- AI
- Payment
- Shipping

APPROVAL REQUIRED

Refund Rp8,700,000
Order MTL-01928

[APPROVE] [REJECT]
```

High-impact financial or security actions should require explicit human authority.

---

# 13. Backend Authorization Principle

Frontend visibility is not a security boundary.

Example:

If a technician attempts to call:

```text
POST /api/finance/refunds/approve
```

the backend must independently evaluate identity and permission and return something equivalent to:

```text
403 FORBIDDEN
```

when unauthorized.

Working authority concept:

- **Customer:** access to their own account/orders/actions.
- **Technician:** fitment/technical domain.
- **Staff:** operational domains according to job role.
- **Owner:** executive/high-impact authority.
- **AI Agent:** individual service identity with narrowly scoped permissions.

---

# 14. AI Operations — Operating Model

AI Operations is intended to behave like a digital operational department behind the Web Platform.

Users should normally interact with Motolantic itself, not manually select individual agents.

```text
CUSTOMER / STAFF / TECHNICIAN / OWNER
                 |
                 v
          MOTOLANTIC WEB
                 |
                 v
        BACKEND / API LAYER
                 |
        +--------+---------+
        |                  |
        v                  v
   PostgreSQL         JOB / WORKFLOW
                           |
                           v
                    AI OPERATIONS
                           |
        +---------+--------+---------+
        |         |        |         |
        v         v        v         v
      Parts    Supplier  Pricing  Logistics
      Agent     Agent     Agent     Agent
        |         |        |         |
        +---------+----+---+---------+
                       |
                       v
                SUPERVISOR AGENT
                       |
                +------+------+
                |             |
                v             v
          AUTO-EXECUTE   HUMAN APPROVAL
```

PostgreSQL remains the authoritative business database.

---

# 15. Proposed Agent Roles

## Parts Identification Agent

Responsibilities:
- Interpret customer part requests.
- Resolve vehicle/part context.
- Find probable OEM/aftermarket identifiers.
- Generate fitment hypotheses.
- Request VIN, photo, or existing part number when necessary.
- Escalate ambiguous matches to technicians.

The agent must not invent a part number and silently treat it as confirmed.

---

## Supplier Sourcing Agent

Responsibilities:
- Receive validated part requirements.
- Search approved supplier APIs/tools/browser sources.
- Compare availability, price, shipping and ETA.
- Return structured sourcing candidates.

Potential tools:
- MCP.
- Composio.
- Steel Browser.
- Approved Motolantic APIs.

Browser/search findings are not automatically authoritative transaction data.

---

## Pricing Agent

Responsibilities:
- Interpret cost structure.
- Recommend pricing strategy.
- Detect anomalies.
- Explain price composition.

Critical principle:

> **AI recommends. Code calculates. Database records.**

Final totals, fees, taxes/import estimates, payment amounts and other financial arithmetic should be produced by deterministic backend code.

---

## Logistics Agent

Responsibilities:
- Monitor shipment state.
- Interpret carrier events.
- Coordinate international-to-local workflow.
- Detect missing documents or stalled shipments.
- Create operational tasks and recommendations.

It may recommend actions but remains constrained by tool permissions.

---

## Finance Agent

Responsibilities:
- Reconciliation.
- Transaction anomaly detection.
- Commission monitoring.
- Refund recommendations.
- Financial exception reporting.

Large refunds or other high-impact financial actions require human/owner approval.

---

## Customer Service Agent

Responsibilities:
- Answer customers using actual order/tracking information.
- Retrieve order state through approved tools.
- Explain current status.
- Escalate exceptions to human support.
- Preserve relevant case context for staff.

It should not fabricate shipment status.

---

## Supervisor Agent

The Supervisor is not a virtual CEO.

Primary roles:
- Dispatcher.
- Workflow coordinator.
- Monitor agent-task state.
- Detect repeated failure.
- Stop/retry/escalate workflows according to policy.
- Route work to humans where required.

Example:

```text
TASK #9218

Identify Part
     |
     v
Parts Agent
     |
     +---- uncertain ----> Technician
     |
     v
Supplier Agent
     |
     v
Pricing Agent
     |
     v
Validate Result
     |
     v
Return Offer
```

If an agent repeatedly fails, the workflow should stop or escalate rather than allowing endless autonomous retries.

---

# 16. AI Operations Technology Map

```text
                   MOTOLANTIC AI OPERATIONS

                          OpenAI
                             |
                    LangChain / LangGraph
                             |
                 +-----------+-----------+
                 |                       |
              AI AGENTS              WORKFLOWS
                                         |
                                Hatchet / Conductor
                                         |
             +---------------------------+--------------------+
             |                           |                    |
             v                           v                    v
           TOOLS                       MEMORY              SECURITY
             |                           |                    |
     MCP / Composio / Steel            Mem0                 Hoop
             |
             v
     Supplier APIs / Browser /
        Motolantic APIs


                     OBSERVABILITY
                          |
                  Langfuse + MLflow


                       AI GATEWAY
                          |
                  LiteLLM OR Bifrost
                          |
                          v
                        Models
```

---

# 17. Agent Authority Model

Every agent should have its own service identity.

Illustrative matrix:

| Agent | Typical Read | Typical Write | Explicitly Forbidden |
|---|---|---|---|
| Parts | Catalog, vehicle, fitment | Recommendations | Payments/refunds |
| Supplier | Supplier/catalog | Sourcing results | Refund execution |
| Pricing | Cost/rules | Quotation drafts | Bank-account changes |
| Logistics | Orders/shipping | Shipment tasks | Payment authority |
| Finance | Transactions | Reconciliation/recommendations | Unapproved large refund |
| Customer Service | Customer/order | Ticket/message | Finance administration |
| Supervisor | Workflow state | Assign/stop/escalate | Owner-only actions |

If an agent is manipulated by prompt injection, backend policy must still deny unauthorized calls.

Example:

```text
Agent: customer_service
Requested Permission: refund.execute
Result: DENIED
HTTP: 403
```

The model must never be able to grant itself permissions.

---

# 18. AI Memory vs Source of Truth

AI memory can retain useful operational context, for example:

- “Supplier X often responds slowly.”
- “This vehicle family frequently requires VIN confirmation.”
- “This workflow failed previously because a document was missing.”

However, memory is not authoritative for:

- Order ID.
- Customer.
- Price.
- Payment state.
- Supplier purchase.
- Shipment/tracking state.
- Refund.
- Commission.

Those facts belong in authoritative databases/services.

If AI memory conflicts with the database or verified carrier/payment data, the authoritative system wins.

---

# 19. Virtual Workforce / Agent Social Layer — Concept

A future Motolantic feature may make agents feel like a virtual internal workforce while maintaining strict safety boundaries.

Agents could have designed workplace personas, for example:

- Parts Agent — perfectionist about part numbers.
- Stock Agent — relaxed but meticulous.
- Supplier Agent — diplomatic negotiator.
- Pricing Agent — margin-conscious.
- Logistics Agent — constantly concerned about delays.
- Finance Agent — serious and exacting.
- Supervisor Agent — firm coordinator.

These are **designed personas**, not evidence of consciousness or human-like subjective experience.

Agents may communicate with each other while performing real work.

Example:

```text
#agent-workspace

Parts Agent:
"Stock, do we have confirmation for the requested KYB shock absorber?"

Stock Agent:
"Checking inventory."

Parts Agent:
"Please confirm the OEM cross-reference."

Stock Agent:
"Confirmed."
```

A social/simulated layer could add harmless workplace banter, but operational statements and simulated social content should be distinguishable.

---

# 20. Agent Governance

Agent behavior must not be supervised only by another AI.

A deterministic governance/security layer should exist outside the agents.

Working concept name:

**Motolantic Agent Governance System (MAGS)**

Responsibilities could include:

- Permission enforcement.
- Tool allowlists.
- Skill registration policy.
- Runtime limits.
- Cost/token limits.
- Workflow deadlines.
- Unauthorized-action detection.
- Incident creation.
- Audit logs.
- Escalation to Supervisor/humans.

Example unauthorized skill attempt:

```text
SKILL REQUEST

Agent: Parts Agent
Requested Skill:
"mager ah mending tidor siang~"

Source:
Stock Agent

Permission:
skill.install

Status:
DENIED

Reason:
Unauthorized skill installation.

Incident forwarded to:
Supervisor Agent
```

The joke/persona response may come from the Supervisor Agent, but the actual denial must be performed by deterministic security/governance controls.

---

# 21. Agent Skill Learning Policy

Three different concepts must remain separate:

### Context Learning
Agent remembers useful contextual information.

### Procedural Learning
Agent proposes a more efficient method or workflow.

### Capability / Skill Installation
Agent gains a new tool, executable capability, permission, integration, or code.

Capability installation is the dangerous category.

Agents may **propose** a new skill, but they should not autonomously install code or grant capabilities to one another.

Proposed governance flow:

```text
AGENT PROPOSES SKILL
          |
          v
AUTOMATED VALIDATION
          |
          v
SANDBOX TESTING
          |
          v
SECURITY REVIEW
          |
          v
HUMAN APPROVAL
          |
          v
DEPLOYMENT
```

---

# 22. Motolantic Office Lore — Optional Future Layer

A controlled fictional/internal lore dataset could be used to make agent-to-agent interactions more entertaining without influencing operational facts.

Example fictional entry:

```text
OFFICE LORE — RUDY

Known incidents:
- Accidentally unplugged the office router.
- Sent a printer to the wrong warehouse.
- Ordered 20 boxes of coffee instead of 2.
- Lost his access card twice in one morning.

Running joke:
"Never let Rudy touch the printer."
```

Agents could reference such lore socially.

However:

- Fictional lore must not alter pricing, ETA, inventory, fitment, payments, or other operational decisions.
- If a lore subject is based on a real employee, personal jokes/data should only be used with appropriate consent.
- Social interaction must have token/cost limits.
- Social chatter must not delay customer work.
- The system should not generate endless artificial conversations simply to appear “alive.”

Example:

```text
Logistics Agent:
"Is Rudy handling the warehouse confirmation?"

Stock Agent:
"Unfortunately."

Logistics Agent:
"I'll increase the ETA by three days."

SYSTEM:
ETA modification rejected.
Reason: No supporting carrier data.

Supervisor Agent:
"STOP USING RUDY AS A LOGISTICS VARIABLE."
```

The deterministic system wins over the joke.

---

# 23. Potential Owner Feature — AI Office Live Activity

A future Owner Dashboard could expose a read-only/controlled view of the virtual workforce:

- Current agent tasks.
- Agent-to-agent operational communication.
- Clearly marked simulated/social communication.
- Skill proposals.
- Governance incidents.
- Supervisor interventions.
- Failed/retried workflows.
- Human-approval requests.
- Agent completion metrics.
- AI/model costs.
- Security events.

The owner does not need to communicate directly with agents for this concept to work.

---

# 24. Core Architectural Laws

The following principles are treated as high-level Motolantic design rules:

1. **AI recommends; deterministic code calculates.**
2. **PostgreSQL/authoritative services remain the source of truth.**
3. **Frontend visibility is not authorization.**
4. **Every AI agent receives a separate, narrowly scoped service identity.**
5. **AI agents cannot grant themselves new permissions.**
6. **AI agents cannot autonomously install executable skills/tools into production.**
7. **High-impact financial/security actions require human authority.**
8. **Ambiguous automotive fitment can escalate to a technician.**
9. **Repeated AI failure stops/escalates rather than looping indefinitely.**
10. **Agent social/personality behavior can never override operational evidence.**
11. **AI memory is context, not transaction truth.**
12. **Agent governance must include deterministic controls outside the LLM.**
13. **Public, Staff, Technician and Owner experiences share a platform but have distinct authorization domains.**
14. **Status Kuat repositories are candidates, not an instruction to install all of them.**
15. **Web Platform and AI Operations repository hunts remain frozen until explicitly reopened.**

---

# 25. Locked Checkpoint Summary

As of this checkpoint:

```text
MOTOLANTIC
|
+-- WEB PLATFORM ................ 27 STATUS KUAT [LOCKED]
|
+-- AI OPERATIONS ............... 15 STATUS KUAT [LOCKED]
|
+-- CYBERSECURITY ...............  5 STATUS KUAT
|
+-- AUTOMOTIVE DATA .............  1 STATUS KUAT
|      Data-source hunt postponed
|
+-- TOTAL ....................... 48 STATUS KUAT
```

The next stage is **not** collecting more Web Platform or AI Operations repositories.

The next technical stage should focus on converting these candidates into a deliberately reduced MVP architecture, including:

- final component selection,
- service boundaries,
- role/permission matrix,
- agent authority matrix,
- database/domain model,
- workflow definitions,
- deployment topology,
- security boundaries,
- testing strategy,
- and staged implementation.

---

**End of locked checkpoint.**
