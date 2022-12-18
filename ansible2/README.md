## Atelier installer un serveur jenkins avec Ansible

## Objectif

* Réutilsier des scripts Ansible fournis par la communauté dans son projet
* Créer un playbook Ansible
* Déployer une application en respectant les bonnes pratiques avec Ansible

## Instructions de l'atelier

* Copier le fichier `Vagrantfile` de l'exercice précédent et créer une nouvelle VM pour cet exercice

* Avant de lancer la création du serveur vagrant, modifier le fichier `Vagrantfile` pour activer la redirection du port `8080` pour pouvoir accéder au serveur
```
config.vm.network "forwarded_port", guest: 8080, host: 8080
```

* Réutiliser la VM vagrant de l'exercice précédent et vous s'y connecter

* Le script utilé pour l'installation est [le rôle officielle Jenkins pour le déploiement de ce type d'application via Ansible](https://galaxy.ansible.com/geerlingguy/jenkins)

* Installer des librairies `geerlingguy.jenkins` et `geerlingguy.java` dans le projet à l'aide de la commande
```
ansible-galaxy install -r requirements.yml
```

* Lancer la création du serveur jenkins à l'aide de la commande
```
ansible-playbook playbook.yml -vv
```

* Valider que le script est idempotent lors de la seconde exécution et que rien ne se passe
```
ansible-playbook playbook.yml -vv
```

* Dans le navigateur, accéder au serveur à l'aide du lien `http://localhost:8080` et les credentiales `admin/admin`
