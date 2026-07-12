# skillgraph — le skill qui trouve les skills

Un **plugin Claude Code** qui interroge [/skillgraph](https://skillgraph.info) — le
moteur de recherche + carte de milliers de skills publics (`SKILL.md`) — depuis ton
agent, via un **serveur MCP distant**. Tu tapes `/skillgraph` suivi de ta requête,
l'agent te propose le meilleur skill avec sa commande d'installation et son niveau de
risque.

Ce dépôt est **sa propre marketplace de plugin** : le plugin embarque le skill **et**
configure automatiquement le serveur MCP.

## Installation (recommandé — plugin)

Dans Claude Code :
```
/plugin marketplace add elolozone/skillgraph-skill
/plugin install skillgraph@skillgraph
```
Ou en ligne de commande :
```bash
claude plugin marketplace add elolozone/skillgraph-skill
claude plugin install skillgraph@skillgraph
```
Ou via l'interface **« Ajouter une marketplace »** : colle `elolozone/skillgraph-skill`,
clique **Synchro**, puis installe le plugin **skillgraph**.

→ Cela installe le skill `/skillgraph` **et** configure le serveur MCP `skillgraph`
(`https://skillgraph.info/mcp/`, sans authentification).

## Utilisation
```
/skillgraph extraire les tableaux d'un PDF
/skillgraph un skill pour la comptabilité française
```
L'agent appelle l'outil MCP `search_skills`, classe les résultats par pertinence, et
te montre les meilleurs avec `security_level` (niveau de risque) et `review_flags`.

## Contenu du plugin
```
.claude-plugin/
  marketplace.json     # ce dépôt est sa propre marketplace
  plugin.json          # manifeste du plugin
skills/skillgraph/
  SKILL.md             # le skill /skillgraph
.mcp.json              # serveur MCP distant (Streamable HTTP)
```

## Outils MCP exposés
| Outil | Rôle |
|------|------|
| `search_skills(query, limit, france)` | Recherche sémantique + mots-clés (réécriture LLM des requêtes en langage naturel). `france=true` privilégie les skills adaptés au contexte français. |
| `get_skill(id)` | Fiche détaillée : résumé, dépôt, licence, note de sécurité, install. |

## Sécurité
Ce plugin **ne fait qu'aider à trouver** ; il **n'installe rien automatiquement**. Un
skill est du texte que l'agent exécutera : relis toujours le `SKILL.md` source (et son
code) avant d'installer, et n'installe qu'avec ton accord explicite. Les résumés,
scores et niveaux de risque sont indicatifs.
