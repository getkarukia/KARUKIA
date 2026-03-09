# KARUKIA — Whitepaper

**AI-assisted development methodology: 11 audit dimensions via the MCP protocol.**

*Version 3.0 — Document in English. Version française : [LIVRE-BLANC.md](./LIVRE-BLANC.md)*

```
██╗  ██╗ █████╗ ██████╗ ██╗   ██╗██╗  ██╗    ██╗ █████╗
██║ ██╔╝██╔══██╗██╔══██╗██║   ██║██║ ██╔╝    ██║██╔══██╗
█████╔╝ ███████║██████╔╝██║   ██║█████╔╝     ██║███████║
██╔═██╗ ██╔══██║██╔══██╗██║   ██║██╔═██╗     ██║██╔══██║
██║  ██╗██║  ██║██║  ██║╚██████╔╝██║  ██╗    ██║██║  ██║
╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝ ╚═╝  ╚═╝    ╚═╝╚═╝  ╚═╝
   AI methodology for highly regulated industries · Made in Guadeloupe 🇬🇵
```

---

## 1. The Problem

AI assistants (Claude, GPT, Copilot) are powerful but generic. When asked for a security audit, the AI improvises: it knows OWASP concepts but doesn't follow a structured methodology. Results vary from session to session with no guaranteed exhaustiveness.

**Real-world consequences:**

- Vulnerabilities slip through because the AI "forgets" to check certain points
- No traceability: impossible to prove a control was performed
- No reproducibility: two audits of the same code yield different results
- Zero compliance: frameworks (ISO 27001, SOC 2, HDS) require formal evidence
- No coverage beyond security: TypeScript quality, architecture, performance, and technical debt are invisible

## 2. The KARUKIA Solution

KARUKIA solves this by injecting a **complete methodology** directly into the AI's context, on demand.

### How it works

KARUKIA is an MCP server (Model Context Protocol) — Anthropic's open standard for connecting tools to AI. When the user requests an audit, the MCP server returns a **monolithic prompt** containing:

1. **Persona identity** — name, expertise, communication style
2. **Workflow** — mandatory steps, in order, with validation gates
3. **Checklists** — all relevant control points, inline in the prompt
4. **Templates** — expected output format (tables, scores, verdicts)
5. **Guard rails** — non-negotiable rules (e.g., "every finding must cite file:line")

The AI receives this prompt and **becomes** the specialist for the duration of the session. It cannot "forget" a control — it's written in its context.

### The MCP Protocol

