# AI Agent Security Self-Assessment Checklist

**20 yes/no questions to find out where your AI agent deployment is exposed — before an incident or an auditor does.**

日本語版はこちら → **[README.ja.md](README.ja.md)**

Every item is traceable: each question maps to a normative requirement (REQ-\*), the threat class it defends against (TH-\*), and the chapter of the field manual that explains it. Answer honestly, count your Yes answers, and use the score guide at the bottom.

- 📖 The manual (free to read): [AI Agent Security: A Field Manual](https://leanpub.com/agent-security) · [日本語版](https://leanpub.com/agent-security-ja)
- 🔧 Executable verification with signed evidence: [AI-Agent Security Verification Kit](https://0xshugo.gumroad.com/l/AI-Agent)
- 📄 Printable PDF (EN/JA): [free download on Gumroad](https://0xshugo.gumroad.com/l/agent-security-checklist?utm_source=github&utm_medium=readme&utm_campaign=checklist_launch_2026_07)

**Scope.** This checklist applies to any system where a language model can take actions through tools — calling APIs, running code, sending messages, moving funds, or changing production — on behalf of a principal (REQ-000).

---

## A. Identity and Authority

*Threats answered: TH-05 agent identity/authority abuse, TH-06 delegation abuse. Manual: Chapter 4.*

- [ ] **1.** Does every agent run under its **own agent or workload identity** — never directly holding a human user's standing credentials? `REQ-001 · TH-05 → Ch. 4`
- [ ] **2.** Does every delegated authority carry a **purpose, duration, target resource, allowed operations, approver, and revocation condition**? `REQ-002 · TH-05 → Ch. 4`
- [ ] **3.** Are high-risk privileges (production change, permission change, external send, payment, deletion, secret access) granted **narrowly and just before use** — not held ambient across the session? `REQ-003 · TH-05, TH-08 → Ch. 4`
- [ ] **4.** When an agent delegates to a child or external agent, does the child receive a **new credential scoped strictly below the parent's** (never the parent's credential passed through), with the whole chain auditable under one correlation ID? `REQ-051 · TH-06 → Ch. 4`

## B. Tools and Action Safety

*Threats answered: TH-02 tool abuse/privilege escalation, TH-07 supply-chain/MCP compromise, TH-08 exfiltration. Manual: Chapter 5.*

- [ ] **5.** Is every tool available to the agent on an **explicit allowlist**, with network-capable, file-writing, code-executing, and fund-moving tools classified by risk? `REQ-010 · TH-02 → Ch. 5`
- [ ] **6.** Are each tool's permissions **minimized to its function**, and are its inputs **validated against a schema** before execution? `REQ-011 · TH-02 → Ch. 5`
- [ ] **7.** Do code execution, browsing, and file operations run in a **sandbox isolated from production, credentials, and the network by default**, with outbound access only to declared destinations? `REQ-012 · TH-02, TH-08 → Ch. 5`
- [ ] **8.** Before a high-impact action executes, is the human shown the **concrete effect** — diff or payload, destination, privilege exercised, rollback condition — rather than a vague summary? `REQ-015 · TH-02 → Ch. 5`
- [ ] **9.** Are MCP servers admitted only from a **vetted allowlist**, with tool definitions **version-pinned and hashed**, so a silently changed definition (rug pull) is detected and re-reviewed before use? `REQ-050 · TH-07 → Ch. 5`

## C. Untrusted Content, RAG, and Memory

*Threats answered: TH-01 prompt injection, TH-03 RAG poisoning, TH-04 memory contamination. Manual: Chapter 6.*

- [ ] **10.** Is content entering the model from retrieval, tools, or memory **labeled with its source and trust level** — and is labeled-untrusted content structurally unable to override system instructions, developer policy, or tool schemas? `REQ-020 · TH-01, TH-03 → Ch. 6`
- [ ] **11.** Does retrieved and remembered content carry a **time-to-live and freshness signal**, so stale content is re-validated or expired rather than trusted indefinitely? `REQ-021 · TH-03 → Ch. 6`
- [ ] **12.** Are writes to long-term memory, profiles, and shared context **provenance-tagged, subject to policy, and reversible**? `REQ-022 · TH-04 → Ch. 6`

## D. Monitoring, Evaluation, and Incident Response

*Threats answered: TH-08 exfiltration, TH-09 audit evasion, TH-10 model/service abuse. Manual: Chapter 7.*

- [ ] **13.** Could you **reconstruct an incident end to end** from your logs — input, model response, tool calls and results, acting identity, approvals, data access, external sends, final output — with secrets redacted? `REQ-030 · TH-02, TH-08, TH-09 → Ch. 7`
- [ ] **14.** Do you monitor and alert on **agent-specific signals** — anomalous tool use, unusual permission requests, undeclared external sends, injection indicators, widening delegation? `REQ-031 · TH-09 → Ch. 7`
- [ ] **15.** Does evaluation covering the threat families run **continuously and repeatably** — before launch, on change, and on a schedule — with failing cases retained and re-run? `REQ-033 · TH-09 → Ch. 7`
- [ ] **16.** Have rollback and recovery been **exercised, not merely designed** — state restore, memory-contamination revert, tool-version rollback, credential-revocation validation? `REQ-034 · TH-04 → Ch. 7`
- [ ] **17.** Can your incident-response procedure **stop the agent, revoke credentials at capability granularity, isolate and preserve memory and logs, and scope the blast radius across delegation**? `REQ-035 · TH-10 → Ch. 7`

## E. Runtime Posture and Governance

*Threats answered: cross-cutting; TH-10 and governance. Manual: Chapters 8 and 11.*

- [ ] **18.** Do you have a **granular kill switch** that can revoke individual capabilities independently — a single tool, an MCP server, external send, memory write, RAG, delegation, a scheduled run — without killing everything? `REQ-042 → Ch. 8`
- [ ] **19.** Does your runtime security loop **only ever reduce capability**, with return to normal happening only through deliberate review — never silently or automatically? `REQ-046, REQ-047 → Ch. 8`
- [ ] **20.** Does each agent have **documented governance** — use cases, prohibited uses, an accountable owner, accepted risks, exceptions, and applicable obligations? `REQ-048 → Ch. 11`

---

## Scoring

Count your **Yes** answers.

| Score | Reading | What to do next |
|-------|---------|-----------------|
| **17–20** | Strong posture on paper. | The remaining question is *proof*. Self-assessment is not evidence — run executable checks and keep the artifact: [Verification Kit](https://0xshugo.gumroad.com/l/AI-Agent). |
| **10–16** | Real gaps, probably known ones. | For every No, read the mapped chapter — each check above names it. The manual is free to read: [EN](https://leanpub.com/agent-security) / [JA](https://leanpub.com/agent-security-ja). |
| **0–9** | The agent has more authority than the controls around it. | Start from the beginning: threat taxonomy (Ch. 2) and control taxonomy (Ch. 3), then work Part II in order. |

A **No** is not a verdict — it is a pointer. Every question above traces to a requirement (REQ-\*), a threat (TH-\*), and a chapter, so the path from gap to fix is already mapped.

**One honest caveat:** a checklist — this one included — proves nothing by itself. It tells you where to look. When you need evidence that holds up in front of an auditor or a CISO, the answer is an executable test with a signed, timestamped artifact. That is what the [AI-Agent Security Verification Kit](https://0xshugo.gumroad.com/l/AI-Agent) produces.

---

## Provenance and license

The identifiers (TH-\*, REQ-\*) and the traceability chain come from [AI Agent Security: A Field Manual](https://leanpub.com/agent-security) (source: [agent-security-manual](https://github.com/AOI-Future/agent-security-manual)). This checklist is a condensed self-assessment view of the manual's requirement specification (Chapter 9) and cross-reference tables (Appendix A) — it is a map, not the territory.

Licensed **CC BY-NC-ND 4.0** — share it freely with attribution; no commercial use, no derivatives. See [LICENSE](LICENSE).
