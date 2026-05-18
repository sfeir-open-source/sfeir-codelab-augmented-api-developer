---
theme: default
background: '#F9F9F9'
title: Comment fonctionne un Agent IA ?
info: Comprendre les interactions User · Agent · LLM · Tools
class: text-left
drawings:
  persist: false
transition: slide-left
fonts:
  sans: Epilogue
  weights: '400,700,900'
  provider: google
duration: 30min
---

<div class="sfeir-label">SFEIR · AI ONLY</div>

# Comment fonctionne<br>un **Agent IA** ?

<span class="gold">Interactions User · Agent · LLM · Tools</span>

---
layout: default
---

<div class="sfeir-label">AGENDA</div>

# Au programme

<v-clicks>

1. De la complétion au Chat — le LLM seul
2. Chat LLM vs Agent — la différence fondamentale
3. Les 3 briques — LLM · Tools · Memory
4. L'architecture d'un agent — qui parle à qui ?
5. La boucle ReAct — Think → Act → Observe
6. Le rôle du développeur — ce qui change
7. Takeaways

</v-clicks>

---
layout: center
class: dark-slide text-center
---

# Comment on est<br><span class="gold">arrivé là ?</span>

---
layout: default
class: dark-slide
---

<div class="sfeir-label">ÉVOLUTION</div>

# De l'autocomplete<br>à l'agent autonome

<v-clicks>

<div class="mt-6 space-y-4">

**2021 — Autocomplete** · Copilot suggère la ligne suivante

**2022–23 — Chat** · Tu poses une question, l'IA répond

**2024 — Agent mode** · Tu décris une tâche, l'IA écrit et exécute du code

**2025+ — Agent autonome** · Tu assignes un ticket, l'IA ouvre une PR

</div>

</v-clicks>

<v-click>

> Chaque étape est un **saut qualitatif**. Autocomplete = éditeur. Chat = collègue. Agent = junior dev. Agent autonome = membre d'équipe async.

</v-click>

---
layout: two-cols-header
---

<div class="sfeir-label">COMPARAISON</div>

# Chat LLM vs Agent IA

::left::

<div class="sfeir-card mt-4">

### Chat LLM

<div class="flow">
  <div class="flow-box">Tu poses une question</div>
  <div class="flow-arrow">↓</div>
  <div class="flow-box">LLM répond</div>
</div>

- Lit uniquement la conversation
- Ne lit pas tes fichiers
- Ne lance pas de commandes
- Ne peut pas ouvrir une PR

**Passif — répond, ne fait pas**

</div>

::right::

<div class="sfeir-card mt-4">

### Agent IA

<div class="flow">
  <div class="flow-box">Tu définis une tâche</div>
  <div class="flow-arrow">↓</div>
  <div class="flow-box agent">Agent lit, agit, itère, livre</div>
</div>

- Lit les fichiers du projet
- Lance les tests et le lint
- Utilise des outils : git, APIs
- Ouvre une pull request

**Actif — exécute, itère, livre**

</div>

---
layout: center
class: dark-slide text-center
---

# Même modèle<br>sous-jacent.

# <span class="gold">Surface de capacités</span><br>fondamentalement différente.

---
layout: center
class: dark-slide text-center
---

# Les 3 briques<br>d'un Agent

# <span class="gold">LLM · Tools · Memory</span>

---
layout: default
---

<div class="sfeir-label">BRIQUE 1</div>

# LLM — Le cerveau

- Le modèle de langage qui **raisonne** sur le problème
- Décide quelles actions prendre
- Interprète les résultats pour déterminer les prochaines étapes
- Génère le code ou la réponse finale

<div class="llm-flow">
  <div class="flow-row">
    <div class="flow-box center-box">Prompt entrant</div>
  </div>
  <div class="flow-row"><div class="flow-arrow">↓</div></div>
  <div class="flow-row">
    <div class="flow-box center-box agent">Raisonnement</div>
  </div>
  <div class="flow-branches">
    <div class="flow-branch">
      <div class="flow-arrow branch-arrow">↙</div>
      <div class="flow-box">Quelle action<br>prendre ?</div>
    </div>
    <div class="flow-branch">
      <div class="flow-arrow">↓</div>
      <div class="flow-box">Quel outil<br>appeler ?</div>
    </div>
    <div class="flow-branch">
      <div class="flow-arrow branch-arrow">↘</div>
      <div class="flow-box">Tâche<br>terminée ?</div>
    </div>
  </div>
