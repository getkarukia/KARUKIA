# KARUKIA MCP

```
██╗  ██╗ █████╗ ██████╗ ██╗   ██╗██╗  ██╗    ██╗ █████╗
██║ ██╔╝██╔══██╗██╔══██╗██║   ██║██║ ██╔╝    ██║██╔══██╗
█████╔╝ ███████║██████╔╝██║   ██║█████╔╝     ██║███████║
██╔═██╗ ██╔══██║██╔══██╗██║   ██║██╔═██╗     ██║██╔══██║
██║  ██╗██║  ██║██║  ██║╚██████╔╝██║  ██╗    ██║██║  ██║
╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝ ╚═╝  ╚═╝    ╚═╝╚═╝  ╚═╝
   AI methodology for highly regulated industries · Made in Guadeloupe 🇬🇵
```

**The complete AI-assisted development methodology, delivered via MCP.**

**Latest: v3.0.6**

26 tools, 19 skills, 1797+ checkpoints across 11 audit dimensions. Works with any AI platform (Claude Code, Cursor, Windsurf, Copilot...) through the Model Context Protocol.

---

> **Using KARUKIA for your team or company?** A commercial license is required.
> See [Commercial Licensing](#commercial-licensing) below or contact **contact@karukia.com**

---

## What is KARUKIA?

KARUKIA is a structured development methodology built around specialized AI personas. Each persona (Neo for security, Jeffrey for architecture, Viper for pentesting, Opo for quality...) comes with its own workflow, guard rails, and knowledge base.

When you call a KARUKIA tool, the MCP server returns a complete prompt — persona identity, workflow, checklists, templates — that transforms your AI assistant into that specialist for the session.

```
You: "Run a security audit"
  -> AI calls neo tool
  -> MCP returns full Neo persona prompt + 445 security controls inline
  -> AI becomes Neo, follows the methodology, produces structured findings
```

## The 11 Audit Dimensions

```
SECURITY  → Neo (445 pts)      "Is my code secure?"
QUALITY   → Certix (369 pts)   "Is my app well-built?"
OFFENSIVE → Viper (245+ tests) "How would a hacker break in?"
TS        → ts_quality (118)   "Is my TypeScript clean?"
CSS       → css_quality (55)   "Is my design system maintainable?"
ARCHI     → archi (70)         "Is my architecture sound?"
TESTS     → test_coverage (68) "Am I testing the right things?"
PERF      → perf (90)          "Where are the bottlenecks?"
DEBT      → debt (55)          "What's slowing us down?"
HDS/ISO   → audit_expert (200+)"Am I ready for certification?"
SCAN      → karukia_scan       "Run all 11 dimensions at once"
```

---

## Quick Start

**Prerequisites:** [Node.js](https://nodejs.org/) 22 or later.

### Step 1 — Add KARUKIA to your project

Create or edit `.mcp.json` at the root of your project:

```json
{
  "mcpServers": {
    "karukia": {
      "command": "npx",
      "args": ["karukia-mcp"]
    }
  }
}
```

> **Note:** If the file already exists and has other MCP servers, just add the `"karukia"` key inside the existing `"mcpServers"` object.

### Step 2 — Restart your AI client

Restart Claude Code (`/quit` then relaunch) or your IDE. The 26 KARUKIA tools are now available.

> On first launch, `npx` downloads the package automatically (~175 KB). Subsequent launches use the cached version.

### Step 3 — Configure your project

Tell your AI:

> "karukia install"

KARUKIA scans your project, detects your stack, and generates configuration files (security scope, CLAUDE.md, memory structure).

### Step 4 — Start working

Just tell your AI what you need in natural language, always through the `auto` orchestrator:

> "karukia auto: add user authentication"
> "karukia auto: audit security"
> "karukia auto: run a pentest"

`auto` is the recommended entry point for everything — it analyzes your request and routes to the right specialists automatically (Jeffrey → Neo → Opo, or Neo alone, or Viper, etc.).

> **Tip:** You only need two commands: `karukia install` (once) then `karukia auto` + your request (always). `auto` handles the rest. For advanced use, skills can also be called directly: "karukia neo", "karukia viper". Say "karukia start" anytime for a full guide.

### Where to put the config

| Client | File | Scope |
|--------|------|-------|
| **Claude Code CLI** | `.mcp.json` at project root | This project only |
| **Claude Code CLI** | `~/.claude.json` (home directory) | All your projects |
| **Claude Desktop** | `claude_desktop_config.json` | Global |
| **Cursor** | `.cursor/mcp.json` at project root | This project only |
| **Windsurf** | MCP settings panel | Global |

---

## Global Installation (optional)

If you want KARUKIA available in all your projects without adding `.mcp.json` each time:

```bash
npm install -g karukia-mcp
```

Then add to your global AI config (`~/.claude.json` for Claude Code):

```json
{
  "mcpServers": {
    "karukia": {
      "command": "karukia-mcp"
    }
  }
}
```

---

## 26 Tools

### Essential (start here)

| Tool | Description |
|------|-------------|
| `install` | **[FIRST STEP]** Configure KARUKIA for your project — run once |
| `auto` | **[MAIN TOOL]** Describe what you need — KARUKIA routes to the right skills |
| `start` | Quick-start guide — explains all skills at 3 progressive levels |

### Core Skills (AI Personas)

Each skill returns a complete prompt that transforms your AI into a specialist.

| Tool | Persona | What it does |
|------|---------|-------------|
| `neo` | Security Auditor | Defensive audit against 6 frameworks (OWASP, HDS, ISO 27001, SOC 2, PCI-DSS, HIPAA) |
| `viper` | Pentest Brigade | Offensive testing with 16 agents, CVSS v4 scoring, MITRE ATT&CK mapping |
| `jeffrey` | Full-Stack Architect | Feature implementation with TDD and security validation |
| `opo` | Quality Validator | Web quality against 369 Certix rules |
| `audit_certix` | Quality Auditor | Deep Certix compliance audit with 5 profiles |
| `ebios_rm_audit` | Risk Analyst | EBIOS Risk Manager methodology (ANSSI) — formal risk analysis |
| `security_hardening` | Hardening Planner | Security improvement chantiers |
| `doc_refactor` | Doc Auditor | Documentation accuracy audit vs actual code |

### Dimensional Skills (v3.0 New)

| Tool | Checkpoints | What it does |
|------|-------------|-------------|
| `ts_quality` | 118 | TypeScript audit — type safety, strict config, generics, async patterns |
| `css_quality` | 55 | CSS/Design System — maintainability, accessibility, metrics |
| `archi` | 70 | Architecture — module structure, coupling, layering |
| `test_coverage` | 68 | Test inventory — frontend/backend coverage quality |
| `perf` | 90 | Performance — frontend, backend, build/bundle |
| `debt` | 55 | Technical debt — dead code, dependency health, code smells |
| `karukia_scan` | 1797+ | **Global scan** — all 11 dimensions in parallel |
| `audit_expert_hds` | 200+ | Expert HDS 2.0/ISO 27001 — 8 domains, certification readiness |
| `change_report` | — | Change management report (ISO 27001 A.8.32) |

### Utilities

| Tool | Description |
|------|-------------|
| `list_checklists` | Browse all 22 checklists by category |
| `suggest_checklists` | Describe your project — get a prioritized audit plan |
| `generate_report` | Compile audit results into a scored Markdown report |

### Memory & Config

| Tool | Description |
|------|-------------|
| `init_memory` | Initialize KARUKIA memory structure in a project |
| `get_session_template` | Get pre-filled session templates for any skill |
| `get_config_template` | Get configuration templates (security scope, CLAUDE.md, analytics) |

---

## 22 Checklists

### Defensive Security (Neo) — 6 checklists, 445 controls

| Checklist | Points | Scope |
|-----------|--------|-------|
| **OWASP Security Baseline** | 62 | Every web app |
| **HDS 2.0** | 52 | Health data, France |
| **ISO 27001:2022** | 93 | Enterprise ISMS |
| **SOC 2 Type II** | 74 | SaaS, US market |
| **PCI-DSS v4.0** | 97 | Payment processing |
| **HIPAA** | 67 | Health data, US |

### Web Quality (Certix) — 5 profiles, 369 rules

369 rules across 5 profiles: DEV (development), UX (user experience), CONT (content), OPS (operations), JUR (legal/compliance).

Certix is KARUKIA Solutions' own web quality referential, built for modern web applications.

### Offensive Security (Viper) — 4 checklists, 245+ tests

| Checklist | Tests | Scope |
|-----------|-------|-------|
| **OWASP WSTG v5** | 100 | Web penetration testing |
| **Cloud Platform** | 80+ | Firebase, GCP, AWS, Azure |
| **Healthcare** | 50+ | PHI, encryption, medical data |
| **Attack Scenarios** | 15+ | PTES templates, MITRE ATT&CK |

### Dimensional Quality (New in v3.0) — 7 checklists, 656+ checkpoints

| Checklist | Points | Scope |
|-----------|--------|-------|
| **TypeScript Quality** | 118 | Type safety, strict config, patterns |
| **CSS / Design System** | 55 | Maintainability, a11y, metrics |
| **Architecture** | 70 | Module structure, coupling, layering |
| **Test Coverage** | 68 | Frontend/backend inventory, quality |
| **Performance** | 90 | Frontend, backend, build/bundle |
| **Technical Debt** | 55 | Dead code, deps, code smells |
| **Expert HDS/ISO 27001** | 200+ | Certification readiness — 8 domains |

---

## Usage Examples

### Full security audit

> "Run a security audit on my project"

Your AI calls `neo` — becomes the Neo security auditor — follows the methodology — produces structured findings with severity, file:line references, and remediation steps.

### Build a feature with guardrails

> "karukia jeffrey: implement user authentication"

Your AI calls `jeffrey` — becomes the Jeffrey architect — implements with TDD, then chains to Neo for security validation (rejection loop: if Neo rejects, Jeffrey fixes, max 3 iterations).

### Pentest your app

> "karukia viper"

Your AI calls `viper` — deploys the Brigade methodology with 16 specialized agents across Recon, Surface Analysis, and Exploitation phases.

### Orchestrate everything

> "karukia auto: add a logout button and audit security"

Your AI calls `auto` — analyzes the request — routes to the right skill(s) — manages the chain.

---

## Documentation

- [Livre Blanc (Français)](./LIVRE-BLANC.md) — Document technique détaillé : architecture, méthodologie, cas d'usage
- [Whitepaper (English)](./WHITEPAPER.md) — Technical deep-dive: architecture, methodology, use cases

---

## About

KARUKIA is developed by **[KARUK IA Solutions](https://karukia.com)**, a B2B SaaS studio specializing in regulated industries (healthcare, finance, pharma), based in Guadeloupe. 🇬🇵

KARUKIA was built while developing a healthcare SaaS application for HDS 2.0 and ISO 27001 certification. The methodology grew out of a real question: what does it actually take to get certified, from day one of development? The checklists reflect what a real auditor asks, point by point — not theory.

The project is built around three principles:
1. **Separation of concerns** — Security, quality, and implementation are separate disciplines handled by separate AI personas.
2. **Formal checkpoints over gut feeling** — 1797+ documented checkpoints beat "I think it's fine."
3. **Defense in depth** — Defensive audit first, quality validation second, offensive testing last.

> *Made in Guadeloupe — AI doesn't replace the expert, it frees them.*

---

## Commercial Licensing

KARUKIA MCP is licensed under the [Business Source License 1.1](./LICENSE) (BUSL-1.1).

### Free use (no license needed)

- Personal projects and individual developers
- Educational institutions, students, research
- Non-profit organizations

### Commercial license required

If your company or consulting firm uses KARUKIA for production work or deploys it across developer teams, a commercial license is required.

| Plan | Price (EUR HT/year) | Team size |
|------|---------------------|-----------|
| **Starter** | 5 000 | Up to 10 developers |
| **Business** | 12 000 | Up to 50 developers |
| **Enterprise** | 20 000 | Unlimited developers + priority support |

All plans include: full access to all 26 tools, 19 skills, 1797+ checkpoints across 11 audit dimensions, and all updates for the license duration. Annual license, renewable.

**Contact:** contact@karukia.com

> A single external security audit costs 10-15k EUR. KARUKIA gives your entire team the methodology to run audits continuously — for less than the price of one.

### Change License

On March 6, 2028, the Licensed Work will automatically convert to the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
