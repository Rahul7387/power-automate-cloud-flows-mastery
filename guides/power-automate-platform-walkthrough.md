# Guide — Power Automate Platform Walkthrough
### A tour of every major surface in the product, as of July 2026

This is the guide to read before Project 1 if you've never opened Power Automate. It walks the actual navigation and screens you'll spend time in, so the projects in this repo make sense in context rather than assuming prior familiarity.

---

## 1. Signing in and orientation

Go to **make.powerautomate.com**. You land on **Home**, which shows: your recently used flows, a Copilot prompt box ("Describe the flow you want to build"), suggested templates based on your role/connected apps, and a summary of your current environment (shown top-right — this matters, see section 6).

**The environment picker (top-right corner) is the single most important piece of UI you'll interact with.** Everything in Power Automate — flows, connections, tables, solutions — is scoped to an environment. Building something in the wrong environment (e.g., your personal Dev environment instead of a shared Test environment) is the single most common beginner mistake, and it's invisible unless you check this picker.

## 2. My flows

**My flows** lists every cloud flow you own or that's shared with you, in three tabs: **Cloud flows**, **Desktop flows**, **Business process flows**. For each cloud flow you can see: status (on/off), last modified, and a **3-dot menu** with Run, Edit, Details, Turn off, Export, Share.

Click into any flow's **Details** page to see:
- **Run history** — every past execution, with a green/red/gray status indicator, duration, and a "Resubmit" option for failed runs (this is the self-service restore capability referenced in the 2026 Release Wave 1 notes).
- **Analytics** — a chart of runs over time, useful for spotting a sudden spike in failures before someone complains.
- **Connections** used by the flow, and whether each is healthy.

## 3. Creating a flow — four starting points

1. **Blank canvas** ("Build your own from blank") — pick a trigger manually, add actions one at a time. This is how Projects 1-5 in this repo should be built the first time, so you understand every piece.
2. **Template gallery** — thousands of pre-built flow templates ("When a new email arrives, save the attachment to OneDrive"). Good for learning common patterns fast, but resist just cloning templates once you're past Project 2 — you need to be able to build from blank.
3. **Connector-first** — start by picking a connector/trigger you know you need (e.g., "SharePoint — When a file is created") and let Power Automate suggest a starting structure.
4. **Copilot** — describe what you want in plain English in the prompt box. Covered in depth in Project 6 and worth trying from your very first flow onward, once you understand what it's actually generating (don't skip straight to Copilot before you've built a few flows by hand — you need to be able to tell when it's wrong).

## 4. The flow designer

Once inside a flow, the designer shows a **vertical canvas**: trigger at top, actions flowing downward, with branches (Condition, Switch, Scope) rendering as visually indented/boxed sections. Key UI elements:

- **The lightning bolt (⚡) icon** always marks the trigger — every flow has exactly one.
- **"+ New step"** between any two actions lets you insert an action mid-flow, not just at the end.
- **The dynamic content picker** (appears when you click into any input field) lets you insert outputs from earlier steps — this is how data flows between actions without you manually copying values.
- **"Peek code"** (three-dot menu on any action) shows the underlying JSON definition — invaluable for understanding exactly what an action is doing, and for spotting the "non-OpenAPI connection" issue that blocks Copilot editing (see Project 6's limitations).
- **The Copilot pane** (right side, if enabled) lets you ask questions about the flow you're currently editing, in addition to generating new steps.

## 5. Connectors and connections

**Data > Connections** shows every connection you've created (a specific authenticated link to a specific account — e.g., "SharePoint - contoso.com" or "Outlook - jane@contoso.com"). A **connector** (like "SharePoint") is the general integration; a **connection** is your specific authenticated instance of using it. This distinction matters the moment you deal with ALM (Project 7) — connection references let a solution point at different actual connections per environment, while the connector itself stays the same.

**Data > Custom connectors** is where you register connectors wrapping your own or third-party REST APIs (relevant to Project 4 and the companion Copilot Studio repo's Project 5).

## 6. Environments, Solutions, and the admin surface

Click the **environment picker** to see every environment you have access to (Default, Dev, Test, Prod, or others your admin created). Each environment has its own flows, connections, and (if attached) Dataverse database.

**Solutions** (left nav) is where solution-aware flows live — the ALM pattern from Project 7. A flow built inside a Solution behaves differently from one built loose in an environment: it gets connection references, environment variables, and can be exported/imported as a package.

If you have admin rights, the **Power Platform admin center** (a separate site, admin.powerplatform.microsoft.com) is where environment creation, DLP policies, capacity monitoring, and the consolidated licensing/usage reporting from 2026 Release Wave 1 all live — distinct from the maker-facing make.powerautomate.com site.

## 7. Approvals

**Approvals** (left nav) is a dedicated hub, separate from "My flows," showing every approval request you've sent and every one waiting on your response — across all flows, not just one. This is where Project 2's approval requests actually surface for the approver.

## 8. Process advisor / Process mining

**Process mining** (left nav, may appear as "Process advisor" depending on your tenant's rollout) is where Project 8's work happens: data ingestion, the Process Intelligence studio workspace, object-centric process models, and custom KPI dashboards.

## 9. Monitor and Copilot in automation center

**Monitor** gives a cross-flow view of recent runs, useful when triaging "did anything fail overnight" without opening each flow individually. **Copilot in automation center** lets you ask this same question in natural language ("show me failed runs from last night across all my flows") instead of manually filtering the Monitor view.

## 10. The mobile app

The Power Automate mobile app (iOS/Android) surfaces **Approvals** (respond to approval requests on the go), **flow run notifications**, and the ability to manually trigger button-based instant flows (like Project 2's expense submission) from your phone. Worth setting up early — it's how most real approvers actually interact with the approvals you build.

---

## What to do next
Once this tour makes sense, go build **[Project 1](../projects/01-beginner-flow-fundamentals/README.md)** from a blank canvas — you'll recognize every screen this guide just described.
