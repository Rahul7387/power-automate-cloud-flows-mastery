# Guide — Dynamics 365 Integration with Power Automate
### Triggers, automation options, and when to choose which

Dynamics 365 apps (Sales, Customer Service, Field Service, Finance & Operations) all run on **Dataverse** under the hood, so most of what you already know from Projects 4 and 7 applies directly — but Dynamics adds its own trigger types, competing automation options, and decision points this guide walks through before Project 10 puts them into practice.

---

## 1. Dataverse connector vs. the legacy "Dynamics 365" connector

There are, confusingly, **two connectors** that can trigger a flow off a Dynamics 365 record change:

- **Dataverse connector (current, recommended)** — triggers: "When a row is added, modified, or deleted," with full support for **filtering attributes** (only fire when specific columns change) and **filter expressions** (only fire when a condition on the row is met), plus separate **pre-operation (before) and post-operation (after)** trigger scopes.
- **Dynamics 365 connector (legacy)** — the older connector, largely superseded. New flows should default to the Dataverse connector; only use the legacy connector when integrating with an older, still-supported flow you haven't yet migrated.

**Always build new Dynamics 365 automation on the Dataverse connector.** The legacy connector persists for backward compatibility, not for new development.

## 2. Filtering attributes and filter expressions — don't skip this

A flow triggered by "When a row is modified" on a busy table (like `Opportunity` or `Case`) without filtering will fire on **every single field change**, including ones irrelevant to your logic — a huge, easily avoidable source of wasted runs and throttling risk.

- **Filtering attributes**: specify exactly which columns must change for the trigger to fire (e.g., only `statuscode` and `estimatedvalue`, not every field on the Opportunity form).
- **Filter expression**: an OData-style condition further narrowing which rows qualify (e.g., only fire when `statuscode eq 3` — Won).

Both should be treated as mandatory configuration on any Dataverse trigger against a high-traffic table, not an optional refinement.

## 3. Before (pre-operation) vs. after (post-operation) triggers

- **Post-operation (after)** triggers — the default and by far the most common — fire once the database change has already committed. Use this for almost everything: notifications, downstream system sync, approvals.
- **Pre-operation (before)** triggers fire before the change commits and can **modify or block** the operation itself (similar in spirit to a synchronous plugin). This is a more advanced, higher-stakes pattern — a poorly designed before-trigger flow can introduce latency into every save operation on that table, or block saves unexpectedly if the flow itself fails. Use before-triggers sparingly and only when you genuinely need to intercept/validate before commit, not as a default choice.

## 4. Automation options in the Dynamics 365 ecosystem — decision matrix

| Option | Best for | Trade-off |
|---|---|---|
| **Power Automate cloud flow (Dataverse trigger)** | Most business-process automation: notifications, approvals, cross-system sync, AI-driven actions | Slight latency (near-real-time, not synchronous); the default choice for new work |
| **Business Process Flow (BPF)** | Guiding a user through a structured, staged process directly in the model-driven app UI (e.g., Lead → Qualify → Opportunity stages) | Not a background automation mechanism — it's a UI/UX guide for the end user, not a substitute for a flow |
| **Classic Dataverse Workflow (background/real-time)** | Legacy systems still running them | Microsoft's guidance has shifted decisively toward Power Automate cloud flows for new automation; don't build new classic workflows |
| **Power Automate (Real-time via Dataverse plugin pipeline)** | Scenarios needing true synchronous, transactional behavior tied to the save operation | Higher complexity and stricter performance requirements than a standard post-operation cloud flow |
| **Custom plugin (.NET/C#, pro-code)** | Complex synchronous logic, high-performance transactional requirements, or logic needing capabilities outside what flows expose | Requires pro-dev skills, deployment, and support — reserve for genuine gaps that low-code can't cover |

**The practical rule for this repo's audience:** default to a Power Automate cloud flow with a post-operation Dataverse trigger. Reach for a BPF when you need to guide a user through stages in the UI (a complementary, not competing, tool). Only consider pre-operation triggers, real-time flows, or custom plugins when you have a specific, well-understood requirement a standard cloud flow can't meet.

## 5. Calling a flow directly from a Dynamics 365 form

Beyond record-change triggers, you can also let a user **manually invoke a flow from a button** on a model-driven app form or the command bar — the modern replacement for older custom ribbon-button JavaScript patterns. This uses the same "Manually trigger a flow" pattern from Project 2, but bound to a Dataverse row context instead of a generic button, so the flow receives the specific record's ID as input automatically.

This is the right pattern for actions like "Send to Contract," "Escalate to Manager," or "Generate Proposal Document" — user-initiated, record-specific actions that don't need to fire automatically on every save.

## 6. Business Events (loosely-coupled integration)

For scenarios spanning **Dataverse and Dynamics 365 Finance & Operations** (or other systems publishing to the same event framework), **Business Events** provide a pub/sub pattern: a system publishes a business event (e.g., "Sales order confirmed") without knowing or caring who's listening, and a Power Automate flow (or Azure Service Bus/Event Grid subscriber) picks it up. This decouples systems more cleanly than a direct point-to-point trigger, and is worth knowing about even if your first integration doesn't need it yet — it's the right answer once you have three or more systems that all need to react to the same event independently.

## 7. Licensing note specific to Dynamics 365

Many Dynamics 365 application licenses (Sales, Customer Service, Field Service) include **Power Automate use rights scoped to flows operating within that Dynamics 365 app's context** — this is narrower than a full standalone Power Automate Premium license. Before assuming a flow is "free" because your organization has Dynamics 365 licenses, confirm whether the specific scenario (especially anything using premium connectors outside the Dynamics/Dataverse ecosystem, like an external HTTP call or a non-Dataverse premium connector) falls within those included use rights or requires separate Power Automate licensing — this is a genuinely common licensing gap organizations discover only after a flow is already in production.

---

## What to do next
Apply this guide directly in **[Project 10 — Dynamics 365 Sales Automation](../projects/10-bonus-dynamics-365-automation/README.md)**, which builds a Dataverse-triggered flow with filtering attributes, a command-bar-invoked instant flow, and a BPF working alongside both.
