# Démonstration ansible commmandes

## Objectif

Démontrer les concepts de base de Ansible

* Déployer un serveur Ansible sur Vagrant
* Déployer un fichier inventaire Ansible
* Exécuter la commande ping
* Installer des commandes
* Créer un groupe d'inventaires et effectuer des opérations sur le groupe

## Instructions de la démonstration

* Installer VirtualBox [lien d'installation](https://www.virtualbox.org/wiki/Downloads)

    Dans les environnements Windows, l'installation de VirtualBox peut nécessité l'installation de [Microsoft C++ redistributable 2019](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170)

* Installer vagrant [lien d'installation](https://developer.hashicorp.com/vagrant/docs/installation)

* Créer un fichier `vagrantfile` permettant la création du serveur vagrant depuis le repertoire `<repertoire racine>/demo-ansible` 
```
vagrant init ubuntu/focal64
```

* Créer le serveur vagrant depuis le fichier `vagrantfile`
```
vagrant up
```

* Consulter les informations de connection ssh avec le serveur vagrant
```
vagrant ssh-config
```

la réponse est de la forme
```
PS C:\Users\idade\code\iac\demo-ansible> vagrant ssh-config
Host default
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile C:/Users/idade/code/iac/demo-ansible/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

* Vérifier du coup qu'il est possible de se connecter sur le serveur avec cette adresse
```
ssh vagrant@127.0.0.1 -p 2222 -i .vagrant/machines/default/virtualbox/private_key
```

* Installer Ansible sur le serveur Vagrant
```sh
# Vérifier si pip est installé
python3 -m pip -V
# Installer pip
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py --user
# Installer Ansible
python3 -m pip install --user ansible
# Valider l'installation
ansible --version
```

* Utiliser le fichier `vagrant1` de base pour exécuter la commande ping sur le serveur present dans l'inventaire

* Utiliser le fichier `vagrant2` de base pour exécuter la commande ping sur le serveur present dans l'inventaire

* Utiliser le fichier `vagrant3` de base pour exécuter la commande adhoc sur le groupe de serveurs present dans l'inventaire

* Supprimer le serveur Vagrant
```
vagrant
```