# Atelier création instance AWS EC2 avec Terraform

## Objectif

* Utiliser Terraform pour effectuer le déploiement d'une instance AWS EC2

## Instructions de l'atelier

* La documentation du fournisseur Terraform AWS est accessible en ligne [ici](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)

* Créer un utilisateur AWS avec les droits adminitsrateur

* Installer la ligne de commande AWS ; les instructions sont [ici](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

* Définir les variables d'accès à AWS
```
export AWS_ACCESS_KEY_ID=<identifiant cré d'accès AWS>
export AWS_SECRET_ACCESS_KEY=<secret cré d'accès AWS>
```

* Recupérer le projet sur votre ordinateur​
```
git clone https://github.com/defus/iac.git​
```

* Enregistrer le fournisseur Terraform
```
git checkout -b ec2-<prénom>
cd iac/terraform1
mkdir ec2
cd ec2
touch main.tf
```

* Ajouter dans le fichier `main.tf` le contenu suivant (code de création du serveur EC2) :

```terraform
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region  = "ca-central-1"
}

resource "aws_instance" "ec2_server" {
  ami           = "ami-03520d0f674d64df7"
  instance_type = "t2.micro"

  tags = {
    Name = "ExempleAppServerInstance"
  }
}
```

* Initialiser la configuration Terraform du projet avec la commande :
```
terraform init
```
   
   A la fin de la commande, constaer qu'un fichier `.terraform.lock.hcl` est crée et contient la version exacte de terraform téléchargé et utilisé.

   Le repertoire `.terraform` contient le provider AWS recupéré.


* Formater et valider la configuration à l'aide de la commande :
```
terraform fmt
```

    La commande affiche la liste des ficheirs qui ont été formatés

* Valider la configuration à l'aide de la commande suivante :
```
terraform validate
```

* Créer l'infrastructure à l'aide de la commande :
```
terraform apply
```

* Supprimer les ressources crées
```
terraform destroy
```

* Versionner le changement
```
git add .
git commit -m "Premier script de création de serveur"
```
