# Portail RH · Mutuelle Sabali

Exercice Product Builder : un PRD, une spec pour agent de code et un prototype cliquable de deux écrans pour le portail RH d'une mutuelle santé à Dakar.

> Cas entièrement fictif : la mutuelle, l'entreprise cliente et les personnes citées sont inventées.

## Les livrables

| Livrable | Où le trouver |
| --- | --- |
| **Prototype** | [sabali-portail-rh.vercel.app](https://sabali-portail-rh.vercel.app) : liste des salariés et inscription d'un salarié |
| **PRD** | [SPEC.md, partie 1](SPEC.md#partie-1--prd) : périmètre, règles de visibilité, parcours, questions aux décideurs, hypothèses |
| **PRD en PDF** | [PRD-portail-RH-Sabali.pdf](PRD-portail-RH-Sabali.pdf) : le même PRD, sur une page |
| **Master prompt** | [premier-message.md](premier-message.md) : le prompt qui a lancé le build |
| **Ce que je ferais avec plus de temps** | [docs/plus-de-temps.md](docs/plus-de-temps.md) |

## La méthode

Le PRD est écrit pour se suffire à lui-même : une personne comme un agent le comprend sans la consigne de l'exercice. Il sert tel quel de spec à Claude Code, qui construit le prototype. Le dépôt suit les [bonnes pratiques de Claude Code](https://code.claude.com/docs/en/best-practices) et sépare trois choses :

- [`SPEC.md`](SPEC.md) : ce qu'on construit. Le PRD, puis la spec du prototype, ce qui est hors périmètre et une vérification de bout en bout.
- [`premier-message.md`](premier-message.md) : le master prompt, envoyé une seule fois pour lancer le build. Un plan d'abord, puis la construction, les preuves (build, captures) et le déploiement.
- [`CLAUDE.md`](CLAUDE.md) : les règles que Claude Code relit à chaque session. Court, et seulement ce qui ne change pas : commandes, stack, règles de confidentialité.

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
