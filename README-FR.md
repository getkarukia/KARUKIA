# KARUKIA MCP

```
    ○─┬─○                              ○─┬─○
      │   ╲       K A R U K  I A     ╱   │
    ○─┤    ○────────────────────────○    ├─○
      │         ≋≋  ◈ · MCP · ◈  ≋≋      │
    ○─┴─○                              ○─┴─○
                 Made in Guadeloupe  🇬🇵
```

**La méthodologie complète de développement assistée par IA, livrée via MCP.**

**Dernière version : v1.2.4** — Avatars terminaux pour Neo & Jeffrey + enrichissements docs.

21 outils, 11 skills, 935+ points de contrôle sécurité / qualité / pentest. Compatible avec n'importe quelle plateforme IA (Claude Code, Cursor, Windsurf, Copilot…) via le Model Context Protocol.

*English version: [README.md](./README.md)*

---

## Qu'est-ce que KARUKIA ?

KARUKIA est une méthodologie de développement structurée, construite autour de personas IA spécialisés. Chaque persona (Neo pour la sécurité, Jeffrey pour l'architecture, Viper pour le pentest, Opo pour la qualité…) embarque son propre workflow, ses garde-fous et sa base de connaissances.

Quand tu appelles un outil KARUKIA, le serveur MCP retourne un prompt complet — identité du persona, workflow, checklists, templates — qui transforme ton assistant IA en ce spécialiste pour la session.

```
Toi : « Audite la sécurité de mon projet »
  → l'IA appelle l'outil neo
  → le serveur MCP retourne le prompt complet de Neo + 445 contrôles inline
  → l'IA devient Neo, suit la méthodologie, produit un rapport structuré
```

---

## Les trois couches

```
Couche 1 — DÉFENSIF (Neo)     445 contrôles   « Mon code est-il sécurisé ? »
Couche 2 — QUALITÉ (Opquast)  245 règles      « Mon app est-elle bien construite ? »
Couche 3 — OFFENSIF (Viper)   245 tests       « Comment un attaquant pourrait-il entrer ? »
```

---

## Démarrage rapide

**Prérequis :** [Node.js](https://nodejs.org/) 20 ou supérieur.

### Étape 1 — Ajouter KARUKIA à ton projet

Crée ou édite `.mcp.json` à la racine de ton projet :

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

> **Note :** Si le fichier existe déjà avec d'autres serveurs MCP, ajoute simplement la clé `"karukia"` dans l'objet `"mcpServers"` existant.

### Étape 2 — Redémarre ton client IA

Redémarre Claude Code (`/quit` puis relance) ou ton IDE. Les 21 outils KARUKIA sont désormais disponibles.

> Au premier lancement, `npx` télécharge le package automatiquement (~175 Ko). Les lancements suivants utilisent la version en cache.

### Étape 3 — Configure ton projet

Dis à ton IA :

> « karukia install »

KARUKIA scanne ton projet, détecte ton stack et génère les fichiers de configuration (scope sécurité, CLAUDE.md, structure mémoire).

### Étape 4 — Commence à travailler

Décris simplement ce que tu veux en langage naturel :

> « karukia: ajoute l'authentification utilisateur »
> « karukia: audite la sécurité »
> « karukia: lance un pentest »

L'orchestrateur (`auto`) analyse ta demande et route vers les bons spécialistes automatiquement.

> **Conseil :** Il te suffit de deux commandes : `karukia install` (une fois) puis `karukia` + ta demande (toujours). Pour un contrôle direct, appelle un skill par son nom : « karukia neo », « karukia viper », « karukia jeffrey ». Tape « karukia start » à tout moment pour un guide complet.

### Où placer la config

| Client | Fichier | Portée |
|--------|---------|--------|
| **Claude Code CLI** | `.mcp.json` à la racine du projet | Ce projet uniquement |
| **Claude Code CLI** | `~/.claude.json` (dossier home) | Tous tes projets |
| **Claude Desktop** | `claude_desktop_config.json` | Global |
| **Cursor** | `.cursor/mcp.json` à la racine | Ce projet uniquement |
| **Windsurf** | Panneau MCP settings | Global |

---

## Installation globale (optionnel)

Si tu veux KARUKIA disponible dans tous tes projets sans ajouter `.mcp.json` à chaque fois :

```bash
npm install -g karukia-mcp
```

Puis ajoute à ta config IA globale (`~/.claude.json` pour Claude Code) :

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

## 21 Outils

### Essentiels (commence ici)

| Outil | Description |
|-------|-------------|
| `install` | **[PREMIÈRE ÉTAPE]** Configure KARUKIA pour ton projet — à lancer une seule fois |
| `auto` | **[OUTIL PRINCIPAL]** Décris ce que tu veux — KARUKIA route vers les bons skills |
| `start` | Guide de démarrage — explique tous les skills à 3 niveaux progressifs |

### 11 Skills (Personas IA)

Chaque skill retourne un prompt complet qui transforme ton IA en spécialiste.

| Outil | Persona | Ce qu'il fait |
|-------|---------|---------------|
| `neo` | Auditeur Sécurité | Audit défensif contre 6 frameworks (OWASP, HDS, ISO 27001, SOC 2, PCI-DSS, HIPAA) |
| `viper` | Brigade Pentest | Tests offensifs avec 16 agents, scoring CVSS v4, mapping MITRE ATT&CK |
| `jeffrey` | Architecte Full-Stack | Implémentation de features avec TDD et validation sécurité |
| `opo` | Validateur Qualité | Qualité web contre les 245 règles Opquast |
| `audit_opquast` | Auditeur Qualité | Audit Opquast complet avec 14 checklists thématiques |
| `ebios_rm_audit` | Analyste Risques | Méthodologie EBIOS Risk Manager (ANSSI) — analyse formelle |
| `security_hardening` | Planificateur Durcissement | Chantiers d'amélioration sécurité |
| `terraform_update` | Spécialiste IaC | Automatisation Terraform pour KMS, GCS, IAM |
| `doc_refactor` | Auditeur Doc | Audit de précision documentation vs code réel |

### 5 Utilitaires

| Outil | Description |
|-------|-------------|
| `list_checklists` | Parcourir les 24 checklists par catégorie |
| `get_checklist` | Récupérer le contenu complet d'une checklist |
| `search_rules` | Chercher parmi les 935+ points par mot-clé et sévérité |
| `suggest_checklists` | Décrire ton projet — obtenir un plan d'audit priorisé |
| `generate_report` | Compiler les résultats d'audit en rapport Markdown scoré |

### 4 Mémoire & Config

| Outil | Description |
|-------|-------------|
| `init_memory` | Initialiser la structure mémoire KARUKIA dans un projet |
| `get_session_template` | Obtenir des templates de session pré-remplis pour chaque skill |
| `get_config_template` | Obtenir des templates de configuration (scope sécurité, CLAUDE.md) |
| `get_shared` | Accéder aux composants méthodologiques partagés |

---

## 24 Checklists

### Sécurité défensive (Neo) — 6 checklists, 445 contrôles

| Checklist | Points | Périmètre |
|-----------|--------|-----------|
| **OWASP Security Baseline** | 62 | Toute application web |
| **HDS 2.0** | 52 | Données de santé, France |
| **ISO 27001:2022** | 93 | SMSI entreprise |
| **SOC 2 Type II** | 74 | SaaS, marché US |
| **PCI-DSS v4.0** | 97 | Traitement de paiements |
| **HIPAA** | 67 | Données de santé, US |

### Qualité web (Opquast) — 14 checklists, 245 règles

Contenus, données personnelles, e-commerce, formulaires, identification, images, internationalisation, liens, navigation, newsletter, présentation, sécurité UX, serveur & performances, structure et code.

Basé sur [Opquast](https://www.opquast.com/) — le référentiel français de qualité web utilisé par plus de 15 000 professionnels.

### Sécurité offensive (Viper) — 4 checklists, 245+ tests

| Checklist | Tests | Périmètre |
|-----------|-------|-----------|
| **OWASP WSTG v5** | 100 | Pentest web |
| **Cloud Platform** | 80+ | Firebase, GCP, AWS, Azure |
| **Healthcare** | 50+ | PHI, chiffrement, données médicales |
| **Scénarios d'attaque** | 15+ | Templates PTES, MITRE ATT&CK |

---

## Exemples d'utilisation

### Audit sécurité complet

> « Audite la sécurité de mon projet »

Ton IA appelle `neo` — devient l'auditeur Neo — suit la méthodologie — produit des findings structurés avec sévérité, références fichier:ligne et étapes de remédiation.

### Construire une feature avec garde-fous

> « karukia jeffrey: implémente l'authentification utilisateur »

Ton IA appelle `jeffrey` — devient l'architecte Jeffrey — implémente avec TDD, puis enchaîne avec Neo pour la validation sécurité (boucle de rejet : si Neo rejette, Jeffrey corrige, max 3 itérations).

### Pentester son app

> « karukia viper »

Ton IA appelle `viper` — déploie la méthodologie Brigade avec 16 agents spécialisés sur les phases Reco, Analyse de surface et Exploitation.

### Tout orchestrer

> « karukia: ajoute un bouton logout et audite la sécurité »

Ton IA appelle `auto` — analyse la demande — route vers les bons skills — gère la chaîne.

---

## Pour les secteurs réglementés

KARUKIA est conçu pour les équipes qui développent dans des environnements soumis à certification : HDS 2.0, ISO 27001, SOC 2, PCI-DSS, HIPAA, RGPD.

**KARUKIA ne remplace pas l'auditeur humain. Il prépare le dossier** : findings tracés, mappés par framework, avec référence fichier:ligne. Quand l'auditeur arrive, les preuves existent déjà.

Voir le [Livre Blanc](./LIVRE-BLANC.md) pour une analyse détaillée.

---

## Documentation

- [Livre Blanc (Français)](./LIVRE-BLANC.md) — Document technique détaillé : architecture, méthodologie, cas d'usage
- [Whitepaper (English)](./WHITEPAPER.md) — Technical deep-dive: architecture, methodology, use cases

---

## Cloud / Équipe

KARUKIA tourne en local par défaut (stdio via `npx`). Gratuit, zéro infrastructure.

**Pour les équipes** — un serveur KARUKIA géré (liste d'attente) : connexion centralisée pour toute l'équipe, clé API unique, audit trail partagé, cohérence des checklists entre développeurs.

→ **contact@karukia.com** pour rejoindre la liste d'attente.

---

## À propos

KARUKIA est développé par **[KARUK IA Solutions](https://karukia.com)**, studio SaaS B2B spécialisé dans les domaines réglementés (santé, finance, pharma), basé en Guadeloupe. 🇬🇵

Construit à partir de l'expérience directe de la certification HDS 2.0 / ISO 27001 dans le secteur de la santé en France. La méthodologie a été ouverte pour partager ce qu'un vrai processus de certification exige — pas de la théorie.

> *Made in Guadeloupe — L'IA ne remplace pas l'expert, elle le libère.*

---

## Licence

KARUKIA MCP est gratuit pour usage personnel, éducatif et professionnel interne.

**L'usage commercial ou la revente nécessitent une autorisation écrite.** Contact : contact@karukia.com

Voir [LICENSE](./LICENSE) pour les conditions complètes.
