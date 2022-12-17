# Atelier déploiement d'un serveur Web en HA avec balancement de charge​

## Objectif

* Déployer des infrastructures composées de plusieurs ressoruces dans AWS
* Déclarer des variables dans un projet Terraform
* Déclarer une sortie dans un projet Terraform

## Instructions de l'atelier

Le projet actuel se trouve dans le repertoire `<racine du repos>/terraform2`

Le projet est actuellement incomplet, par mégarde, deux fichiers ont été supprimés et il est de votre devoir de le reconstruire pour permettre la construction de l'infrastructure.

* Fichier `variables.tf`
* Fichier `outputs.tf`

Par change, les spécifications du contenu de ces deux fichiers sont documentés ici :

### variables.tf

Contient les variables du projet. Les caractéristiques des variables sont :

| Nom de la variable  | Type | Valeur par défaut | Description | 
| ------------- | ------------- | -------------- | ------------ |
| server_port  | number  | Aucune | Le port utilisé par le serveur pour les requêtes HTTP |
| alb_name  | string  | terraform-alb-web | Le nom du balanceur de charge |
| instance_security_group_name  | string  | terraform-instance-asg | Le nom du security group pour les instances EC2 |
| alb_security_group_name  | string  | terraform-web-asg | Le nom du security group du balanceur de charge |

### outputs.tf

Contient les sorties post exécution du script Terraform

| Nom de la variable  | Valeur | Description |
| ------------- | ------------- | -------------- |
| alb_dns_name  | aws_lb.web.dns_name  | Le nom de domaine du balanceur de charge pour l'accès à l'application |

* Reconstruire les fichiers `variables.tf` et `outputs.tf`

* Créer les ressources à l'aide de la commande `terraform apply`

* Valider que vous avew accès à l'application à l'aide du DNS fourni à la fin de l'exécution

* Supprimer les ressources à l'aide de la commande `terraform destroy`
