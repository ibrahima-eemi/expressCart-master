# Déploiement de l'Infrastructure avec Ansible

Ce guide explique comment utiliser les playbooks Ansible pour déployer l'infrastructure nécessaire au projet **expressCart**.

## Prérequis

1. **Ansible installé** : Assurez-vous qu'Ansible est installé sur votre machine. Vous pouvez l'installer avec :
    ```bash
    sudo apt update && sudo apt install -y ansible
    ```
2. **Accès SSH** : Ajoutez les clés SSH pour accéder aux machines cibles. Configurez-les dans l'inventaire Ansible (`inventory/production`).

## Machines cibles

- Une machine pour le serveur web (Nginx).
- Une machine pour la base de données (MongoDB).
- Une machine pour le serveur applicatif (Node.js).

## Structure du Projet

```plaintext
ansible_project/
├── inventory/
│   └── production
├── group_vars/
│   └── all.yml
├── roles/
│   ├── webserver/
│   ├── database/
│   ├── appserver/
├── playbooks/
│   └── site.yml
```

## Instructions pour Lancer les Playbooks

1. Clonez ce dépôt sur votre machine locale :
    ```bash
    git clone https://github.com/ibrahima-eemi/expressCart-master.git
    cd ansible_expresscart
    ```
2. Mettez à jour l'inventaire avec les adresses IP des machines cibles dans `inventory/production`.

3. Lancez le playbook principal :
    ```bash
    ansible-playbook -i inventory/production playbooks/site.yml
    ```

Ce playbook configure :

- Le serveur web (Nginx).
- La base de données (MongoDB).
- Le serveur applicatif (Node.js) avec le projet expressCart.

## Dépannage

Si des erreurs se produisent, utilisez le mode verbose pour obtenir des détails :
```bash
ansible-playbook -i inventory/production playbooks/site.yml -v
```

Pour cibler un rôle ou une machine spécifique, utilisez l'option `--tags` ou `--limit` :
```bash
ansible-playbook -i inventory/production playbooks/site.yml --tags "webserver"
```