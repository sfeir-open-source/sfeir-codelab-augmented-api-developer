# Lab — Movie Library API

> **Durée estimée :** 2h  
> **Agent recommandé :** [OpenCode](https://opencode.ai/) — mais tout agent compatible Agent Skills fonctionne (Claude Code, Cursor, GitHub Copilot, Gemini CLI…)

---

## Objectif

Construire une REST API de gestion de films, **en pilotant un agent de code**.  
L'enjeu n'est pas d'écrire du code manuellement : c'est de diriger l'agent efficacement, en tirant parti de **skills spécialisés** à chaque étape du projet.

---

## Prérequis

- Node.js ≥ 20 (pour `npx skills`)
- Un agent de code installé et configuré
- Un terminal

---

## Étape 0 — Installer les skills

Les skills sont des modules de connaissance que l'agent charge à la demande. Installe les quatre skills du lab :

```bash
# Skill de design : aide à valider les décisions d'architecture
npx skills add mattpocock/skills
Sélectionner **grill-me** ou **grill-me-with-docs** skill
Sélectionner un agent additionnel si celui qui vous utilisez n'est pas dans la liste des agents universels

# Skill de documentation : structure la doc selon Diataxis
npx skills add rlespinasse/agent-skills

# Skill de revue de code : analyse qualité multi-dimensionnelle
npx skills add addyosmani/agent-skills
```

> Ces commandes installent les skills dans ton projet. Ton agent les découvre automatiquement au démarrage.

---

## Contrat API

Voici le contrat que ton agent doit implémenter. **Ne code rien manuellement** — donne ce contrat à l'agent.

### Entité `Movie`

| Champ | Type | Description |
|-------|------|-------------|
| `id` | string (UUID) | Identifiant unique |
| `title` | string | Titre du film |
| `year` | number | Année de sortie |
| `genre` | string | Genre (action, drama, comedy…) |
| `director` | string | Réalisateur |
| `rating` | number (0–5) | Note |

### Endpoints

| Méthode | Route | Description |
|---------|-------|-------------|
| `GET` | `/movies` | Liste tous les films. Filtres : `?genre=` `?year=` |
| `GET` | `/movies/:id` | Détail d'un film |
| `POST` | `/movies` | Crée un film |
| `PUT` | `/movies/:id` | Met à jour un film |
| `DELETE` | `/movies/:id` | Supprime un film |
| `GET` | `/movies/:id/similar` | Films du même genre |

### Règles

- Stockage **en mémoire** (pas de base de données pour l'instant)
- `GET /movies` sans filtre retourne tous les films
- `GET /movies/:id/similar` exclut le film source de la réponse
- Codes HTTP standards : `200`, `201`, `204`, `404`, `400`
- Format d'erreur uniforme :
  ```json
  { "error": "message d'erreur" }
  ```

---

## Étape 1 — Design avec `grill-me`

Avant de coder, utilise le skill **grill-me** pour valider tes choix de design avec l'agent.

**Prompt de démarrage :**
```
grill-me — je veux construire une REST API de gestion de films en [ton stack].
Le stockage est en mémoire. L'agent doit m'interroger sur mes choix d'architecture
avant de commencer à coder.
```

L'agent va te poser des questions une par une sur : le stack, la structure du projet, la gestion des erreurs, les conventions de nommage, etc. Réponds-y — c'est lui qui code ensuite.

---

## Étape 2 — Construction de l'API

Une fois le design validé, donne le contrat à l'agent :

**Prompt :**
```
Implémente l'API Movie Library selon le contrat suivant.
Stack choisi : [ton stack].
Stockage en mémoire.
[Colle ici le contrat API de ce README]
```

L'agent bootstrap le projet, crée les fichiers, implémente tous les endpoints.  
Laisse-le faire — interviens seulement si tu veux orienter une décision.

> **Conseil :** observe comment l'agent structure le projet. Tu peux lui demander d'expliquer ses choix.

---

## Étape 3 — Revue de code avec `code-review-and-quality`

L'API est construite. Utilise le skill de revue pour analyser la qualité.

**Prompt :**
```
code-review-and-quality — fais une revue complète de mon API Movie Library.
Analyse les cinq axes : correction, lisibilité, architecture, sécurité, performance.
Identifie les problèmes critiques et importants. Propose des corrections.
```

Applique les corrections suggérées en continuant à dialoguer avec l'agent.

---

## Étape 4 — Sécurité avec `security-and-hardening`

**Prompt :**
```
security-and-hardening — analyse mon API et applique les bonnes pratiques
de sécurité : validation des entrées, headers HTTP, gestion des erreurs,
protection contre les injections. Applique les corrections non-négociables.
```

---

## Étape 5 — Documentation avec `diataxis`

**Prompt :**
```
diataxis — génère la documentation de mon API Movie Library en suivant
le framework Diataxis : tutorial, how-to, explanation, reference.
```

---

## Félicitations

Tu as construit une API REST complète en pilotant un agent à chaque étape du cycle de développement : design → implémentation → revue → sécurité → documentation.

---

## Bonus — Aller plus loin

Ces défis sont indépendants, tu peux les faire dans n'importe quel ordre.

### 🗄️ Base de données SQLite

Remplace le stockage en mémoire par SQLite, avec un ORM qui permet de changer de base facilement (Prisma, TypeORM, SQLAlchemy, GORM…).

**Prompt :**
```
Migre le stockage en mémoire vers SQLite en utilisant [Prisma / TypeORM / SQLAlchemy].
Utilise le pattern Repository pour que la couche de données soit interchangeable.
```

### 🔐 Authentification

Ajoute une couche d'authentification JWT : inscription, connexion, routes protégées.

**Prompt :**
```
Ajoute une authentification JWT à l'API.
Endpoints : POST /auth/register et POST /auth/login.
Les routes POST, PUT, DELETE /movies doivent nécessiter un token valide.
```

### 🐳 Containerisation

**Prompt :**
```
Crée un Dockerfile multi-stage optimisé pour l'API.
Ajoute un docker-compose.yml pour le développement local.
L'image finale doit être la plus légère possible.
```

### ⎈ Helm Chart

**Prompt :**
```
Crée un Helm chart pour déployer l'API sur Kubernetes.
Inclus : Deployment, Service, ConfigMap pour les variables d'environnement,
et un Ingress. Paramétrise le nombre de replicas et l'image.
```
