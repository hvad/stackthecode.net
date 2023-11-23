+++
title = 'Aws terraform authentification'
draft = false
description = 'Article to begin with AWS'
categories = ['AWS', 'Terraform']
tags = ['AWS', 'Terraform']
comments = false # Enable Disqus comments for specific page
authorbox = true # Enable authorbox for specific page
pager = true # Enable pager navigation (prev/next) for specific page
toc = true # Enable Table of Contents for specific page
mathjax = true # Enable MathJax for specific page
+++

# How to terraform authentification on AWS

## First step in AWS IHM

Connect with your account to AWS and clic on `Informations d'identification de sécurité`.

Clic on `Créer une clé d'accès` and after choose `Interface de ligne de commande (CLI)`

Check the box `Je comprends la recommandation ci-dessus et 
je souhaite procéder à la création d'une clé d'accès.` and clic on button `Suivant`

Enter a description and clic on button `Créer une clef d'accès`

Clic on button `Télécharger le fichier .csv`

## Configure AWS Cli 

Edit csv file to add header User Name (profil). It must look like below :

```
User Name,Access key ID,Secret access key
profil,EESHOO0A3CH,kigEESHOO0A6RAkeeche2FEUEE1ohshai1
```

Import csv file with command below (profil_accessKeys is csv file):

```
$ ws configure import --csv file://profil_accessKeys.csv
```

Check with command below : 

```
$ aws configure list --profile profil
  Name                    Value             Type    Location
  ----                    -----             ----    --------
profile                   profil           manual    --profile
access_key     ****************3CH shared-credentials-file    
secret_key     ****************shai1 shared-credentials-file    
region                <not set>             None    None
```

A file ~/.aws/credentials was create with information below : 

```
[profil]
aws_access_key_id = EESHOO0A3CH
aws_secret_access_key = kigEESHOO0A6RAkeeche2FEES2QUEE1ohshai1
```

## Terraform configuration

Edit or create file main.tf like below :
 
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

Edit or create file provider.tf like below :

```
# Configure the AWS Provider
provider "aws" {
  region     = var.aws_region
  shared_credentials_files = ["~/.aws/credentials"]
  profile = "profil"
}
```

Authentification at AWS is configured now. 
