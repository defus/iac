# Atelier git et github

## Objectif de l'atelier

L'objectif de l'atelier git et Github va être de :

* Valider que vous êtes en mesure d'utiliser les commandes de base de git 

* Et de manipuler les fonctionnalités simple de github

## Déroulement de l'atelier


* Recupérer ce projet sur votre ordinateur​
```
git clone https://github.com/defus/iac.git​
```

* Créer une branche de travail qui porte votre prénom à partir du projet recupéré​
```
cd iac​
git checkout -b <prénom>​
```

* Ajouter un fichier nommé `file1.txt` dans le répertoire `/<racine projet>/git/` ; dans ce fichier, enregistrez votre prénom

* Faire un commit du fichier dans l'historique git
```
git add git/file1.txt
git commit -m "J'ai ajouté mon prénom dans le fichier"
```

* Pousser le fichier sur github​
```
git push --set-upstream origin prénom
```

* Créer une PR (pull request) sur github pour demander la fusion de votre changement dans la branche principale (main)
