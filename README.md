# Power Automate Cloud Flows — Comprehensive Progressive Training Portfolio (2026)

A component-by-component Power Automate **cloud flow** training portfolio: nine hands-on projects spanning **Beginner → Intermediate → Advanced → Expert**, plus a **capstone** that unifies every capability into one governed, continuously-improved automation platform. Each project isolates one core building block — triggers & conditions, approvals & Adaptive Cards, data operations & control flow, premium connectors & integration, error handling & reliability, reusable child flows & AI Builder/Copilot authoring, enterprise ALM & governance, and process mining — so you learn (and can demonstrate) each piece deliberately.

> **Kept current as of July 4, 2026.** This portfolio reflects Power Automate's **2026 Release Wave 1** (April-September 2026): improved cloud flow management, capacity notifications, Process license sharing through flow groups, self-service restore capabilities, object-centric process mining with a modern Process Intelligence studio and native Microsoft Fabric integration, consolidated licensing/usage reporting, and expanded inventory details in the Power Platform admin center. It also reflects the upcoming **AI Builder credit model change** (seeded credits end November 1, 2026, shifting billing to Copilot Credits).

---

## 📂 Projects by difficulty

| Tier | # | Project | Core capability | Preview |
|---|---|---|---|---|
| 🟢 Beginner | 1 | [Flow Fundamentals](./projects/01-beginner-flow-fundamentals/README.md) | Triggers, actions, conditions | [🔗 Preview](https://rahul7387.github.io/power-automate-cloud-flows-mastery/projects/01-beginner-flow-fundamentals/index.html) |
| 🟢 Beginner | 2 | [Approvals & Adaptive Cards](./projects/02-beginner-approvals-adaptive-cards/README.md) | Human-in-the-loop, instant flows | [🔗 Preview](https://rahul7387.github.io/power-automate-cloud-flows-mastery/projects/02-beginner-approvals-adaptive-cards/index.html) |
| 🟡 Intermediate | 3 | [Data Ops & Control Flow](./projects/03-intermediate-data-ops-control-flow/README.md) | Variables, loops, Switch, Compose | [🔗 Preview](https://rahul7387.github.io/power-automate-cloud-flows-mastery/projects/03-intermediate-data-ops-control-flow/index.html) |
| 🟡 Intermediate | 4 | [Connectors & Integration Hub](./projects/04-intermediate-connectors-integration/README.md) | Dataverse, SQL, HTTP, custom connectors | [🔗 Preview](https://rahul7387.github.io/power-automate-cloud-flows-mastery/projects/04-intermediate-connectors-integration/index.html) |
| 🟠 Advanced | 5 | [Error Handling & Reliability](./projects/05-advanced-error-handling-reliability/README.md) | Scopes, retries, exception logging | [🔗 Preview](https://rahul7387.github.io/power-automate-cloud-flows-mastery/projects/05-advanced-error-handling-reliability/index.html) |
| 🟠 Advanced | 6 | [Child Flows, AI Builder & Copilot](./projects/06-advanced-child-flows-ai-builder-copilot/README.md) | Reusable components, AI, natural-language authoring | [🔗 Preview](https://rahul7387.github.io/power-automate-cloud-flows-mastery/projects/06-advanced-child-flows-ai-builder-copilot/index.html) |
| 🔴 Expert | 7 | [Enterprise ALM & Governance](./projects/07-expert-alm-governance/README.md) | Solutions, pipelines, DLP, licensing strategy | [🔗 Preview](https://rahul7387.github.io/power-automate-cloud-flows-mastery/projects/07-expert-alm-governance/index.html) |
| 🔴 Expert | 8 | [Process Mining](./projects/08-expert-process-mining/README.md) | Object-centric process intelligence | [🔗 Preview](https://rahul7387.github.io/power-automate-cloud-flows-mastery/projects/08-expert-process-mining/index.html) |
| ⭐ Capstone | 9 | [Procure-to-Pay Command Center](./projects/09-capstone-automation-command-center/README.md) | All eight capabilities, one governed platform | [🔗 Preview](https://rahul7387.github.io/power-automate-cloud-flows-mastery/projects/09-capstone-automation-command-center/index.html) |

> Live previews are static HTML/CSS mockups approximating the real Power Automate designer look & feel (flow canvas, approval cards, run history, admin dashboards). They are visual references only, not functional flows.

📋 **[Best Practices & Limitations Reference](./BEST-PRACTICES-AND-LIMITATIONS.md)** — a consolidated, standalone checklist pulling together every best practice and platform limitation called out across all nine projects, plus a licensing quick-reference table. Read this before building anything in production.

---

## 🧭 How to use this repo

- **New to Power Automate?** Build Projects 1 → 8 in order. Project 3's loop discipline assumes Project 1's naming/condition habits; Project 5's error handling assumes you understand what a "normal" flow looks like first; Project 7's governance assumes you have real flows worth governing.
- **Want one project to demo to leadership?** Go straight to **Project 9 (Capstone)** — it references every pattern below it and closes on the process-mining-driven ROI story that gets automation programs funded long-term, not just approved once.
- **Only care about one capability?** Every project is self-contained enough to build independently once you understand the prerequisites called out in its README.

---

## 🏗️ Common practices applied across every project

1. **Name everything meaningfully** — actions, scopes, child flows. The single cheapest, highest-leverage habit in this entire repo.
2. **Filter before you loop** — every data-heavy project reduces datasets before iterating, not inside the iteration.
3. **Compose intermediate values** for debuggability — a recurring pattern from Project 3 onward.
4. **Confirm-before-act on anything consequential** — approvals (Project 2), confirmation steps before external writes (Project 4), and human review gates on AI-driven decisions (Project 6) all reflect the same underlying discipline.
5. **Log durably, independent of platform run history** — Projects 4, 5, and 7 all write to Dataverse/SQL tables that outlive Power Automate's own retention window.
6. **Cost and licensing are design inputs, not afterthoughts** — from Project 4's premium-connector decision onward, every project's architecture includes at least one choice made partly because of its licensing implications.
7. **Governance scales with capability and risk** — the more automated and interconnected a flow becomes (Project 5's reliability engineering, Project 7's ALM, Project 8's process-wide analysis), the more explicit its safeguards and oversight must be.

---

## 🔧 Platform & tooling baseline (as of July 2026)

- **2026 Release Wave 1** cloud flow improvements: better flow management, capacity notifications, Process license sharing through **flow groups**, self-service restore capabilities
- **Object-centric process mining** and a modern **Process Intelligence studio** workspace with custom KPIs and native **Microsoft Fabric** integration
- **Copilot in cloud flows** (natural-language flow authoring and editing) and **Copilot in automation center** (natural-language querying of flow run history and product questions)
- **Consolidated licensing and usage reporting**, plus expanded Power Platform inventory details (flow dependencies, connections, environment relationships), in the Power Platform admin center
- **AI Builder credit transition**: seeded per-user AI Builder credits (5,000/user) end **November 1, 2026**; usage shifts to the Copilot Credit billing model from that date
- Continued **Power Automate Process license** model (license the flow itself, regardless of who triggers it) alongside per-user Free/standard/Premium licensing and pay-as-you-go
- **On-premises data gateway** as the standard bridge for on-prem systems (SQL Server, file shares, etc.), with gateway clustering as the recommended production-reliability pattern

---

## 📌 Suggested presentation order

1. Start with the **Capstone (Project 9)** for a manager/leadership audience — one coherent story across every difficulty tier, closing on a data-driven improvement roadmap rather than just a working demo.
2. Use **Projects 1-8** as deep-dive appendices when a specific stakeholder (security, finance, a particular business team) wants to go deeper on exactly one capability.
3. Keep the **Best Practices & Limitations Reference** open alongside any of these conversations — it's designed to answer "but what about X" questions on the spot.

---

## 📄 License

This repository is provided as a personal learning/portfolio and internal proof-of-concept reference. Adapt freely for your own organization's Power Automate training and POC needs. Licensing and platform-capability details cited throughout are informational summaries of Microsoft's published guidance as of mid-2026 — always verify current terms on Microsoft's official Power Automate documentation and licensing pages before making purchasing or architecture decisions.
