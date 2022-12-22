# Atelier commande de base Kubernetes

## Objectif

Démontrer les concepts de base de Ansible

* Installer un environnement Kubernetes sur un poste de développement Windows
* Installer le client kubectl qui permet l'administration du cluster Kubernetes
* Exécuter les commandes de base Kubernetes

## Instructions de la démonstration

### Installation kubernetes sur Minikube

* Installer VirtualBox [lien d'installation](https://www.virtualbox.org/wiki/Downloads)

    Dans les environnements Windows, l'installation de VirtualBox peut nécessité l'installation de [Microsoft C++ redistributable 2019](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170)

* La procédure d'installation est disponible [ici](https://minikube.sigs.k8s.io/docs/start/) cette procédure consiste à réaliser les opérations suivantes :

  * Télécharger le binaire minikube et le placer dans le repertoire `c:\minikube\minikube.exe`
  ```powershell
  New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
  Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
  ```

  * Ajouter dans le PATH le programme minikue
  ```powershell
  $oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
  if ($oldPath.Split(';') -inotcontains 'C:\minikube'){ `
    [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine) `
  }
  ```

* Démarrer le cluster minikube
```
minikube start
```

* Démarrer le cluster minikube sur VirtualBox
```
minikube start
```

* Installer `kubectl` pour administrer le cluster kubernetes
```
minikube kubectl -- get po -A
```

* Faire le reste des exercices de la page `minikue start` disponible [ici](https://minikube.sigs.k8s.io/docs/start/)
