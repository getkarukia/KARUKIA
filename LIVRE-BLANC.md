# KARUKIA — Livre Blanc

**Méthodologie de développement assistée par IA : 11 dimensions d'audit via le protocole MCP.**

*Version 3.0 — Document en français. English version: [WHITEPAPER.md](./WHITEPAPER.md)*

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
- Aucune visibilité sur la qualité du code TypeScript, l'architecture, les performances, ou la dette technique

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
                                              ├── 20 skills (prompt builders)
                                              ├── 31 checklists (1673+ points)
                                              └── Système de mémoire (sessions, trackers)
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

Pour les équipes, KARUKIA supporte un transport HTTP streamable avec authentification bearer token, logs structurés et rate limiting par session.

### Ce que le serveur retourne

Chaque tool MCP retourne un **prompt monolithique** — un seul bloc de texte contenant tout ce dont l'IA a besoin. Pas de chaînes d'appels, pas de RAG, pas de base vectorielle. Juste un prompt bien construit.

Exemple simplifié pour `neo` (audit sécurité) :

```
[GUARD — obligations non-négociables]
+
[Persona Neo — identité, style, expertise]
+
[Workflow — étapes obligatoires]
+
[Checklist OWASP — 62 contrôles inline]
+
[Checklist ISO 27001 — 93 contrôles inline]
+
[Template de sortie — format table + verdict]
```

---

## 4. Les 11 dimensions d'audit

### Sécurité défensive (Neo)

Vérification point par point de chaque contrôle applicable. Chaque finding cite le fichier et la ligne. Chaque verdict est soit CONFORME, soit NON-CONFORME avec preuve.

| Framework | Points | Périmètre |
|-----------|--------|-----------|
| OWASP Security Baseline | 62 | Toute application web |
| HDS 2.0 | 52 | Données de santé (France) |
| ISO 27001:2022 | 93 | SMSI entreprise |
| SOC 2 Type II | 74 | SaaS (marché US) |
| PCI-DSS v4.0 | 97 | Traitement de paiements |
| HIPAA | 67 | Données de santé (US) |

Neo est systématiquement appelé après Jeffrey (implémentation). Si Neo rejette, Jeffrey corrige — boucle maximum 3 itérations.

### Qualité web (Opquast)

