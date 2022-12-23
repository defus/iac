# Atelier déployer une application sur AKS (Azure Kubernetes Services)

## Objectif

Manipuler les fonctionnalités de base AKS :

* Déployer un registre de conteneur et pousser l'image de l'application sur ce conteneur d'images
* Déployer son cluster AKS
* Déployer son application sur AKS

## Instructions de la démonstration

* Déployer la VM Vagrant du [lab docker1](../docker1/README.md)

* Se connecter sur la VM vagrant

```
vagrant ssh
```

* Installer docker-compose sur la station vagrant

```
sudo apt-get install docker-compose
```

* Installer la commande `az` sur la vm vagrant [voir ici](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt)

* Se connecter à azure `az` sur la vm vagrant [voir ici](https://learn.microsoft.com/en-us/cli/azure/authenticate-azure-cli?source=recommendations)

* Faire le lab [ici](https://learn.microsoft.com/en-us/azure/aks/tutorial-kubernetes-prepare-app) pour préparer l'application pour AKS 

* Faire le lab [ici](https://learn.microsoft.com/en-us/azure/aks/tutorial-kubernetes-prepare-acr?tabs=azure-cli) pour préparer votre registre de container docker ACR (Azure Container Registry)

  Pour pouvoir pousser l'image sur l'ACR, il faut recuperer l'access-token à l'aide de la commande
  ```
  az acr login --name acrdefus --expose-token
  ```

  Et utilsier cet access-token pour se connecter comme suit
  ```
  docker login acrdefus.azurecr.io --username 00000000-0000-0000-0000-000000000000 --password $ACCESS_TOKEN
  ```
  
* Faire le lab [ici](https://learn.microsoft.com/en-us/azure/aks/tutorial-kubernetes-deploy-cluster?tabs=azure-cli) pour préparer votre registre de container docker ACR (Azure Container)

  Commande de creation du cluster
  ```
  az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 2 \
    --generate-ssh-keys \
    --attach-acr acrdefus
  ```

* Faire le lab [ici](https://learn.microsoft.com/en-us/azure/aks/tutorial-kubernetes-deploy-application?tabs=azure-cli) pour exécuter l'application dans le cluster.

* Exécuter la commande suivante pour supprimer le cluster AKS :

```
az group delete --name myResourceGroup --yes --no-wait
```