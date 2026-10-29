# Portail RH · Mutuelle Sabali

Exercice Product Builder : un PRD, une spec pour agent de code et un prototype cliquable de deux écrans pour le portail RH d'une mutuelle santé à Dakar.

> Cas entièrement fictif : la mutuelle, l'entreprise cliente et les personnes citées sont inventées.

## Les livrables

| Livrable | Où le trouver |
| --- | --- |
| **PRD** | [SPEC.md, partie 1](SPEC.md#partie-1--prd) : périmètre, règles de visibilité, parcours, questions aux décideurs, hypothèses |
| **Ce que je ferais avec plus de temps** | [docs/plus-de-temps.md](docs/plus-de-temps.md) |
| **Prototype** | Liste des salariés et inscription d'un salarié, sur Vercel : lien ajouté après le déploiement |

## La méthode

Le PRD est écrit pour se suffire à lui-même : une personne comme un agent le comprend sans la consigne de l'exercice. Il sert tel quel de spec à Claude Code, qui construit le prototype. Le dépôt suit les [bonnes pratiques de Claude Code](https://code.claude.com/docs/en/best-practices) et sépare trois choses :

- [`CLAUDE.md`](CLAUDE.md) : les règles du projet, chargées à chaque session. Court, et seulement ce qui ne change pas : commandes, stack, règles de confidentialité.
- [`SPEC.md`](SPEC.md) : ce qu'on construit. Le PRD, la spec du prototype, ce qui est hors périmètre et une vérification de bout en bout.
- [`premier-message.md`](premier-message.md) : la tâche du moment. Un plan d'abord, puis la construction, les preuves (build, captures) et le déploiement.

Les arbitrages produit sont les miens ; l'IA m'a fait gagner du temps sur la rédaction et le code.

## Reproduire

```bash
git clone https://github.com/ThomasRNathan/sabali-portail-rh.git
cd sabali-portail-rh
claude --permission-mode plan
```

Puis coller le contenu de `premier-message.md`.

---

Thomas Nathan · [thomasnathan.com](https://www.thomasnathan.com)