245 règles Opquast v5.0 en 14 catégories. Deux modes : validation ciblée avant merge (`opo`) ou audit exhaustif (`audit_opquast`). Basé sur [Opquast](https://www.opquast.com/) — le référentiel français de qualité web en libre accès (CC-BY-SA), utilisé par plus de 19 000 professionnels certifiés depuis 2004. Merci à Opquast de mettre leur référentiel à disposition de la communauté.

### Sécurité offensive (Viper)

Méthodologie BRIGADE avec 16 agents spécialisés. Scoring CVSS v4.0, mapping MITRE ATT&CK, narratifs d'attaque réalistes. Couverture web, cloud, et santé.

### TypeScript Quality

118 points de contrôle en 7 catégories : type safety, config stricte, génériques, patterns async, modules, erreurs, métriques. Notation A à D.

### CSS / Design System

55 points de contrôle pour la maintenabilité, l'accessibilité, et la cohérence du design system.

### Architecture

70 points de contrôle pour la structure des modules, le couplage/complexité, et le respect du layering.

### Couverture de tests

68 points de contrôle pour l'inventaire frontend/backend et la qualité des tests par échantillonnage.

### Performance

90 points de contrôle couvrant le frontend, le backend, et l'analyse du build/bundle.

### Dette technique

55 points de contrôle pour le code mort, la santé des dépendances, et les code smells.

### Expert HDS / ISO 27001

200+ points de contrôle répartis en 8 domaines pour préparer la certification : cryptographie, audit trail, contrôle d'accès, classification des données, multi-tenant, résilience, gestion des vulnérabilités, réseau.

### Scan global (`karukia_scan`)

Méta-orchestrateur qui lance les 11 dimensions en parallèle — 1673+ points au total. Produit un scorecard unifié, déduplique les findings, et priorise les corrections.

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
| Audit sécurité | Neo |
| Pentest | Viper |
| Audit complet | karukia_scan |
| Qualité TypeScript | ts_quality |
| Architecture | archi |
| Performance | perf |
| Analyse de risques | ebios_rm_audit |
| Préparation certification | audit_expert_hds |

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
karukia/
├── config/
│   └── security-scope.md     — Frameworks et contraintes actives
├── memory/
│   ├── sessions/             — Une session par tâche
│   └── references/           — Patterns, leçons, plans de durcissement
└── trackers/
    └── KARUKIA-TRACKER.json  — Tracker unifié (toutes les dimensions)
```

Le `KARUKIA-TRACKER.json` trace les chantiers d'amélioration sur toutes les dimensions : sécurité, qualité, TypeScript, CSS, architecture, tests, performance et dette. Chaque skill lit et met à jour sa section automatiquement.

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
| Audit niveau expert | `audit_expert_hds` — 8 domaines, 200+ points |
| Rapport de changements | `change_report` — ISO 27001 A.8.32 |
| Pentest documenté | Rapport Viper avec scoring CVSS v4 + MITRE ATT&CK |

### Pourquoi KARUKIA est différent des autres outils

| Critère | Outils d'audit génériques | KARUKIA |
|---------|--------------------------|---------|
| Couverture frameworks | Un framework à la fois | 6 frameworks + EBIOS RM en un seul outil |
| Dimensions couvertes | 1 (sécurité) | 11 (sécurité à architecture) |
| Construction de preuves | Snapshots manuels | Mémoire structurée inter-sessions |
| Cycle complet | Audit seul | Code → Sécurité → Qualité → Pentest |
| Origine | Théorique | Construit en préparant une vraie certification HDS/ISO 27001 |
| Qualité web | Absente | 245 règles Opquast (référentiel français) |

### Construit sur le terrain

KARUKIA est né du développement d'un SaaS de santé en cours de préparation pour les certifications HDS 2.0 et ISO 27001. La méthodologie a été construite pour répondre à une vraie question : qu'est-ce que la certification exige concrètement, dès le premier jour de développement ? Les checklists reflètent ce qu'un auditeur réel demande, point par point — pas de la théorie.

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
2. `karukia audit_expert_hds` — audit expert 8 domaines
3. `karukia viper` — pentest offensif spécifique santé
4. `karukia ebios_rm_audit` — analyse de risques formelle ANSSI
5. Documentation complète pour le dossier de certification

### Équipe de développement — qualité continue

1. `karukia install` sur le projet partagé
2. Les développeurs utilisent `karukia: [tâche]` au quotidien
3. Chaque feature passe par Jeffrey → Neo → Opo automatiquement
4. `karukia_scan` lancé périodiquement pour un bilan global sur les 11 dimensions

---

## 9. Déploiement

### Local — Gratuit (disponible maintenant)

```bash
npx karukia-mcp
```

Chaque développeur installe KARUKIA en local via `npx`. Zéro serveur, zéro infrastructure, zéro coût.

---

## 10. Comparaison

| Critère | IA générique | KARUKIA |
|---------|-------------|---------|
| Méthodologie | Improvisée | 1673+ contrôles documentés |
| Dimensions couvertes | 1 (ce qu'on demande) | 11 (sécurité à architecture) |
| Reproductibilité | Variable | Déterministe (mêmes checklists) |
| Traçabilité | Aucune | Findings avec fichier:ligne |
| Conformité | Impossible à prouver | Rapports par framework |
| Coût | — | Gratuit (local, npm) |
| Données envoyées | Selon le provider | Aucune (100 % local) |
| Installation | — | `npx karukia-mcp` |

---

## 11. À propos & Licence

KARUKIA est développé par **[KARUK IA Solutions](https://karukia.com)**, studio SaaS B2B spécialisé dans les domaines réglementés (santé, finance, pharma), basé en Guadeloupe. 🇬🇵

KARUKIA est né du développement d'un SaaS de santé en cours de préparation pour les certifications HDS 2.0 et ISO 27001. La méthodologie a été construite pour répondre à une vraie question : qu'est-ce que la certification exige concrètement, dès le premier jour de développement ?

> *Made in Guadeloupe — L'IA ne remplace pas l'expert, elle le libère.*

KARUKIA MCP est **gratuit pour usage personnel et éducatif**. Aucun compte requis.

Pour toute utilisation commerciale, contactez **KARUKIA Solutions** — contact@karukia.com

**npm :** [karukia-mcp](https://www.npmjs.com/package/karukia-mcp)

**GitHub :** [github.com/guillaum-h/KARUKIA](https://github.com/guillaum-h/KARUKIA)
