# KARUKIA — Livre Blanc

**Methodologie de developpement assistee par IA : securite, qualite et pentest via le protocole MCP.**

*Document en francais. English version: [WHITEPAPER.md](./WHITEPAPER.md)*

---

## 1. Le probleme

Les assistants IA (Claude, GPT, Copilot) sont puissants mais generiques. Quand on demande un audit de securite, l'IA improvise : elle connait les concepts OWASP mais ne suit pas une methodologie structuree. Le resultat varie d'une session a l'autre, sans exhaustivite garantie.

**Consequences concretes :**

- Des vulnerabilites passent entre les mailles parce que l'IA "oublie" de verifier certains points
- Aucune tracabilite : impossible de prouver qu'un controle a ete effectue
- Pas de reproductibilite : deux audits du meme code donnent des resultats differents
- Zero conformite : les frameworks (ISO 27001, SOC 2, HDS) exigent des preuves formelles

## 2. La solution KARUKIA

KARUKIA transforme le probleme en injectant une **methodologie complete** directement dans le contexte de l'IA, a la demande.

### Comment ca marche

KARUKIA est un serveur MCP (Model Context Protocol) — le standard ouvert d'Anthropic pour connecter des outils a une IA. Quand l'utilisateur demande un audit, le serveur MCP retourne un **prompt monolithique** contenant :

1. **L'identite du persona** — nom, expertise, style de communication
2. **Le workflow** — etapes obligatoires, dans l'ordre, avec gates de validation
3. **Les checklists** — tous les points de controle pertinents, inline dans le prompt
4. **Les templates** — format de sortie attendu (tables, scores, verdicts)
5. **Les garde-fous** — regles non-negociables (ex: "chaque finding doit citer file:line")

L'IA recoit ce prompt et **devient** le specialiste pour la duree de la session. Elle ne peut pas "oublier" un controle — il est ecrit dans son contexte.

### Le protocole MCP

