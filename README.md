# skillgraph — le skill qui trouve les skills

Un **Claude Agent Skill** qui interroge [/skillgraph](https://skillgraph.info) —
le moteur de recherche + carte de milliers de skills publics (`SKILL.md`) — depuis
ton agent (Claude Code, etc.), via un **serveur MCP distant**. Tu tapes
`/skillgraph <requête>`, l'agent te propose le meilleur skill avec sa commande
d'installation et son niveau de risque.

## Installation

### 1. Ajouter le serveur MCP (une fois)
```bash
claude mcp add --transport http skillgraph https://skillgraph.info/mcp/
```
Ou copie le fichier [`.mcp.json`](./.mcp.json) à la racine de ton projet
(configuration MCP au niveau projet). Aucune authentification requise.

### 2. Installer le skill
Copie le dossier dans tes skills Claude :
```bash
npx degit elolozone/skillgraph-skill ~/.claude/skills/skillgraph
```

## Utilisation
```
/skillgraph extraire les tableaux d'un PDF
/skillgraph un skill pour la comptabilité française
```
L'agent appelle l'outil MCP `search_skills`, classe les résultats par pertinence,
et te montre les meilleurs avec `security_level` (niveau de risque) et `review_flags`.

## Outils MCP exposés
| Outil | Rôle |
|------|------|
| `search_skills(query, limit, france)` | Recherche sémantique + mots-clés (réécriture LLM des requêtes en langage naturel). `france=true` privilégie les skills adaptés au contexte français. |
| `get_skill(id)` | Fiche détaillée d'un skill : résumé, dépôt, licence, note de sécurité, install. |

## Sécurité
Ce skill **ne fait qu'aider à trouver**. Il **n'installe rien automatiquement** : un
skill est du texte que l'agent exécutera. Relis toujours le `SKILL.md` source (et son
code) avant d'installer, et n'installe qu'avec ton accord explicite. Les résumés,
scores et niveaux de risque fournis sont indicatifs.

## Comment ça marche (côté serveur)
Le serveur MCP est co-hébergé avec /skillgraph : la recherche est un pipeline RAG
(BM25 + KNN dense sur `sqlite-vec` → fusion RRF → reranker cross-encoder), avec
réécriture LLM des requêtes en langage naturel. Voir la page « Comment ça marche » du
site.
