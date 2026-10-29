# Ce que je ferais avec plus de temps

Suite du [PRD](../SPEC.md#partie-1--prd). Par ordre de risque, avec ce que chaque étape tranche.

1. **Vérifier en pharmacie** (avec le support et une pharmacie partenaire) comment la couverture est contrôlée et quand une inscription y apparaît. Tranche la question 2 du PRD (délai d'activation) : si la pharmacie exige une carte physique, « Couvert » dans le portail n'évite pas le refus d'un nouveau salarié.
2. **Tester le prototype avec 3 RH**, dont la DRH de TransSahel et une entreprise de moins de 20 salariés : comprennent-elles les statuts sans aide ? Ont-elles le nom exact et la date de naissance au moment d'inscrire ? Tranche les libellés et les champs obligatoires.
3. **Comprendre pourquoi les salariés passent par leur RH** alors qu'ils envoient leurs demandes sur WhatsApp : relire 20 conversations avec le support (délai de réponse, motif de refus envoyé ou non), appeler 5 salariés, et demander à chaque appel RH « le salarié vous a-t-il déjà écrit sur WhatsApp ? ». Tranche la question 3 du PRD (améliorer WhatsApp ou créer un espace salarié).
4. **Comprendre la facture** : la faire expliquer par la DRH de TransSahel et classer 20 appels factures avec le support. Tranche le contenu de la page Factures.
5. **Faire valider le tableau de visibilité par la juriste** : motif de blocage visible par la RH, collecte du téléphone, seuil de 20 salariés (entreprise entière ou par site).
6. **Côté build** : traduire le tableau de visibilité en tests automatiques (aucune route du rôle RH ne renvoie de donnée de remboursement), puis ajouter fiche salarié, sortie et factures au prototype.