Le [Model Context Protocol](https://modelcontextprotocol.io/) est un standard ouvert qui permet a n'importe quel client IA de se connecter a des serveurs d'outils. KARUKIA fonctionne avec :

- **Claude Code** (CLI)
- **Claude Desktop**
- **Cursor**
- **Windsurf**
- **Tout client compatible MCP**

L'installation se fait en une ligne :

```bash
npx karukia-mcp
```

Aucun compte, aucune cle API, aucune donnee envoyee a l'exterieur. Le serveur tourne en local sur la machine du developpeur.

---

## 3. Architecture

### Transport local (par defaut)

```
Developpeur <-> Client IA <-> [stdio] <-> KARUKIA MCP Server (local)
                                              |
                                              ├── 11 skills (prompt builders)
                                              ├── 24 checklists (935+ points)
                                              └── Systeme de memoire (sessions, knowledge)
```

Le serveur communique via **stdio** (entree/sortie standard). Aucun port reseau ouvert, aucun appel HTTP. Tout reste sur la machine locale.

### Transport distant (entreprise)

```
Equipe <-> Clients IA <-> [HTTPS] <-> KARUKIA MCP Server (Cloud Run)
                                          |
                                          ├── Auth bearer (MCP_API_KEY)
                                          ├── Rate limiting par session
                                          ├── Audit trail (Pino logs)
                                          └── Meme moteur, memes checklists
```

Pour les deploiements en equipe, KARUKIA supporte un transport **HTTP streamable** deployable sur votre propre infrastructure (Cloud Run, Kubernetes, VM). Authentification par bearer token, logs structures, rate limiting.

### Ce que le serveur retourne

Chaque tool MCP retourne un **prompt monolithique** — un seul bloc de texte contenant tout ce dont l'IA a besoin. Pas de chaines d'appels, pas de RAG, pas de base vectorielle. Juste un prompt bien construit.

Exemple simplifie pour `neo` (audit securite) :

```
[GUARD v2 — obligations non-negociables]
+
[Persona Neo — identite, style, expertise]
+
[Workflow — 5 etapes obligatoires]
+
[Checklist OWASP — 62 controles inline]
+
[Checklist ISO 27001 — 93 controles inline]
+
[Template de sortie — format table + verdict]
```

Taille typique d'un prompt skill : 15-40 KB selon les checklists actives.

---

## 4. Les trois couches d'audit

### Couche 1 — Audit defensif (Neo)

**Persona :** Neo, auditeur cybersecurite senior.

**Methode :** Verification point par point de chaque controle applicable. Chaque finding doit citer le fichier et la ligne. Chaque verdict est soit CONFORME, soit NON-CONFORME avec preuve.

**Frameworks supportes :**

| Framework | Points | Perimetre |
|-----------|--------|-----------|
| OWASP Security Baseline | 62 | Toute application web |
| HDS 2.0 | 52 | Donnees de sante (France) |
| ISO 27001:2022 | 93 | SMSI entreprise |
| SOC 2 Type II | 74 | SaaS (marche US) |
| PCI-DSS v4.0 | 97 | Traitement de paiements |
| HIPAA | 67 | Donnees de sante (US) |

**Sortie :** Table de findings avec severite (CRITIQUE / MAJEUR / MINEUR / INFO), reference checklist, fichier:ligne, et verdict global APPROUVE ou REJETE.

**Chaine :** Neo est systematiquement appele apres Jeffrey (implementation) pour valider la securite du code produit. Si Neo rejette, Jeffrey corrige — boucle maximum 3 iterations.

### Couche 2 — Audit qualite (Opo / Opquast)

**Persona :** Opo, gardien de la qualite web.

**Methode :** Verification des 245 regles Opquast v5.0 reparties en 14 categories thematiques. Deux modes :

- **opo** — Validation ciblee sur les fichiers modifies (rapide, avant merge)
- **audit_opquast** — Audit exhaustif des 245 regles (complet, avec scoring)

**Categories :** Contenus, donnees personnelles, e-commerce, formulaires, identification, images et medias, internationalisation, liens, navigation, newsletter, presentation, securite UX, serveur et performances, structure et code.

**Base :** [Opquast](https://www.opquast.com/) — le referentiel francais de qualite web utilise par plus de 15 000 professionnels.

### Couche 3 — Audit offensif (Viper)

**Persona :** V.I.P.E.R., hacker ethique certifie (OSCP, OSCE, OSWE, GWAPT).

**Methode :** Methodologie BRIGADE avec 16 agents specialises repartis en 3 phases :

1. **Reconnaissance** (5 agents) — Backend, frontend, config, deps, data
2. **Surface d'attaque** (3 agents) — Matrice de controles, flux de donnees, STRIDE
3. **Verification d'exploits** (5-6 agents) — Par categorie OWASP + cloud + supply chain

**Scoring :** CVSS v4.0 pour chaque finding, mapping MITRE ATT&CK, narratifs d'attaque realistes.

**Checklists :**

| Checklist | Tests | Perimetre |
|-----------|-------|-----------|
| OWASP WSTG v5 | 100 | Pentest web |
| Cloud Platform | 80+ | Firebase, GCP, AWS, Azure |
| Healthcare | 50+ | PHI, chiffrement, donnees medicales |
| Scenarios d'attaque | 15+ | Templates PTES, MITRE ATT&CK |

---

## 5. L'orchestrateur (Auto)

L'outil `auto` est le point d'entree principal. L'utilisateur decrit sa demande en langage naturel, et l'orchestrateur :

1. **Analyse** la requete (type, scope, complexite)
2. **Route** vers la bonne chaine de skills
3. **Gere la boucle de rejet** (si Neo rejette, Jeffrey corrige, max 3 iterations)
4. **Consolide** le rapport final

### Table de routage

| Type de demande | Chaine de skills |
|----------------|-----------------|
| Feature frontend | Jeffrey → Neo → Opo |
| Feature backend | Jeffrey → Neo |
| Correction de bug | Jeffrey → Neo |
| Audit securite | Neo seul |
| Pentest | Viper seul |
| Audit qualite | audit_opquast seul |
| Analyse de risques | ebios_rm_audit seul |
| Documentation | doc_refactor seul |
| Infrastructure | terraform_update → Neo |
| Durcissement | security_hardening → Neo |

### Utilisation

```
"karukia: ajoute l'authentification utilisateur"
    → Jeffrey implemente → Neo valide la securite → Opo verifie la qualite

"karukia: audite la securite de mon projet"
    → Neo audite point par point → rapport structure

"karukia: lance un pentest"
    → Viper deploie 16 agents → CVSS scoring → narratifs d'attaque
```

---

## 6. Systeme de memoire

KARUKIA maintient une memoire structuree entre les sessions :

```
KARUKIA/
└── memory/
    ├── INDEX.md              — Index des sessions
    ├── sessions/             — Une session par tache
    │   └── YYYY-MM-DD_xxx/
    │       ├── task_plan.md  — Plan et objectifs
    │       ├── findings.md   — Decouvertes
    │       ├── progress.md   — Journal de progression
    │       └── context.json  — Contexte machine-readable
    ├── knowledge/
    │   ├── patterns.md       — Patterns recurrents du projet
    │   └── lessons.md        — Lecons apprises
    └── config/
        └── security-scope.md — Frameworks et contraintes actives
```

Cela permet a KARUKIA de **capitaliser** sur les sessions precedentes : patterns detectes, lecons apprises, decisions architecturales documentees.

---

## 7. Cas d'usage

### Startup SaaS — mise en conformite SOC 2

1. `karukia install` — detecte le stack (React + Node.js + PostgreSQL)
2. `karukia neo` avec frameworks SOC 2 + OWASP — audit complet
3. Les findings deviennent des chantiers de durcissement
4. `karukia jeffrey` implemente les corrections → Neo revalide
5. Rapport exportable pour l'auditeur SOC 2

### Application de sante — certification HDS

1. `karukia install` — detecte les donnees de sante, active HDS 2.0
2. `karukia neo` avec frameworks HDS + ISO 27001 — audit defensif
3. `karukia viper` — pentest offensif specifique sante
4. `karukia ebios_rm_audit` — analyse de risques formelle ANSSI
5. Documentation complete pour le dossier de certification

### Equipe de developpement — qualite continue

1. `karukia install` sur le projet partage
2. Les developpeurs utilisent `karukia: [tache]` au quotidien
3. Chaque feature passe par Jeffrey → Neo → Opo automatiquement
4. Les audits complets (`neo`, `viper`, `audit_opquast`) sont lances periodiquement

---

## 8. Deploiement entreprise

### Local (par defaut)

```bash
npx karukia-mcp
```

Chaque developpeur lance le serveur localement. Zero infrastructure, zero cout.

### Equipe (serveur partage)

Un serveur KARUKIA deploye sur votre infrastructure (Cloud Run, Kubernetes, VM) avec :

- **Authentification** par bearer token (`MCP_API_KEY`)
- **Rate limiting** par session
- **Audit trail** structure (logs Pino JSON)
- **Checklists personnalisees** possibles

Contact **contact@karukia.com** pour un accompagnement deploiement.

---

## 9. Comparaison

| Critere | IA generique | KARUKIA |
|---------|-------------|---------|
| Methodologie | Improvisee | 935+ controles documentes |
| Reproductibilite | Variable | Deterministe (memes checklists) |
| Tracabilite | Aucune | Findings avec file:line |
| Conformite | Impossible a prouver | Rapports par framework |
| Cout | — | Gratuit (local, npm) |
| Donnees envoyees | Selon le provider | Aucune (100% local) |
| Installation | — | `npx karukia-mcp` |

---

## 10. Licence et contact

KARUKIA MCP est **gratuit** pour un usage personnel, educatif et professionnel interne. Aucun compte requis.

L'usage commercial ou la revente necessitent une autorisation ecrite.

**Contact :** contact@karukia.com

**npm :** [karukia-mcp](https://www.npmjs.com/package/karukia-mcp)

**GitHub :** [github.com/guillaum-h/KARUKIA](https://github.com/guillaum-h/KARUKIA)
