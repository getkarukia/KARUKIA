# KARUKIA — Livre Blanc

**Méthodologie de développement assistée par IA : sécurité, qualité et pentest via le protocole MCP.**

*Document en français. English version: [WHITEPAPER.md](./WHITEPAPER.md)*

```
██╗  ██╗ █████╗ ██████╗ ██╗   ██╗██╗  ██╗    ██╗ █████╗
██║ ██╔╝██╔══██╗██╔══██╗██║   ██║██║ ██╔╝    ██║██╔══██╗
█████╔╝ ███████║██████╔╝██║   ██║█████╔╝     ██║███████║
██╔═██╗ ██╔══██║██╔══██╗██║   ██║██╔═██╗     ██║██╔══██║
██║  ██╗██║  ██║██║  ██║╚██████╔╝██║  ██╗    ██║██║  ██║
╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝ ╚═╝  ╚═╝    ╚═╝╚═╝  ╚═╝
   Méthodologie IA pour les domaines réglementés · Made in Guadeloupe 🇬🇵
```

---

## 1. Le problème

Les assistants IA (Claude, GPT, Copilot) sont puissants mais génériques. Quand on demande un audit de sécurité, l'IA improvise : elle connaît les concepts OWASP mais ne suit pas une méthodologie structurée. Le résultat varie d'une session à l'autre, sans exhaustivité garantie.

**Conséquences concrètes :**

- Des vulnérabilités passent entre les mailles parce que l'IA « oublie » de vérifier certains points
- Aucune traçabilité : impossible de prouver qu'un contrôle a été effectué
- Pas de reproductibilité : deux audits du même code donnent des résultats différents
- Zéro conformité : les frameworks (ISO 27001, SOC 2, HDS) exigent des preuves formelles

## 2. La solution KARUKIA

KARUKIA transforme le problème en injectant une **méthodologie complète** directement dans le contexte de l'IA, à la demande.

### Comment ça marche

KARUKIA est un serveur MCP (Model Context Protocol) — le standard ouvert d'Anthropic pour connecter des outils à une IA. Quand l'utilisateur demande un audit, le serveur MCP retourne un **prompt monolithique** contenant :

1. **L'identité du persona** — nom, expertise, style de communication
2. **Le workflow** — étapes obligatoires, dans l'ordre, avec gates de validation
3. **Les checklists** — tous les points de contrôle pertinents, inline dans le prompt
4. **Les templates** — format de sortie attendu (tables, scores, verdicts)
5. **Les garde-fous** — règles non-négociables (ex. : « chaque finding doit citer fichier:ligne »)

L'IA reçoit ce prompt et **devient** le spécialiste pour la durée de la session. Elle ne peut pas « oublier » un contrôle — il est écrit dans son contexte.

### Le protocole MCP