The [Model Context Protocol](https://modelcontextprotocol.io/) is an open standard that allows any AI client to connect to tool servers. KARUKIA works with:

- **Claude Code** (CLI)
- **Claude Desktop**
- **Cursor**
- **Windsurf**
- **Any MCP-compatible client**

Installation is a single line:

```bash
npx karukia-mcp
```

No account, no API key, no data sent externally. The server runs locally on the developer's machine.

---

## 3. Architecture

### Local transport (default)

```
Developer <-> AI Client <-> [stdio] <-> KARUKIA MCP Server (local)
                                            |
                                            ├── 20 skills (prompt builders)
                                            ├── 22 checklists (1797+ points)
                                            └── Memory system (sessions, trackers)
```

The server communicates via **stdio** (standard input/output). No network port opened, no HTTP calls. Everything stays on the local machine.

### Remote transport (enterprise)

```
Team <-> AI Clients <-> [HTTPS] <-> KARUKIA MCP Server (Cloud Run)
                                        |
                                        ├── Bearer auth (MCP_API_KEY)
                                        ├── Per-session rate limiting
                                        ├── Audit trail (Pino logs)
                                        └── Same engine, same checklists
```

For team deployments, KARUKIA supports a **streamable HTTP** transport. Bearer token authentication, structured logs, rate limiting.

### What the server returns

Each MCP tool returns a **monolithic prompt** — a single text block containing everything the AI needs. No chain of calls, no RAG, no vector database. Just a well-constructed prompt.

Simplified example for `neo` (security audit):

```
[GUARD — non-negotiable obligations]
+
[Neo persona — identity, style, expertise]
+
[Workflow — mandatory steps]
+
[OWASP checklist — 62 inline controls]
+
[ISO 27001 checklist — 93 inline controls]
+
[Output template — table format + verdict]
```

---

## 4. The 11 Audit Dimensions

### Security — Defensive (Neo)

Point-by-point audit against 6 compliance frameworks. Every finding cites file:line. Every verdict is COMPLIANT or NON-COMPLIANT with evidence.

| Framework | Controls | Scope |
|-----------|----------|-------|
| OWASP Security Baseline | 62 | Every web app |
| HDS 2.0 | 52 | Health data (France) |
| ISO 27001:2022 | 93 | Enterprise ISMS |
| SOC 2 Type II | 74 | SaaS (US market) |
| PCI-DSS v4.0 | 97 | Payment processing |
| HIPAA | 67 | Health data (US) |

### Quality — Web (Certix)

369 rules across 5 profiles (DEV, UX, CONT, OPS, JUR). Two modes: targeted validation (`opo`, before merge) or exhaustive audit (`audit_certix`).

Certix is KARUKIA Solutions' own web quality referential, built for modern web applications.

### Offensive (Viper)

BRIGADE methodology with 16 specialized agents. CVSS v4.0 scoring, MITRE ATT&CK mapping, realistic attack narratives. Web, cloud, and healthcare-specific testing.

### TypeScript Quality

118 checkpoints across 7 categories: type safety, strict configuration, generics, async patterns, modules, errors, and measurable metrics. Grade A-D scoring.

### CSS / Design System

55 checkpoints for maintainability, accessibility, and design system consistency.

### Architecture

70 checkpoints for module structure, coupling/complexity analysis, and architectural layering.

### Test Coverage

68 checkpoints for frontend and backend test inventory and quality sampling.

### Performance

90 checkpoints across frontend performance, backend optimization, and build/bundle analysis.

### Technical Debt

55 checkpoints for dead code detection, dependency health, and code smell identification.

### Expert HDS / ISO 27001

200+ checkpoints across 8 specialized domains for certification readiness: cryptography, audit trail, access control, data classification, multi-tenancy, resilience, vulnerability management, and network security.

### Global Scan (`karukia_scan`)

Meta-orchestrator that runs all 11 dimensions in parallel — 1797+ total checkpoints. Produces a unified scorecard, deduplicates findings, and prioritizes remediation.

---

## 5. The Orchestrator (Auto)

The `auto` tool is the main entry point. The user describes their request in natural language, and the orchestrator routes to the right skill chain automatically.

### Routing table

| Request type | Skill chain |
|-------------|-------------|
| Frontend feature | Jeffrey → Neo → Opo |
| Backend feature | Jeffrey → Neo |
| Security audit | Neo |
| Pentest | Viper |
| Full audit | karukia_scan |
| TypeScript review | ts_quality |
| Architecture review | archi |
| Performance audit | perf |
| Risk analysis | ebios_rm_audit |
| Certification prep | audit_expert_hds |

---

## 6. Memory System

KARUKIA maintains a structured memory across sessions:

```
karukia/
├── config/
│   └── security-scope.md     — Active frameworks and constraints
├── memory/
│   ├── sessions/             — One directory per task session
│   └── references/           — Patterns, lessons, hardening plans
└── trackers/
    └── KARUKIA-TRACKER.json  — Unified chantier tracker (all dimensions)
```

The `KARUKIA-TRACKER.json` file tracks improvement chantiers (tasks) across all 11 dimensions: security, quality, TypeScript, CSS, architecture, tests, performance, and debt. Each skill reads and updates its section automatically.

---

## 7. For Regulated Industries

### KARUKIA Does Not Replace the Auditor

KARUKIA prepares the evidence dossier so that when the auditor arrives, everything is already structured and traced.

### What KARUKIA Generates

| Auditor's Requirement | What KARUKIA Generates |
|----------------------|------------------------|
| Control evidence | Findings traced with file:line reference |
| Risk mapping | EBIOS RM report (5 ANSSI workshops) |
| Technical security policy | `security-scope.md` generated by `install` |
| Per-framework compliance | Neo reports (HDS, ISO, SOC 2, PCI-DSS…) |
| Certification-level audit | `audit_expert_hds` — 8 domains, 200+ checkpoints |
| Change management | `change_report` — ISO 27001 A.8.32 compliance |
| Documented pentest | Viper report with CVSS v4 + MITRE ATT&CK |

### Built from Real Development

KARUKIA was built while developing a healthcare SaaS application targeting HDS 2.0 and ISO 27001 certification. The methodology was designed to answer a real question: what does it actually take to get certified, from day one of development? The checklists are not theoretical — they reflect what a real auditor asks, point by point.

---

## 8. Use Cases

### SaaS Startup — SOC 2 compliance

1. `karukia install` — detects stack, generates config
2. `karukia neo` with SOC 2 + OWASP — full defensive audit
3. `karukia jeffrey` implements fixes → Neo revalidates
4. Exportable report for the SOC 2 auditor

### Healthcare Application — HDS certification

1. `karukia install` — detects health data, activates HDS 2.0
2. `karukia audit_expert_hds` — 8-domain expert audit
3. `karukia viper` — healthcare-specific pentest
4. `karukia ebios_rm_audit` — formal ANSSI risk analysis
5. Complete documentation for the certification dossier

### Development Team — continuous quality

1. `karukia install` on the shared project
2. Developers use `karukia: [task]` daily
3. Every feature goes through Jeffrey → Neo → Opo automatically
4. `karukia_scan` runs periodically for a global health check

---

## 9. Deployment

### Local — Free (available now)

```bash
npx karukia-mcp
```

Each developer runs the server locally via `npx`. Zero infrastructure, zero cost.

---

## 10. Comparison

| Criteria | Generic AI | KARUKIA |
|----------|-----------|---------|
| Methodology | Improvised | 1797+ documented controls |
| Dimensions covered | 1 (whatever you ask) | 11 (security to architecture) |
| Reproducibility | Variable | Deterministic (same checklists) |
| Traceability | None | Findings with file:line |
| Compliance | Impossible to prove | Per-framework reports |
| Cost | — | Free (local, npm) |
| Data sent externally | Depends on provider | None (100% local) |
| Installation | — | `npx karukia-mcp` |

---

## 11. About & License

KARUKIA is developed by **[KARUK IA Solutions](https://karukia.com)**, a B2B SaaS studio specializing in regulated industries (healthcare, finance, pharma), based in Guadeloupe. 🇬🇵

KARUKIA was built while developing a healthcare SaaS application targeting HDS 2.0 and ISO 27001 certification. The methodology was designed to answer a real question: what does it actually take to get certified, from day one of development?

> *Made in Guadeloupe — AI doesn't replace the expert, it frees them.*

KARUKIA MCP is **free for personal and educational use**. No account required.

For commercial use, contact **KARUKIA Solutions** — contact@karukia.com

**npm:** [karukia-mcp](https://www.npmjs.com/package/karukia-mcp)

**GitHub:** [github.com/guillaum-h/KARUKIA](https://github.com/guillaum-h/KARUKIA)
