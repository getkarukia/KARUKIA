# KARUKIA MCP

**Security, quality and pentesting for your codebase — powered by AI, delivered via MCP.**

```bash
npx karukia-mcp
```

21 tools. 11 specialized AI personas. 935+ checkpoints across 6 compliance frameworks.
Zero config, zero account, runs 100% locally on your machine.

---

## How it works

KARUKIA is an MCP server that transforms your AI assistant into specialized experts on demand. When you ask for a security audit, your AI doesn't just wing it — it receives a complete methodology (persona, workflow, 445 security controls inline) and follows it systematically.

```
You: "karukia: audit the security of my project"

  AI receives Neo persona + 445 controls + OWASP/ISO/SOC2 checklists
  AI becomes Neo — systematic, evidence-based, file:line references
  AI delivers structured findings with severity and remediation steps
```

### Three audit layers

| Layer | Persona | Checkpoints | Question it answers |
|-------|---------|-------------|---------------------|
| Defensive | **Neo** | 445 controls | Is my code secure? |
| Quality | **Opo** | 245 rules | Is my app well-built? |
| Offensive | **Viper** | 245 tests | How would a hacker break in? |

---

## Quick Start

**Prerequisites:** [Node.js](https://nodejs.org/) 20+

### 1. Add to your project

Create `.mcp.json` at the root of your project:

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

That's it. On first launch, `npx` downloads the package (~175 KB) and caches it.

### 2. Configure (once)

Tell your AI:

> "karukia install"

KARUKIA scans your project, detects your stack, and generates configuration files.

### 3. Use it

Just talk naturally:

> "karukia: add user authentication"
> "karukia: audit security"
> "karukia: run a pentest"
> "karukia neo" — direct security audit
> "karukia viper" — direct pentest

The orchestrator analyzes your request and chains the right specialists automatically: code (Jeffrey) → security (Neo) → quality (Opo).

### Where to put `.mcp.json`

| AI Client | Config file | Scope |
|-----------|-------------|-------|
| Claude Code | `.mcp.json` at project root | Per project |
| Claude Code | `~/.claude.json` | All projects |
| Claude Desktop | `claude_desktop_config.json` | Global |
| Cursor | `.cursor/mcp.json` | Per project |
| Windsurf | MCP settings panel | Global |

### Global install (optional)

```bash
npm install -g karukia-mcp
```

Then use `"command": "karukia-mcp"` (without `npx`) in your MCP config.

---

## 11 AI Personas

Each tool returns a complete prompt that transforms your AI into a specialist for the session.

| Persona | Tool | What it does |
|---------|------|-------------|
| **Neo** — Security Auditor | `neo` | Defensive audit against OWASP, HDS 2.0, ISO 27001, SOC 2, PCI-DSS, HIPAA |
| **Viper** — Ethical Hacker | `viper` | Offensive pentest with 16 agents, CVSS v4, MITRE ATT&CK mapping |
| **Jeffrey** — Full-Stack Architect | `jeffrey` | Build features with TDD, then auto-chains to Neo for validation |
| **Opo** — Quality Guardian | `opo` | Targeted Opquast validation on modified files |
| **Auto** — Orchestrator | `auto` | Routes your request to the right skill chain |
| Quality Auditor | `audit_opquast` | Full 245-rule Opquast audit with scoring |
| Risk Analyst | `ebios_rm_audit` | EBIOS RM risk analysis (ANSSI methodology) |
| Hardening Planner | `security_hardening` | Structured remediation chantiers |
| IaC Specialist | `terraform_update` | Terraform KMS/GCS/IAM with security validation |
| Doc Auditor | `doc_refactor` | Documentation accuracy audit vs actual code |
| Configurator | `install` | One-shot project setup and detection |

### Utilities

| Tool | What it does |
|------|-------------|
| `list_checklists` | Browse all 24 checklists |
| `get_checklist` | Read a full checklist |
| `search_rules` | Search 935+ checkpoints by keyword |
| `suggest_checklists` | Get a prioritized audit plan for your stack |
| `generate_report` | Structured Markdown report with scoring |

---

## 935+ Checkpoints

### Defensive (Neo) — 6 checklists, 445 controls

| Framework | Controls | Scope |
|-----------|----------|-------|
| OWASP Security Baseline | 62 | Every web app |
| HDS 2.0 | 52 | Health data (France) |
| ISO 27001:2022 | 93 | Enterprise ISMS |
| SOC 2 Type II | 74 | SaaS (US market) |
| PCI-DSS v4.0 | 97 | Payment processing |
| HIPAA | 67 | Health data (US) |

### Quality (Opquast) — 14 checklists, 245 rules

Content, personal data, e-commerce, forms, identity, images, i18n, links, navigation, newsletter, presentation, security UX, performance, code structure.

Based on [Opquast](https://www.opquast.com/) — the web quality reference used by 15,000+ professionals.

### Offensive (Viper) — 4 checklists, 245+ tests

| Checklist | Tests | Scope |
|-----------|-------|-------|
| OWASP WSTG v5 | 100 | Web penetration testing |
| Cloud Platform | 80+ | Firebase, GCP, AWS, Azure |
| Healthcare | 50+ | PHI, encryption, medical data |
| Attack Scenarios | 15+ | PTES templates, MITRE ATT&CK |

---

## Usage Examples

**Security audit:**
> "karukia neo" — your AI becomes Neo, audits your codebase point by point

**Build with guardrails:**
> "karukia jeffrey: implement user authentication" — builds with TDD, then auto-validates with Neo

**Pentest:**
> "karukia viper" — 16 specialized agents run recon, surface analysis, and exploitation

**Orchestrate everything:**
> "karukia: add a logout button and audit security" — auto-chains Jeffrey → Neo → Opo

---

## Documentation

- [Livre Blanc (Français)](./LIVRE-BLANC.md) — Document technique détaillé : architecture, méthodologie, cas d'usage
- [Whitepaper (English)](./WHITEPAPER.md) — Technical deep-dive: architecture, methodology, use cases

---

## Cloud / Enterprise

KARUKIA runs locally by default (stdio transport via `npx`). A remote HTTP transport is also available for enterprise deployments — hosted on your own infrastructure, accessible via API with bearer authentication.

Interested in a managed KARUKIA instance for your team? Contact: **contact@karukia.com**

---

## About

KARUKIA is built around three principles:

1. **Separation of concerns** — Security, quality, and implementation are handled by separate AI personas with distinct expertise.
2. **Formal checkpoints over gut feeling** — 935 documented controls beat "I think it's fine."
3. **Defense in depth** — Defensive audit first, quality validation second, offensive testing last.

Built from real-world experience securing a healthcare SaaS to HDS 2.0 / ISO 27001 standards.

---

## License

Free for personal, educational, and internal professional use. No account required.

Commercial use or resale requires written authorization. Contact: **contact@karukia.com**

See [LICENSE](./LICENSE) for full terms.
