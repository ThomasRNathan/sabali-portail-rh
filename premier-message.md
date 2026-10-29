Lis @SPEC.md, puis propose un plan avant d'écrire du code : structure des fichiers, modèle de données, composants, ordre de construction. Attends mon accord.

Une fois le plan validé :

1. Construis les deux écrans, tous les états et la barre de démo. Le dépôt contient déjà des fichiers : si `create-next-app` refuse un dossier non vide, génère le projet dans un dossier temporaire puis déplace-le ici.
2. Fais passer la vérification de bout en bout (SPEC.md, partie 4) et corrige jusqu'à ce qu'elle passe. Montre-moi les preuves : sortie du build, captures.
3. Commite par étapes et pousse sur `origin/main`, puis déploie avec `npx vercel --prod --yes` et donne-moi l'URL de production. Rappelle-moi de l'ouvrir en navigation privée : si Vercel demande une connexion, il faut désactiver la protection des déploiements dans les réglages du projet.
4. Dans le README, remplace seulement « lien ajouté après le déploiement » par l'URL de production, puis ajoute à la fin deux sections : « Lancer en local » (commandes et scénarios de démo) et « Choix faits ». Ne touche pas au reste.
