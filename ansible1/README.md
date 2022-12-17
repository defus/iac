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

* Créer un fichier `vagrantfile` permettant la création du serveur vagrant depuis le repertoire `<repertoire racine>/ansible1` 
```
vagrant init ubuntu/focal64
```

* Ajouter le script d'installation Ansible dans le fichier `Vagrantfile`
```
  config.vm.provision "shell", inline: <<-SHELL
    # Mettre à jour la vm
    apt-get update
    # Installer pip
    su -c 'curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py' vagrant
    su -c 'python3 get-pip.py --user' vagrant
    # Installer Ansible
    su -c 'python3 -m pip install --user ansible' vagrant
  SHELL
```

* Partager le projet ansible actuel avec le serveur ansible en ajoutant cette section dans le fichier `Vagrantfile`
```
config.vm.synced_folder ".", "/vagrant"
```

* Créer le serveur vagrant depuis le fichier `vagrantfile`
```
vagrant up
```

* Consulter les informations de connection ssh avec le serveur vagrant
```
vagrant ssh-config
```

La réponse est de la forme
```
PS C:\Users\idade\code\iac\ansible1> vagrant ssh-config
Host default
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile C:/Users/idade/code/iac/ansible1/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

* Vérifier du coup qu'il est possible de se connecter sur le serveur avec cette adresse
```
ssh vagrant@127.0.0.1 -p <port dans vagrant ssh-config> -i .vagrant/machines/default/virtualbox/private_key
```

* Véfifier que Ansible et pip sont installés sur le serveur
```sh
# Vérifier si pip est installé
python3 -m pip -V
```

* Vérifier quue le repertoire `/vagrant` est monté sur le serveur et contient les données du projet 

* Utiliser le fichier `vagrant1` de base pour exécuter la commande `ping` sur le serveur present dans l'inventaire
```
cd /vagrant
ansible testserver -i inventory/vagrant1.ini -m ping
```

* Utiliser le fichier `vagrant2` de base pour exécuter la commande `ping` sur le serveur present dans l'inventaire
```
cd /vagrant
ansible testserver -i inventory/vagrant2.ini -m ping
```

* Utiliser le fichier `vagrant3` de base pour exécuter la `commande adhoc` sur le groupe de serveurs present dans l'inventaire
```
cd /vagrant
ansible servers -i inventory/vagrant3.ini -m ansible.builtin.command -a uptime
ansible servers -i inventory/vagrant3.ini -a uptime
ansible bdserver -i inventory/vagrant3.ini -a "tail /var/log/dmesg"
ansible bdserver -i inventory/vagrant3.ini -m ansible.builtin.package -a "name=git state=present" --become
```

* Déclarer l'emplacement de l'inventaire dans le fichier de configuration `./ansible.cfg` et valider qu'il n'est plus requis de préciser l'emplacement de l'inventaire dans la ligne de commande ansible.
```
[defaults]
inventory=inventory/vagrant3.ini
```

* Valider avec la commande
```
ansible bdserver -a "tail /var/log/dmesg"
```

* Supprimer le serveur Vagrant
```
vagrant destroy
```