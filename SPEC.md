# SPEC · Prototype du portail RH Mutuelle Sabali

Deux parties : le PRD (le produit, tel que partagé) et la spécification du prototype (ce qu'on construit maintenant). Le prototype couvre les parcours 1 et 2 du PRD et l'encart d'aide « remboursements ».

---

## Partie 1 · PRD

### Contexte et objectif

D'après les entretiens (DRH de TransSahel, responsable du support) et la note de la juriste : la Mutuelle Sabali couvre les salariés d'une quarantaine d'entreprises à Dakar. Les RH inscrivent leurs nouveaux salariés par un Excel envoyé par mail, ressaisi à la main en 3 à 5 jours ouvrés, et appellent le support pour tout : 60 % des appels portent sur les inscriptions et sorties, 25 % sur les factures, 15 % sur les remboursements, que les salariés suivent sur WhatsApp.

**La V1 permet à une RH d'inscrire, vérifier et sortir ses salariés et de lire sa facture sans appeler, sans jamais voir de donnée de santé.** Démo au plus gros client, TransSahel Logistique (300 salariés), dans 6 semaines ; cible : −50 % d'appels « est-il couvert ? » à 3 mois.

### 1. Périmètre

- **Pour :** les RH des entreprises clientes, chacune limitée à son entreprise ; côté mutuelle, le support valide ou renvoie chaque inscription.
- **Pas pour :** les salariés, qui suivent leurs demandes sur WhatsApp.
- **V1 :** P0 liste des salariés, inscription, sortie, aide « remboursements » ; P1 factures, import Excel.
- **Jamais :** remboursement ou consommation par salarié, classement de salariés, suspension décidée par la RH (le contrat la réserve à la mutuelle).

### 2. Règles de visibilité

La note juridique interdit à l'employeur toute donnée médicale et le suivi des demandes de remboursement. Filtrage côté serveur, partout : écrans, API, exports, emails, messages d'erreur.

| La RH voit | La RH ne voit jamais |
| --- | --- |
| Les salariés de son entreprise : nom, date de naissance, matricule, téléphone | Une demande de remboursement : existence, statut, montant, motif de refus |
| Statut de couverture, dates, motif administratif d'un blocage | Diagnostic, médicament, ordonnance, usage de la carte |
| Les factures de son entreprise | Un classement de salariés ; les données d'une autre entreprise |

Consommation agrégée : V2 seulement, pour l'entreprise entière et au moins 20 salariés, total seul, pour qu'on ne reconnaisse personne.

### 3. Données, statuts et parcours

**Salarié** : prénom(s) et nom tels que sur la pièce d'identité, date de naissance, date d'entrée (requis) ; téléphone WhatsApp (+221, 9 chiffres) et matricule (facultatifs) ; statut, motif de correction, date de couverture prévue, date de fin.

```mermaid
flowchart LR
    rh((RH inscrit)) --> attente["En attente<br/>couverture d'ici le JJ/MM"]
    attente -->|mutuelle valide| couvert["Couvert<br/>depuis le JJ/MM"]
    attente -->|mutuelle bloque| corriger["À corriger<br/>motif + bouton Corriger"]
    corriger -->|RH corrige| attente
    couvert -->|RH déclare la sortie| fin["Fin programmée<br/>jusqu'au JJ/MM"]
    fin -->|date de fin atteinte| termine["Terminé<br/>masqué par défaut"]
```

**Statuts** : `EN_ATTENTE` affiche « couvert d'ici le JJ/MM », soit la plus tardive entre la date d'entrée et l'envoi + 5 jours ouvrés (délai actuel, Q2) ; `A_CORRIGER` porte un motif posé par le support : `NOM`, `DATE_NAISSANCE` ou `AUTRE` ; puis `COUVERT`, `FIN_PROGRAMMEE`, `TERMINE`. Email à la RH au passage à `COUVERT` ou `A_CORRIGER`.

1. **Liste des salariés** : en tête « 2 à corriger · 5 en attente » ; recherche par nom ou matricule ; colonnes Nom, Matricule, Statut et date, Action ; « À corriger » d'abord, sortis masqués.
2. **Inscription** : erreurs sous le champ dès la saisie ; doublon dans l'entreprise → lien vers sa fiche ; dans une autre entreprise → « Contactez la mutuelle », sans la nommer ; échec d'envoi → saisie conservée, bouton Réessayer. Import Excel (P1) : mêmes contrôles, ligne par ligne.
3. **Sortie** (remplace « suspendre ») : la RH saisit une date de départ → `FIN_PROGRAMMEE` ; la mutuelle fixe la date de fin selon le contrat.
4. **Facture** (P1) : total du mois décomposé, chaque ligne reliée aux salariés concernés, sans donnée de soin.
5. **« Où en est mon remboursement ? »** : encart d'aide, jamais sur la ligne d'un salarié : « Je n'ai pas accès à vos remboursements, c'est confidentiel. Écrivez au [numéro WhatsApp] où vous envoyez vos demandes. »

**États** : aucun salarié (« Ajoutez votre premier salarié pour démarrer sa couverture ») ; recherche sans résultat (« Pas inscrit, donc pas couvert. », bouton Inscrire) ; rien à corriger ; liste indisponible (« Impossible de charger la liste », bouton Réessayer).

### 4. Questions pour les décideurs

1. **CEO de la mutuelle** : pour la démo TransSahel, il demandait la consommation de chaque salarié, un top 10 des plus gros consommateurs et un bouton de suspension, que la note juridique interdit. Montre-t-on plutôt la couverture et des factures lisibles ? *Par défaut :* oui ; « suspendre » devient « déclarer une sortie », la consommation agrégée passe en V2.
2. **CEO et responsable du support** : quel délai d'activation promet-on aux RH ? *Par défaut :* le délai actuel, affiché en date, puis 1 jour ouvré dès que le support le tient, puisque la ressaisie disparaît.
3. **CEO et responsable du support** : pour les remboursements, améliore-t-on WhatsApp plutôt que de créer un espace salarié ? *Par défaut :* oui, avec un message à chaque étape de la demande, motif de refus compris ; à revoir si les appels RH sur ce sujet ne baissent pas.

### 5. Hypothèses (supposé, non vérifié)

- La mutuelle importe les salariés déjà couverts avant l'ouverture ; tout salarié déclaré est éligible (« pas couvert » = absent de la liste ou sorti).
- Le chauffeur de TransSahel refusé en pharmacie le mois dernier n'était pas encore saisi, ou l'était avec une erreur.
- Une inscription validée est aussitôt visible des pharmacies, même sans carte.
- Les salariés interrogent leur RH faute de nouvelles sur WhatsApp, pas faute de connaître le numéro : ils y envoient déjà leurs demandes.
- La facture se compose des cotisations par salarié et du prorata ; une seule formule par entreprise ; ayants droit hors V1.

---

## Partie 2 · Prototype

### Données fictives

- Génération déterministe (graine fixe) pour TransSahel Logistique : 300 salariés actifs et 12 sortis.
- Prénoms et noms sénégalais variés (Diop, Ndiaye, Fall, Sow, Ba, Sarr, Faye, Gueye, Diallo, Cissé, Mbaye, Seck, Thiam, Kane, Ndour…), matricules `TS-0001` et suivants.
- Statuts : 2 `A_CORRIGER` (un motif `NOM`, un motif `DATE_NAISSANCE`), 5 `EN_ATTENTE`, 3 `FIN_PROGRAMMEE`, le reste `COUVERT` ; les 12 sortis sont `TERMINE`.
- Une liste cachée « autres entreprises » contient au moins Moussa Diop, né le 12/03/1990, pour tester le doublon externe.
- La date du jour est la vraie date du navigateur ; les jours ouvrés excluent samedi et dimanche.

### Écran 1 · Liste des salariés (route `/`)

Ordre de lecture, du plus important au secondaire :

1. En-tête : « Mutuelle Sabali · Espace RH », le nom de l'entreprise et le bouton principal « Inscrire un salarié ».
2. Bandeau d'action : « 2 à corriger » (accent d'alerte) et « 5 en attente », cliquables pour filtrer la liste ; à côté, en discret, le nombre de salariés couverts. Quand rien n'est à corriger : « Tout est à jour ».
3. Recherche par nom ou matricule, filtrage instantané.
4. Tableau : Nom (prénom et NOM), Matricule, Statut (badge et ligne de date : « Couverture depuis le 03/02/2024 », « En attente · couverture d'ici le 05/11/2026 », « À corriger · nom différent de la pièce d'identité », « Fin programmée · couverture jusqu'au 30/11/2026 »), Action (« Corriger » sur les lignes à corriger). Tri : À corriger, puis En attente, puis ordre alphabétique. 50 lignes à la fois.
5. Secondaire, sous le tableau ou en colonne latérale : la bascule « Afficher les salariés sortis » et l'encart « Un salarié vous demande où en est son remboursement ? » avec la phrase du PRD, un numéro WhatsApp fictif (+221 77 000 00 00) et un bouton « Copier le message ».

« Corriger » ouvre le formulaire pré-rempli, avec le motif en tête ; le renvoi repasse le salarié en `EN_ATTENTE`. Sous 640 px de large, le tableau devient une liste de cartes.

### Écran 2 · Inscrire un salarié (route `/inscrire`)

- Lien « Retour à la liste », titre « Inscrire un salarié ».
- Champs dans cet ordre : Prénom(s)*, Nom*, avec l'aide « Tels qu'écrits sur la pièce d'identité : une lettre de différence et la carte peut être refusée en pharmacie. » ; Date de naissance* ; Date d'entrée dans l'entreprise* ; Téléphone WhatsApp (facultatif, « +221 suivi de 9 chiffres ») ; Matricule (facultatif).
- Encart « Après l'envoi », recalculé en direct : « Statut En attente · couverture d'ici le JJ/MM/AAAA. Vous recevrez un email dès que c'est validé. » La date est la plus tardive entre la date d'entrée et aujourd'hui + 5 jours ouvrés.
- Validation à la sortie du champ et à l'envoi. À l'envoi, résumé des erreurs en haut et focus sur le premier champ en erreur. Règles : champs requis ; date de naissance passée, âge entre 16 et 75 ans ; téléphone de 9 chiffres s'il est rempli.
- Doublon (même nom et même date de naissance, sans tenir compte des accents ni de la casse) : dans l'entreprise, « Ce salarié existe déjà. » avec un lien « Voir dans la liste » ; dans une autre entreprise, « Ce salarié est peut-être déjà inscrit ailleurs. Contactez la mutuelle au +221 77 000 00 00. », sans jamais nommer l'autre entreprise.
- Succès : retour à la liste, toast « Inscription envoyée pour Fatou Ndiaye · couverture d'ici le JJ/MM/AAAA », nouvelle ligne surlignée en tête du groupe En attente.
- Échec d'envoi simulé : bandeau en haut du formulaire « L'envoi a échoué. Vos informations sont conservées. » avec un bouton « Réessayer » ; rien n'est perdu.

### États à rendre visibles

- Liste vide : « Aucun salarié pour l'instant. Ajoutez votre premier salarié pour démarrer sa couverture. » et le bouton d'inscription.
- Recherche sans résultat : « Aucun salarié ne correspond à « Diop ». S'il n'apparaît pas, il n'est pas inscrit, donc pas couvert. » et le bouton « Inscrire un salarié ».
- Erreur de chargement : « Impossible de charger la liste. Vérifiez votre connexion. » et « Réessayer ».
- Chargement : squelette du tableau pendant la latence simulée.

### Barre de démo

Une fine bande sombre au-dessus du produit, clairement hors produit :

- « Prototype · données fictives ».
- Sélecteur de scénario : « TransSahel (300 salariés) », « Entreprise sans salarié », « Erreur de chargement ». Le scénario se reflète dans l'URL (`?scenario=vide`, `?scenario=erreur`) pour partager un état par lien.
- Bascule « Simuler un échec d'envoi ».
- Menu « À essayer » : inscrire quelqu'un sans date de naissance ; inscrire Moussa Diop, né le 12/03/1990 (doublon dans une autre entreprise) ; activer l'échec d'envoi puis inscrire.
- Bascule « Annotations » : des pastilles numérotées expliquent la hiérarchie. Sur la liste : 1 ce qui demande une action, 2 retrouver quelqu'un, 3 le détail, 4 l'aide secondaire. Sur le formulaire : 1 les champs qui causent les refus en pharmacie, 2 ce qui se passe après l'envoi.

### Qualité

- Accessibilité : labels liés aux champs, erreurs reliées par `aria-describedby`, navigation au clavier, contraste AA.
- Style sobre : police système ou Inter, gris neutres, une couleur d'accent ; couleurs de statut vert (couvert), ambre (à corriger), bleu (en attente), gris (sorti).

## Partie 3 · Hors périmètre du prototype

- Les parcours 3 à 5 du PRD sauf l'encart d'aide « remboursements » : pas de sortie, pas de factures, pas d'import Excel.
- Authentification, base de données, envoi réel d'emails.
- Toute donnée de santé ou de remboursement, même fictive.

## Partie 4 · Vérification de bout en bout

Le travail est fini quand tout ceci passe, preuves à l'appui :

1. `npm run build` passe sans erreur.
2. Liste, via la barre de démo : TransSahel affiche « 2 à corriger · 5 en attente » en tête ; « Diop » filtre la liste ; une recherche sans résultat affiche son état ; les scénarios « vide » et « erreur » s'affichent, et « Réessayer » recharge.
3. Formulaire : un envoi sans date de naissance montre l'erreur sous le champ et y place le focus ; Moussa Diop, né le 12/03/1990, déclenche le doublon externe sans nommer d'entreprise ; l'échec d'envoi simulé garde la saisie ; une inscription réussie affiche le toast et la ligne surlignée en En attente, avec la bonne date « couverture d'ici ».
4. Le type `Salarie` et les données fictives ne contiennent aucun champ lié à la santé ou aux remboursements.
5. Trois captures dans `screenshots/` : la liste TransSahel, le formulaire avec erreurs, la liste vide.
