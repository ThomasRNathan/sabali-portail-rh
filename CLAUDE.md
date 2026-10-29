# Portail RH Mutuelle Sabali · prototype

Prototype cliquable de deux écrans (liste des salariés, inscription d'un salarié), sans backend, déployé sur Vercel. La spec complète, PRD compris, est dans `SPEC.md` : c'est la source de vérité, lis-la avant toute modification d'interface.

## Commandes

- `npm run dev` : lancer en local
- `npm run build` : doit passer avant chaque déploiement
- `npm run screenshots` : captures Playwright dans `screenshots/`
- `npx vercel --prod --yes` : déployer en production (projet `sabali-portail-rh`). Si la CLI n'est pas connectée, demande-moi de lancer `npx vercel login`.

## Stack

- Next.js (App Router), TypeScript, Tailwind CSS.
- État React côté client ; réseau simulé dans `lib/api.ts` (600 ms de latence, échec à la demande).
- Composants remplaçables par un design system : `DemoBar`, `SummaryStrip`, `EmployeeTable`, `StatusBadge`, `EmptyState`, `ErrorState`, `EmployeeForm`, `Field`, `HelpBox`, `Toast`. Couleurs et espacements dans la configuration Tailwind.

## Règles

- IMPORTANT : aucune donnée médicale ni information de remboursement par salarié, nulle part : interface, données fictives, code, commentaires.
- Seuls les deux écrans existent : pas de liens morts vers la sortie, les factures ou l'import Excel.
- Interface en français ; dates au format JJ/MM/AAAA ; téléphones au format +221 ; formuler les dates avec « couverture » pour éviter les accords.
- Un statut se lit toujours en texte, jamais par la couleur seule.
- Si `SPEC.md` ne tranche pas un point : option la plus simple, notée dans le README, section « Choix faits ».
