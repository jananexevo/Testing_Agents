# QA Agentic Automation System — "Autonomous Testing Team"

A set of specialized AI agents that automate UI/UX, API, security, visual, accessibility, CI/CD, performance, cross-browser, and release-quality testing — orchestrated by a parent orchestrator with an adversarial QA Gatekeeper (HR Manager) in the loop.

## Agent Roster

| Agent File | Role | Tooling |
|---|---|---|
| `qa-ui-ux-agent.md` | Senior QA Engineer — UI/UX E2E automation | Playwright |
| `qa-api-agent.md` | Senior QA Engineer — API automation | Postman + Newman |
| `qa-security-agent.md` | Security QA Engineer — OWASP Top 10 automation | OWASP ZAP, API/UI negative security cases |
| `qa-visual-a11y-agent.md` | Visual (Applitools) + Accessibility (Axe) | Applitools, Axe (axe-core) |
| `qa-cicd-agent.md` | QA Release Engineer — Jenkins + Allure | Jenkins, Allure |
| `qa-performance-agent.md` | Performance QA Engineer — K6 | K6 |
| `qa-crossbrowser-agent.md` | Cross-browser / scalability QA | BrowserStack |
| `testing-orchestrator-agent.md` | **Parent orchestrator** (10 yrs test lead) | Dispatches to all above |
| `qa-gatekeeper-agent.md` | **QA Gatekeeper / HR Manager** — adversarial reviewer | Challenges orchestrator |

## How the System Works

1. **Intake**: You give the Testing Orchestrator a website URL + context (product used by ~300 people, mid-sized company, criticality, release scope).
2. **Discovery**: Orchestrator inventories testable surface (pages, routes, APIs).
3. **Prioritize**: Risk-based scope — smoke + critical paths + high-risk areas. Not an exhaustive industry suite for a ~300-user product.
4. **Dispatch**: Orchestrator sends briefs to specialist agents (Playwright, Postman, OWASP, Applitools/Axe, BrowserStack, K6, Jenkins/Allure as needed).
5. **Consolidate**: Orchestrator merges findings into a release readiness report (executive summary, scope, risks, go/no-go).
6. **Adversarial Review**: QA Gatekeeper audits orchestrator work, finds mistakes/holes, proposes creative tests, and debates the Orchestrator via the 3-round protocol.
7. **Verdict**: Approve / Conditionally Approve / Blocked.

## Recommended Tool Stack (Black-Box / URL-Based)

Since you typically have **only a website URL** (no source code), all tools operate black-box:

| Testing Area | Primary Tool | Integrated Into QA Flow | Standalone / Supplementary |
|---|---|---|---|
| UI/UX E2E | Playwright | Yes — via CI (Jenkins agent) | BrowserStack for cross-browser |
| Visual Regression | Applitools | Yes — via CI | Screenshots baseline |
| Accessibility | Axe (axe-core) | Yes — via CI | — |
| API | Postman + Newman | Yes — via Jenkins agent | — |
| Security | OWASP ZAP (+ API/UI negative cases) | Yes — via CI agent | Manual pen-test handoff |
| Performance | K6 | Yes — via CI agent | Allure / Grafana reporting |
| Cross-browser | BrowserStack | Yes — via CI agent | Real Device Cloud |
| Reporting | Allure | Yes — via CI agent | — |

**Recommended minimal stack for a small (~300-user) product:**
Playwright (smoke + critical flows) + Postman/Newman (API) + Axe (a11y smoke) + Applitools or screenshot baselines (visual) + K6 (smoke/load) + BrowserStack (key browsers/devices) + Jenkins + Allure. Security: ZAP baseline + targeted OWASP negative tests.

## Using the Agents

Each agent file is a ready-to-paste prompt. In any AI chat, paste the desired agent file plus context (URL, environment, release scope).

Example (Orchestrator):
```
You are the Testing Orchestrator Agent... [paste testing-orchestrator-agent.md content]
Product context: internal tool used by ~300 people. Only URL available, no source code. ...
```

## OpenCode / Agent Platform Registration — ✅ DONE

All QA agents are now **registered as OpenCode subagents** (global scope) so they are available in any chat and any day:

```
C:\Users\surya\.config\opencode\agents\
├── qa-ui-ux.md
├── qa-api.md
├── qa-security.md
├── qa-visual-a11y.md
├── qa-cicd.md
├── qa-performance.md
├── qa-crossbrowser.md
├── qa-orchestrator.md
└── qa-gatekeeper.md
```

Each file uses OpenCode markdown agent frontmatter:

```yaml
---
description: <one-line agent purpose>
mode: subagent
---
```

- File name becomes the agent name (e.g., `qa-orchestrator.md` → `@qa-orchestrator`).
- `mode: subagent` makes them dispatchable by the primary agent or invocable via `@mention`.
- Restart OpenCode / reload session for changes to take effect.
- To list loaded agents: `opencode agent list`

### For Cursor / other platforms
- **Cursor**: copy each file to `.cursor/rules/` as `.mdc` with `description`, `globs: []`, `alwaysApply: true`.
- **Claude/Copilot**: save each as a custom agent / system prompt preset.

## Notes
- The orchestrator intentionally applies **risk-based, critical-path testing** for small audiences instead of exhaustive suites.
- The Gatekeeper/HR Manager ensures the Orchestrator "goes through and learns/corrects" mistakes via the adversarial loop.
- All agents follow black-box principles (URL-based) and safe-environment guardrails.
