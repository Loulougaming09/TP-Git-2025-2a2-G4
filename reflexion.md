1. Expliquez, en quelques phrases, la structure actuelle du projet Git après toutes les opérations (branches, merges, commits).

  Après une série d'opérations typiques (branches, merges, commits), un projet Git peut ressembler à ceci :

    * commit abc123 (HEAD -> main)
    |\ Merge: def456 ghi789
    | | Merge branch 'feature' into main
    | |
    | * commit ghi789 (feature)
    | | Ajout nouvelle fonctionnalité
    | |
    * | commit def456
    |/  Fix bug sur main
    |
    * commit jkl012
      Initial commit

  Structure expliquée :

    La branche main contient l'historique principal du projet
    La branche feature a divergé de main pour développer une fonctionnalité isolée
    Un merge a réintégré les modifications de feature dans main, créant un commit de fusion
    HEAD pointe vers le dernier commit de la branche active (main)

  Cette structure en graphe montre clairement les points de divergence et de convergence entre les branches.

2. Quelle est la différence entre `git fetch` et `git pull` ?

  git fetch : Télécharge les modifications du dépôt distant SANS les fusionner automatiquement avec votre branche locale.
  git pull : Fait git fetch + git merge automatiquement (ou rebase selon configuration).

  Exemple concret : 
    Préférer git fetch quand :
      Vous travaillez sur une fonctionnalité critique et voulez d'abord examiner les changements distants avant de les intégrer :
      bashgit fetch origin
      git log HEAD..origin/main  # Voir ce qui a changé
      git diff HEAD origin/main  # Examiner les différences
      git merge origin/main      # Fusionner seulement si OK

    Préférer git pull quand :
      Vous êtes sur une branche de synchronisation régulière et faites confiance aux modifications distantes :
      bashgit pull origin main  # Rapide et direct

3. Expliquez la différence entre `git reset` et `git revert`.

git reset : 

  Réécrit l'historique en déplaçant le pointeur HEAD
  Supprime les commits de l'historique
  Modes : --soft (garde les modifications), --mixed (par défaut), --hard (supprime tout)

git revert :

  Crée un nouveau commit qui annule les modifications d'un commit précédent
  Préserve l'historique complet
  Sûr pour les branches partagées

Exemple de chaque commande :

  git reset (réécrit l'historique)
    git reset --hard HEAD~1  # Supprime le dernier commit

  git revert (préserve l'historique)
    git revert abc123  # Crée un nouveau commit annulant abc123

Utiliser git reset --hard sur une branche déjà poussée et partagée avec d'autres développeurs est très risqué car cela réécrit l'historique distant, provoque des conflits pour les collègues ayant récupéré les commits supprimés, et peut entraîner une perte de travail pour toute l'équipe. Sur les branches partagées, il faut privilégier git revert qui préserve l'historique en créant simplement de nouveaux commits d'annulation.