</div>

---
layout: default
---

<div class="sfeir-label">BRIQUE 2</div>

# Tools — Les bras

Le LLM ne peut interagir avec le monde **qu'à travers des outils**.

<v-click>

```python
read_file("src/utils/date.ts")
write_file("src/utils/date.ts", "...")
run_command("npm test")
search_repo("paginate")
create_pull_request(title="fix: off-by-one in paginate")
```

</v-click>

<v-clicks>

**Exemples concrets :**
- Lire / écrire des fichiers
- Exécuter des tests ou du lint
- Chercher dans le dépôt
- Appeler une API externe — Jira, Confluence, Linear…
- Ouvrir ou commenter une PR

</v-clicks>

---
layout: default
---

<div class="sfeir-label">BRIQUE 3</div>

# Memory — Le contexte

| Type | Description | Exemple |
|------|-------------|---------|
| **In-context** | Tout ce qui est dans la fenêtre de contexte | Historique de conversation |
| **External** | Stockage persistant hors LLM | Fichiers, base de données |
| **Semantic** | Récupération par similarité | RAG sur le codebase |

<v-click>

> La mémoire détermine ce que l'agent **sait** à chaque étape.  
> Sans mémoire persistante, chaque session repart de zéro.

</v-click>

---
layout: center
class: dark-slide text-center
---

# L'architecture<br>d'un <span class="gold">Agent</span>

---
layout: default
---

<div class="sfeir-label">ARCHITECTURE</div>

# Qui parle à qui ?

