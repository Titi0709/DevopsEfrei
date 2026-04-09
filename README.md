# TP DevOps - GitHub Actions, Terraform, Ansible et Runner Self-Hosted

## Objectif
Ce projet a pour objectif de mettre en place une chaîne DevOps simple avec :
- une machine virtuelle Ubuntu
- un runner GitHub self-hosted
- un pipeline GitHub Actions
- l'installation de Terraform
- l'installation et l'exécution d'Ansible en local

## Architecture
L'architecture utilisée dans ce TP est la suivante :
- Le code source est stocké dans un dépôt GitHub
- Un runner self-hosted est installé sur une machine virtuelle Ubuntu
- Lors d'un push sur la branche `main`, GitHub Actions déclenche automatiquement un pipeline
- Ce pipeline est exécuté par le runner local
- Le pipeline vérifie la présence de Terraform et Ansible, puis peut exécuter des commandes Ansible

## Étapes réalisées

### 1. Création de la machine virtuelle
- Installation de VirtualBox
- Téléchargement de l'image ISO Ubuntu 24.04 LTS
- Création et démarrage d'une machine virtuelle Ubuntu

### 2. Installation du runner GitHub
Depuis les paramètres du dépôt GitHub, un runner self-hosted Linux x64 a été configuré.

Commandes utilisées :
```bash
mkdir actions-runner && cd actions-runner
curl -o actions-runner.tar.gz -L <URL_DONNEE_PAR_GITHUB>
tar xzf actions-runner.tar.gz
./config.sh --url <URL_DU_REPO> --token <TOKEN_GITHUB>
./run.sh