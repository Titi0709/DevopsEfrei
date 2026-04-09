# 🚀 TP DevOps – GitHub Actions + Runner Self-Hosted + Ansible

## 📌 Objectif

Mettre en place un pipeline CI/CD avec GitHub Actions utilisant un runner self-hosted sur une machine virtuelle Ubuntu, puis exécuter Terraform et Ansible automatiquement.

---

## 🖥️ 1. Création de la VM

- Installation de VirtualBox
- Téléchargement de l’ISO Ubuntu
- Création de la machine virtuelle Ubuntu
- Installation d’Ubuntu

---

## ⚙️ 2. Installation du runner GitHub Actions

Dans la VM :

    mkdir actions-runner && cd actions-runner
    curl -o actions-runner.tar.gz -L <URL fournie par GitHub>
    tar xzf actions-runner.tar.gz

Configuration du runner :

    ./config.sh --url https://github.com/Titi0709/DevopsEfrei --token <TOKEN>

Lancement du runner :

    ./run.sh

Le runner est maintenant connecté et en attente de jobs.

---

## 📂 3. Création du pipeline GitHub Actions

Fichier :

    .github/workflows/main.yml

Contenu :

    name: DevOps Pipeline

    on:
      push:
        branches:
          - main

    jobs:
      test-runner:
        runs-on: self-hosted

        steps:
          - name: Checkout
            uses: actions/checkout@v4

          - name: Test Terraform
            run: terraform -v

          - name: Test Ansible
            run: ansible --version

          - name: Ansible ping
            run: ansible -i inventory.ini all -m ping

          - name: Run playbook
            run: ansible-playbook -i inventory.ini playbook.yml

---

## 🧱 4. Installation des outils

### Terraform

    sudo apt update
    sudo apt install -y gnupg software-properties-common curl
    curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
    sudo apt-add-repository "deb https://apt.releases.hashicorp.com $(lsb_release -cs) main"
    sudo apt update
    sudo apt install terraform -y

Vérification :

    terraform -v

---

### Ansible

    sudo apt update
    sudo apt install ansible -y

Vérification :

    ansible --version

---

## 📄 5. Inventory Ansible

Fichier `inventory.ini` :

    [local]
    localhost ansible_connection=local

---

## ⚙️ 6. Playbook Ansible

Fichier `playbook.yml` :

    - name: Configure localhost
      hosts: local
      become: true

      tasks:
        - name: Gather facts
          ansible.builtin.setup:

        - name: Set timezone to Europe/Paris
          ansible.builtin.timezone:
            name: Europe/Paris

        - name: Set timezone to Africa/Abidjan
          ansible.builtin.timezone:
            name: Africa/Abidjan

---

## 🔁 7. Exécution du pipeline

    git add .
    git commit -m "update pipeline"
    git push origin main

---

## ⚡ 8. Fonctionnement global

1. Push sur GitHub  
2. Déclenchement du pipeline  
3. GitHub envoie le job au runner  
4. Le runner (VM) exécute :
   - checkout
   - Terraform
   - Ansible
   - playbook  

---

## 📸 9. Screenshots

### Pipeline GitHub Actions
![Pipeline](./screenshots/pipeline.png)

### Runner dans la VM
![Runner](./screenshots/runner.png)

---

## ✅ Résultat

- Runner self-hosted fonctionnel  
- Pipeline opérationnel  
- Terraform et Ansible exécutés automatiquement  
- Configuration système via Ansible  

---

## 👨‍💻 Auteur

Thibault Lefay