<svg class="diagram" viewBox="0 0 880 300" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <marker id="a12" markerWidth="7" markerHeight="5" refX="7" refY="2.5" orient="auto">
      <polygon points="0 0,7 2.5,0 5" fill="#845400"/>
    </marker>
  </defs>
  <!-- System Prompt -->
  <rect x="330" y="6" width="220" height="44" rx="2" fill="#F1F1F1" stroke="#d0c8be" stroke-width="1.5"/>
  <text x="440" y="24" text-anchor="middle" style="font-size:9px;font-family:Space Grotesk,sans-serif;font-weight:600;letter-spacing:1.5px" fill="#845400">SYSTEM PROMPT</text>
  <text x="440" y="42" text-anchor="middle" style="font-size:10px;font-family:Epilogue,sans-serif" fill="#514535">Instructions &amp; contraintes</text>
  <!-- User -->
  <rect x="20" y="124" width="110" height="44" rx="2" fill="#F1F1F1" stroke="#d0c8be" stroke-width="1.5"/>
  <text x="75" y="151" text-anchor="middle" style="font-size:14px;font-family:Epilogue,sans-serif;font-weight:700" fill="#1B1B1B">User</text>
  <!-- Agent -->
  <rect x="330" y="124" width="220" height="44" rx="2" fill="#845400"/>
  <text x="440" y="151" text-anchor="middle" style="font-size:14px;font-family:Epilogue,sans-serif;font-weight:700" fill="white">Agent</text>
  <!-- LLM -->
  <rect x="710" y="124" width="120" height="44" rx="2" fill="#F1F1F1" stroke="#d0c8be" stroke-width="1.5"/>
  <text x="770" y="151" text-anchor="middle" style="font-size:14px;font-family:Epilogue,sans-serif;font-weight:700" fill="#1B1B1B">LLM</text>
  <!-- Tools -->
  <rect x="200" y="242" width="120" height="44" rx="2" fill="#F1F1F1" stroke="#d0c8be" stroke-width="1.5"/>
  <text x="260" y="269" text-anchor="middle" style="font-size:14px;font-family:Epilogue,sans-serif;font-weight:700" fill="#1B1B1B">Tools</text>
  <!-- Memory -->
  <rect x="550" y="242" width="120" height="44" rx="2" fill="#F1F1F1" stroke="#d0c8be" stroke-width="1.5"/>
  <text x="610" y="269" text-anchor="middle" style="font-size:14px;font-family:Epilogue,sans-serif;font-weight:700" fill="#1B1B1B">Memory</text>
  <!-- ② System Prompt → Agent -->
  <line x1="440" y1="50" x2="440" y2="122" stroke="#E5A040" stroke-width="1.5" marker-end="url(#a12)"/>
  <text x="448" y="92" style="font-size:9px;font-family:Space Grotesk,sans-serif" fill="#E5A040">② Instructions</text>
  <!-- ① User → Agent -->
  <line x1="132" y1="143" x2="328" y2="143" stroke="#845400" stroke-width="1.5" marker-end="url(#a12)"/>
  <text x="165" y="136" style="font-size:9px;font-family:Space Grotesk,sans-serif" fill="#E5A040">① Prompt</text>
  <!-- ⑥ Agent → User -->
  <line x1="330" y1="157" x2="134" y2="157" stroke="#845400" stroke-width="1.5" stroke-dasharray="5,3" marker-end="url(#a12)"/>
  <text x="165" y="171" style="font-size:9px;font-family:Space Grotesk,sans-serif" fill="#E5A040">⑥ Response</text>
  <!-- ③ Agent ↔ LLM -->
  <line x1="552" y1="143" x2="708" y2="143" stroke="#845400" stroke-width="1.5" marker-end="url(#a12)"/>
  <line x1="708" y1="157" x2="552" y2="157" stroke="#845400" stroke-width="1.5" marker-end="url(#a12)"/>
  <text x="590" y="137" style="font-size:9px;font-family:Space Grotesk,sans-serif" fill="#E5A040">③ Reasoning</text>
  <!-- ④ Agent → Tools -->
  <line x1="390" y1="168" x2="285" y2="240" stroke="#E5A040" stroke-width="1.5" marker-end="url(#a12)"/>
  <text x="296" y="213" style="font-size:9px;font-family:Space Grotesk,sans-serif" fill="#E5A040">④ Actions</text>
  <!-- ⑤ Agent ↔ Memory -->
  <line x1="492" y1="168" x2="578" y2="240" stroke="#E5A040" stroke-width="1.5" marker-end="url(#a12)"/>
  <line x1="568" y1="240" x2="482" y2="168" stroke="#E5A040" stroke-width="1.5" stroke-dasharray="5,3" marker-end="url(#a12)"/>
  <text x="524" y="212" style="font-size:9px;font-family:Space Grotesk,sans-serif" fill="#E5A040">⑤ Store / Retrieve</text>
</svg>

<v-click>

> Les étapes 3, 4 et 5 se **répètent** de nombreuses fois avant l'étape 6.

</v-click>

---
layout: default
---

<div class="sfeir-label">LES 6 ÉTAPES</div>

# Le flux d'exécution

<v-clicks>

1. **Prompt** — L'utilisateur envoie une tâche à l'Agent
2. **Instructions** — Le System Prompt cadre le comportement de l'agent
3. **Planning / Reasoning** — L'Agent ↔ LLM raisonnent ensemble (bidirectionnel)
4. **Actions** — L'Agent appelle des outils pour agir sur le monde
5. **Store / Retrieve** — L'Agent lit et écrit dans sa mémoire
6. **Response** — L'Agent retourne le résultat à l'utilisateur

</v-clicks>

<v-click>

> Les étapes 3–5 forment une **boucle** qui peut itérer des dizaines de fois avant que l'agent considère la tâche terminée.

</v-click>

---
layout: center
class: dark-slide text-center
---

# La boucle <span class="gold">ReAct</span>

# **Re**asoning + **Act**ing

---
layout: default
---

<div class="sfeir-label">REACT LOOP</div>

# Think → Act → Observe → Repeat

