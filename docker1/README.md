# Atelier commande de base docker

## Objectif

Démontrer les concepts de base de Ansible

* Déployer un serveur Docker sur vagrant
* Déployer le client Docker
* Exécuter les commandes de base Docker

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
    sudo apt-get update

    # Installer repository docker
    sudo apt-get install -y \
      ca-certificates \
      curl \
      gnupg \
      lsb-release
    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    
    # Installer docker
    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

    # Etapes post installation pour permettre l'utilisation de docker en non admin
    sudo groupadd docker
    sudo usermod -aG docker vagrant
  SHELL
```

* Partager le projet docker actuel avec le serveur docker en ajoutant cette section dans le fichier `Vagrantfile` il est nécessaire que le fichier soit avec les options 600 pour permettre la connexion ssh par clé privée
```
config.vm.synced_folder ".", "/docker", owner: "vagrant", mount_options: ["dmode=775,fmode=600"]
```

* Ouvrir le port `2375` du serveur docker en ajoutant la section suivante dans le `Vagranfile`
```
config.vm.network "forwarded_port", guest: 2375, host: 12375, auto_correct: true
```

* Créer le serveur vagrant depuis le fichier `vagrantfile`
```
vagrant up
```

* En cas d'erreur de provisioning, executer la commande suivante pour relancer le provisioning à nouveau
```
vagrant provision
```

* Pour exposer dockerd sur tcp, connectez-vous en ssh sur le serveur vagrant et ajouter l'options requise
```
vagrant ssh
vi /lib/systemd/system/docker.service
# Changer la ligne
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
# en
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0: --containerd=/run/containerd/containerd.sock
```

* Redemarrer le serveur dockerd
```
sudo systemctl daemon-reload
sudo systemctl restart docker.service
```

* Valider que le port de docker tcp est en écoute
```
ss -ntl
```

* Installer le client docker sur la machine windows. Vous pouvez le télécharger [ici](https://download.docker.com/win/static/stable/x86_64/docker-20.10.20.zip), recupérer le client docker du package et de mettre dans le PATH de votre PC Windows

* Valider que votre client docker a un seul contexte actuellement et que ce contexte ne pointe pas vers le serveur dockerd
```
docker context list
```

* Installer un nouveau contexte qui pointe vers le serveur docker
```
docker context create vagrant --docker host=tcp://127.0.0.1:12375
```

* Valider que la commande suivante est fonctionnelle depuis votre machine Windows
```
docker  -H 127.0.0.1:12375 version
```

* Définir le contexte TCP comme contexte par défaut
```
docker context use vagrant
```

* Sur votre machine locale, exécuter les exercices définis [ici](https://training.play-with-docker.com/ops-s1-hello/)

* Supprimer le serveur Vagrant
```
vagrant destroy
```