Le [Model Context Protocol](https://modelcontextprotocol.io/) est un standard ouvert qui permet à n'importe quel client IA de se connecter à des serveurs d'outils. KARUKIA fonctionne avec :

- **Claude Code** (CLI)
- **Claude Desktop**
- **Cursor**
- **Windsurf**
- **Tout client compatible MCP**

L'installation se fait en une ligne :

```bash
npx karukia-mcp
```

Aucun compte, aucune clé API, aucune donnée envoyée à l'extérieur. Le serveur tourne en local sur la machine du développeur.

---

## 3. Architecture

### Transport local (par défaut)

```
Développeur <-> Client IA <-> [stdio] <-> KARUKIA MCP Server (local)
                                              |
                                              ├── 11 skills (prompt builders)
                                              ├── 24 checklists (935+ points)
                                              └── Système de mémoire (sessions, knowledge)
```

Le serveur communique via **stdio** (entrée/sortie standard). Aucun port réseau ouvert, aucun appel HTTP. Tout reste sur la machine locale.

### Transport distant (équipe)

```
Équipe <-> Clients IA <-> [HTTPS] <-> KARUKIA MCP Server (géré)
                                          |
                                          ├── Auth bearer (MCP_API_KEY)
                                          ├── Rate limiting par session
                                          ├── Audit trail (Pino logs)
                                          └── Même moteur, mêmes checklists
```

Pour les équipes, KARUKIA propose un serveur géré accessible via clé API. Ce mode est en cours de développement — **liste d'attente ouverte** sur contact@karukia.com.

### Ce que le serveur retourne

Chaque tool MCP retourne un **prompt monolithique** — un seul bloc de texte contenant tout ce dont l'IA a besoin. Pas de chaînes d'appels, pas de RAG, pas de base vectorielle. Juste un prompt bien construit.

Exemple simplifié pour `neo` (audit sécurité) :

```
[GUARD v2 — obligations non-négociables]
+
[Persona Neo — identité, style, expertise]
+
[Workflow — 5 étapes obligatoires]
+
[Checklist OWASP — 62 contrôles inline]
+
[Checklist ISO 27001 — 93 contrôles inline]
+
[Template de sortie — format table + verdict]
```

Taille typique d'un prompt skill : 15-40 Ko selon les checklists actives.

---

## 4. Les trois couches d'audit

### Couche 1 — Audit défensif (Neo)

```
  ╔═══════════════════════════════════════════════════════╗
  ║    ●─────────────────────────────────────────●       ║
  ║    │   ◉               N E O               ◉ │       ║
  ║    │        Auditeur Cybersécurité            │       ║
  ║    ●─────────────────────────────────────────●       ║
  ║   OWASP · HDS · ISO 27001 · SOC 2 · PCI · HIPAA     ║
  ╚═══════════════════════════════════════════════════════╝
```

**Persona :** Neo, auditeur cybersécurité senior.

**Méthode :** Vérification point par point de chaque contrôle applicable. Chaque finding doit citer le fichier et la ligne. Chaque verdict est soit CONFORME, soit NON-CONFORME avec preuve.

**Frameworks supportés :**

| Framework | Points | Périmètre |
|-----------|--------|-----------|
| OWASP Security Baseline | 62 | Toute application web |
| HDS 2.0 | 52 | Données de santé (France) |
| ISO 27001:2022 | 93 | SMSI entreprise |
| SOC 2 Type II | 74 | SaaS (marché US) |
| PCI-DSS v4.0 | 97 | Traitement de paiements |
| HIPAA | 67 | Données de santé (US) |

**Sortie :** Table de findings avec sévérité (CRITIQUE / MAJEUR / MINEUR / INFO), référence checklist, fichier:ligne, et verdict global APPROUVÉ ou REJETÉ.

**Chaîne :** Neo est systématiquement appelé après Jeffrey (implémentation) pour valider la sécurité du code produit. Si Neo rejette, Jeffrey corrige — boucle maximum 3 itérations.

### Couche 2 — Audit qualité (Opo / Opquast)

**Persona :** Opo, gardien de la qualité web.

**Méthode :** Vérification des 245 règles Opquast v5.0 réparties en 14 catégories thématiques. Deux modes :

- **opo** — Validation ciblée sur les fichiers modifiés (rapide, avant merge)
- **audit_opquast** — Audit exhaustif des 245 règles (complet, avec scoring)

**Catégories :** Contenus, données personnelles, e-commerce, formulaires, identification, images et médias, internationalisation, liens, navigation, newsletter, présentation, sécurité UX, serveur et performances, structure et code.

**Base :** [Opquast](https://www.opquast.com/) — le référentiel français de qualité web utilisé par plus de 15 000 professionnels.

### Couche 3 — Audit offensif (Viper)

**Persona :** V.I.P.E.R., hacker éthique certifié (OSCP, OSCE, OSWE, GWAPT).

**Méthode :** Méthodologie BRIGADE avec 16 agents spécialisés répartis en 3 phases :

1. **Reconnaissance** (5 agents) — Backend, frontend, config, dépendances, données
2. **Surface d'attaque** (3 agents) — Matrice de contrôles, flux de données, STRIDE
3. **Vérification d'exploits** (5-6 agents) — Par catégorie OWASP + cloud + supply chain

**Scoring :** CVSS v4.0 pour chaque finding, mapping MITRE ATT&CK, narratifs d'attaque réalistes.

**Checklists :**

| Checklist | Tests | Périmètre |
|-----------|-------|-----------|
| OWASP WSTG v5 | 100 | Pentest web |
| Cloud Platform | 80+ | Firebase, GCP, AWS, Azure |
| Healthcare | 50+ | PHI, chiffrement, données médicales |
| Scénarios d'attaque | 15+ | Templates PTES, MITRE ATT&CK |

---

## 5. L'orchestrateur (Auto)

L'outil `auto` est le point d'entrée principal. L'utilisateur décrit sa demande en langage naturel, et l'orchestrateur :

1. **Analyse** la requête (type, scope, complexité)
2. **Route** vers la bonne chaîne de skills
3. **Gère la boucle de rejet** (si Neo rejette, Jeffrey corrige, max 3 itérations)
4. **Consolide** le rapport final

### Table de routage

| Type de demande | Chaîne de skills |
|----------------|-----------------|
| Feature frontend | Jeffrey → Neo → Opo |
| Feature backend | Jeffrey → Neo |
| Correction de bug | Jeffrey → Neo |
| Audit sécurité | Neo seul |
| Pentest | Viper seul |
| Audit qualité | audit_opquast seul |
| Analyse de risques | ebios_rm_audit seul |
| Documentation | doc_refactor seul |
| Infrastructure | terraform_update → Neo |
| Durcissement | security_hardening → Neo |

### Utilisation

```
« karukia: ajoute l'authentification utilisateur »
    → Jeffrey implémente → Neo valide la sécurité → Opo vérifie la qualité

« karukia: audite la sécurité de mon projet »
    → Neo audite point par point → rapport structuré

« karukia: lance un pentest »
    → Viper déploie 16 agents → CVSS scoring → narratifs d'attaque
```

---

## 6. Système de mémoire

KARUKIA maintient une mémoire structurée entre les sessions :

```
KARUKIA/
└── memory/
    ├── INDEX.md              — Index des sessions
    ├── sessions/             — Une session par tâche
    │   └── YYYY-MM-DD_xxx/
    │       ├── task_plan.md  — Plan et objectifs
    │       ├── findings.md   — Découvertes
    │       ├── progress.md   — Journal de progression
    │       └── context.json  — Contexte machine-readable
    ├── knowledge/
    │   ├── patterns.md       — Patterns récurrents du projet
    │   └── lessons.md        — Leçons apprises
    └── config/
        └── security-scope.md — Frameworks et contraintes actives
```

Cela permet à KARUKIA de **capitaliser** sur les sessions précédentes : patterns détectés, leçons apprises, décisions architecturales documentées.

---

## 7. Pour les secteurs réglementés

### Le vrai coût d'une certification

Obtenir une certification HDS, ISO 27001 ou SOC 2 est coûteux — non pas parce que l'auditeur humain est incompétent, mais parce que **la documentation est absente le jour J**. Les preuves sont dispersées dans des Jira, des e-mails, des commits sans structure. Les correctifs ont été appliqués en urgence, sans traçabilité.

**KARUKIA ne remplace pas l'auditeur humain. Il prépare le dossier.**

### Ce que KARUKIA produit pour votre certification

| Besoin de l'auditeur | Ce que KARUKIA génère |
|----------------------|-----------------------|
| Preuve de contrôle | Findings tracés avec référence fichier:ligne |
| Cartographie des risques | Rapport EBIOS RM (5 ateliers ANSSI) |
| Politique de sécurité technique | `security-scope.md` généré par `install` |
| Conformité par framework | Rapports Neo (HDS, ISO, SOC 2, PCI-DSS…) |
| Historique des corrections | Mémoire structurée inter-sessions |
| Pentest documenté | Rapport Viper avec scoring CVSS v4 + MITRE ATT&CK |

### Pourquoi KARUKIA est différent des autres outils

| Critère | Outils d'audit génériques | KARUKIA |
|---------|--------------------------|---------|
| Couverture frameworks | Un framework à la fois | 6 frameworks + EBIOS RM en un seul outil |
| Construction de preuves | Snapshots manuels | Mémoire structurée inter-sessions |
| Cycle complet | Audit seul | Code → Sécurité → Qualité → Pentest |
| Origine | Théorique | Né d'une vraie certification HDS/ISO 27001 |
| Qualité web | Absente | 245 règles Opquast (référentiel français) |

### Construit sur une vraie expérience

KARUKIA est né de l'expérience de sécurisation d'un SaaS de santé en vue des certifications HDS 2.0 et ISO 27001. Les checklists ne sont pas théoriques — elles reflètent ce qu'un auditeur réel demande, point par point.

---

## 8. Cas d'usage

### Startup SaaS — mise en conformité SOC 2

1. `karukia install` — détecte le stack (React + Node.js + PostgreSQL)
2. `karukia neo` avec frameworks SOC 2 + OWASP — audit complet
3. Les findings deviennent des chantiers de durcissement
4. `karukia jeffrey` implémente les corrections → Neo revalide
5. Rapport exportable pour l'auditeur SOC 2

### Application de santé — certification HDS

1. `karukia install` — détecte les données de santé, active HDS 2.0
2. `karukia neo` avec frameworks HDS + ISO 27001 — audit défensif
3. `karukia viper` — pentest offensif spécifique santé
4. `karukia ebios_rm_audit` — analyse de risques formelle ANSSI
5. Documentation complète pour le dossier de certification

### Équipe de développement — qualité continue

1. `karukia install` sur le projet partagé
2. Les développeurs utilisent `karukia: [tâche]` au quotidien
3. Chaque feature passe par Jeffrey → Neo → Opo automatiquement
4. Les audits complets (`neo`, `viper`, `audit_opquast`) sont lancés périodiquement

---

## 9. Déploiement

### Local — Gratuit (disponible maintenant)

```bash
npx karukia-mcp
```

Chaque développeur installe KARUKIA en local via `npx`. Zéro serveur, zéro infrastructure, zéro coût.

### Équipe — Serveur géré (liste d'attente)

Un serveur KARUKIA géré permet à toute une équipe de se connecter via une clé API unique :

- **Cohérence** — les mêmes checklists pour tous les développeurs
- **Audit trail centralisé** — logs structurés Pino JSON
- **Gestion des accès** — bearer token par équipe
- **Disponibilité** — sans dépendance à la machine locale

> Ce mode est en cours de développement. Rejoignez la liste d'attente : **contact@karukia.com**

---

## 10. Comparaison

| Critère | IA générique | KARUKIA |
|---------|-------------|---------|
| Méthodologie | Improvisée | 935+ contrôles documentés |
| Reproductibilité | Variable | Déterministe (mêmes checklists) |
| Traçabilité | Aucune | Findings avec fichier:ligne |
| Conformité | Impossible à prouver | Rapports par framework |
| Coût | — | Gratuit (local, npm) |
| Données envoyées | Selon le provider | Aucune (100 % local) |
| Installation | — | `npx karukia-mcp` |

---

## 11. À propos

KARUKIA est développé par **[KARUK IA Solutions](https://karukia.com)**, studio SaaS B2B spécialisé dans les domaines réglementés (santé, finance, pharma), basé en Guadeloupe. 🇬🇵

La méthode KARUKIA est la méthodologie interne utilisée pour construire et certifier des produits SaaS dans des environnements HDS 2.0, ISO 27001 et RGPD. Elle est mise à disposition gratuitement pour usage personnel, éducatif et professionnel interne.

> *Made in Guadeloupe — L'IA ne remplace pas l'expert, elle le libère.*

---

## 12. Licence et contact

KARUKIA MCP est **gratuit** pour un usage personnel, éducatif et professionnel interne. Aucun compte requis.

L'usage commercial ou la revente nécessitent une autorisation écrite.

**Contact :** contact@karukia.com

**npm :** [karukia-mcp](https://www.npmjs.com/package/karukia-mcp)

**GitHub :** [github.com/guillaum-h/KARUKIA](https://github.com/guillaum-h/KARUKIA)