```
💭 Thought:  Je dois comprendre le bug. Je vais lire le fichier.
⚙️  Action:   read_file("src/utils/paginate.ts")
👁️  Observe:  [contenu — ligne 42 utilise > au lieu de >=]

💭 Thought:  Le bug est à la ligne 42. Je corrige > en >=.
⚙️  Action:   write_file("src/utils/paginate.ts", "... >= ...")
👁️  Observe:  Fichier mis à jour.

💭 Thought:  Je dois vérifier que le fix passe les tests.
⚙️  Action:   run_command("npm test -- paginate")
👁️  Observe:  ✓ 12 tests passed

💭 Thought:  Done. Je peux ouvrir une PR.
⚙️  Action:   create_pull_request(...)
```

---
layout: default
---

<div class="sfeir-label">POURQUOI ÇA COMPTE</div>

# ReAct — Ce que ça change

<v-clicks>

- **Transparence** — le raisonnement est visible étape par étape
- **Récupération d'erreur** — l'agent s'adapte quand une action échoue
- **Points de contrôle** — chaque étape peut être inspectée et interrompue
- **Débogage** — si le résultat est faux, on trace quelle étape a dévié

</v-clicks>

<v-click>

> Ce n'est pas de la magie noire — c'est un **processus itératif structuré**.  
> Comprendre ReAct permet de mieux guider l'agent et de détecter quand il déraille.

</v-click>

---
layout: two-cols-header
---

<div class="sfeir-label">RÔLE DU DEV</div>

# Ce qui change pour<br>le développeur

::left::

## Avant

- Écrire chaque ligne d'implémentation
- Chercher et lire la doc manuellement
- Écrire le boilerplate et les tests à la main
- Interruptions constantes du flow

::right::

## Après

- Définir l'intention et les critères d'acceptance
- Relire et orienter les outputs de l'agent
- Identifier les edge cases, imposer la qualité
- Se concentrer sur l'architecture et le design

---
layout: center
class: dark-slide text-center
---

# Le rôle ne rétrécit pas.

# Il se déplace vers un<br><span class="gold">travail à plus haute<br>valeur ajoutée.</span>

---
layout: default
---

<div class="sfeir-label">EXTENSIBILITÉ</div>

# Étendre les capacités : MCP

**Model Context Protocol** = brancher l'agent sur ton écosystème d'outils

<div class="mcp-tree">
  <div class="mcp-col-left">
    <div class="mcp-agent">Agent IA</div>
  </div>
  <div class="mcp-col-right">
    <div class="mcp-row">
      <div class="mcp-arrow">→</div>
      <div class="flow-box mcp-box">Outils natifs<br><span class="mcp-sub">fichiers · tests · git</span></div>
    </div>
    <div class="mcp-row">
      <div class="mcp-arrow">→</div>
      <div class="flow-box mcp-box">Serveurs MCP
        <div class="mcp-items">
          <div>› Requêter ton API interne</div>
          <div>› Lire Confluence / Notion</div>
          <div>› Récupérer tickets Jira / Linear</div>
          <div>› Chercher dans le registre npm</div>
        </div>
      </div>
    </div>
  </div>
</div>

<v-click>

```json
{
  "mcpServers": {
    "confluence": {
      "type": "local",
      "command": "npx",
      "args": ["-y", "@decathlon/mcp-confluence"]
    }
  }
}
```

</v-click>

---
layout: default
class: dark-slide
---

<div class="sfeir-label">RÉCAPITULATIF</div>

# Takeaways

<v-clicks>

- **Agent = LLM** (raisonnement) **+ Tools** (actions) **+ Memory** (contexte)
- La **boucle ReAct** — Think → Act → Observe → repeat until done
- Le **System Prompt** cadre le comportement avant même que l'utilisateur parle
- Les **outils** sont la seule façon pour l'agent d'agir sur le monde
- La **mémoire** détermine ce que l'agent sait à chaque instant
- Le **rôle du dev** — piloter, orienter, valider · pas exécuter

</v-clicks>

---
layout: center
class: dark-slide text-center
---

# Questions ?

---
layout: end
---

# Merci
