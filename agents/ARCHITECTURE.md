# QA Agent System — Architecture, Workflows & Use Cases

> Detailed reference for the merged QA agent fleet (`wdg-qa-compact` unified skill → orchestrator / gatekeeper / specialists).

## 1. Overview

The QA system is an autonomous, **read-only** testing agent team:

- **QA Orchestrator** — parent agent; runs the unified QA lifecycle and dispatches specialists.
- **QA Gatekeeper / HR Manager** — adversarial quality gate; audits and challenges the orchestrator.
- **Specialist agents** — UI/UX (Playwright), API (Postman/Newman), Security (OWASP), Performance (K6), Visual/A11y, Cross-browser (BrowserStack), CI/CD (Jenkins/Allure).

All of them were merged with the condensed checklists from the unified skill **`wdg-qa-compact`** (source: `wdg-qa-agent-2.2.0.zip`).

## 2. Architecture

```
wdg-qa-compact skill (unified lifecycle + condensed specialist checklists)
        │
        ▼
QA Orchestrator ──unified workflow──▶ dispatches ──▶ QA UI/UX · QA API · QA Security · QA Performance
        │                                        │              · QA Visual/A11y · QA Cross-browser · QA CI/CD
        ▼
QA Gatekeeper ──audits against──▶ unified flow + evidence + reporting
        │
        ▼
Release Readiness Report (go / no-go, severity, coverage gaps)
```

Supporting layers: context packs (`context/00–09`), config (`qa-policy`, `severity`, `report-standard`, `suites.json`), probes, scripts, templates, workflows.

## 3. Workflows (from `.qa-agent/workflows/`)

| Phase | Workflow | Purpose | Output |
|---|---|---|---|
| 00 | Environment & tools gate | Run + env gate, baseline, tool gate | `00-environment/` |
| 01 | Discover | Context/contract verification, API scan, flows | `01-discovery/` |
| 02 | Plan | Scenarios S1–S6, test cases, probes, P0–P3 | `02-test-plan/` |
| 03 | Data | Synthetic QA data via approved seeders | `03-test-data/` |
| 04 | Execute | Suites in order → init/results/triage/finish/publish | `04-execution/` |
| 05 | Investigate | Bug reporting from FAILs | `05-investigation/` |
| 06 | Report | Final report + dashboard + chat summary | `06-final-report/` |
| 07 | Regression | Retest on new build vs base run | `07-regression/` |

Suite order (always last = adversarial): `static → data-integrity → api → security → business-rules → payments → e2e → cross-browser → notifications → accessibility → performance → load → adversarial`.

## 4. Agent Roster & Models

| Agent | Domain | Model |
|---|---|---|
| QA Orchestrator | Lifecycle orchestration | `opencode/flint-40b-free#high` |
| QA Gatekeeper | Adversarial audit/verdict | `opencode/nemotron-3.5-super-vl-free#high` | 
| QA UI/UX | Playwright | `opencode/beam-free#high` |
| QA API | Postman/Newman | `opencode/deepseek-v4.1-flash-free#high` |
| QA Security | OWASP | `opencode/glm-5.3-flash-free#high` |
| QA Performance | K6 | `opencode/beam-free#high` |
| QA Visual/A11y | Applitools/Axe | `opencode/mimo-v2.6-flash-free` |
| QA Cross-browser | BrowserStack | `opencode/glm-5.3-flash-free#high` |
| QA CI/CD | Jenkins/Allure | `opencode/big-pickle` |

## 5. Use Cases

1. **Black-box website QA** (current): orchestrator + browser tooling against a public URL (e.g., `ifyoustartownparty.com`); analyse → plan → (later) execute; no testing started per user instruction.
2. **Full WDG app QA run**: gate → discover → plan → data → execute suites → investigate → report.
3. **Regression**: new build vs base run.
4. **Suite-level runs**: `/wdg-qa:suite <id|all>`, gate, phase, publish, regression commands.

## 6. Operating Rules & Safety

- Read-only during QA except `publish-results.mjs`.
- Stop conditions: BLOCKED gate; prod/real-people risk; real payment; destructive ops; credential exposure; shared-data corruption.
- Defect classification: application defect · test issue · environment issue · external dependency.

## 7. Outputs

- Final report + `QA-DASHBOARD.md`; chat summary; STOP.
- Bug reports: `<area>: <wrong behaviour> (<role>, <service type>)`, expected cites contract.

---

**Next steps:** provide env credentials for the target product, then run via `/wdg-qa:gate` / `/wdg-qa:run` or `@qa-orchestrator`.
