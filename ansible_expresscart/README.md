Déploiement de l'Infrastructure avec Ansible
# Guide de Déploiement avec Ansible pour expressCart

Ce guide fournit des instructions détaillées pour utiliser Ansible afin de configurer et déployer les différentes composantes nécessaires au projet expressCart.

![image](https://github.com/user-attachments/assets/4ca962f4-b56a-4e8a-ba12-702a71ab8c28)

## Prérequis

### Installer Ansible
Assurez-vous qu'Ansible est installé sur votre machine. Si ce n'est pas déjà fait, installez-le avec :

```bash
sudo apt update && sudo apt install -y ansible
```

### Configuration SSH
- Configurez l'accès SSH aux machines cibles en ajoutant vos clés SSH.
- Définissez les machines cibles dans l'inventaire Ansible (`inventory/production`).

### Vérifications système
- Les machines doivent disposer des prérequis système pour Nginx, MongoDB, et Node.js.
- Un accès sudo doit être configuré pour l'utilisateur Ansible.

## Architecture cible
Le projet se compose de trois services principaux, déployés sur des machines distinctes :
- **Serveur web** : Nginx.
- **Base de données** : MongoDB.
- **Serveur applicatif** : Node.js, avec l'application expressCart accessible sur le port 1111.

## Structure du Projet
Voici l'organisation des fichiers et dossiers Ansible :

```plaintext
ansible_project/
├── inventory/
│   └── production               # Inventaire des machines cibles
├── group_vars/
│   └── all.yml                  # Variables globales pour toutes les machines
├── roles/
│   ├── webserver/               # Configuration de Nginx
│   ├── database/                # Configuration de MongoDB
│   ├── appserver/               # Configuration du serveur applicatif
├── playbooks/
│   └── site.yml                 # Playbook principal orchestrant tout le déploiement
```

## Instructions pour Lancer les Playbooks

### Cloner le dépôt
Téléchargez le projet sur votre machine locale :

```bash
git clone https://github.com/ibrahima-eemi/expressCart-master.git
cd expressCart-master/ansible_expresscart
```

### Configurer l'inventaire
Ouvrez le fichier `inventory/production` et ajoutez les adresses IP ou noms d’hôte des machines cibles. Par exemple :

```ini
[webserver]
192.168.1.10 ansible_user=ubuntu

[database]
192.168.1.11 ansible_user=ubuntu

[appserver]
192.168.1.12 ansible_user=ubuntu
```

### Mettre à jour les variables
Configurez les paramètres globaux dans `group_vars/all.yml` :

```yaml
app_port: 1111
db_name: expresscart
db_user: admin
db_password: securepassword
db_host: 127.0.0.1
```

### Exécuter le playbook principal
Lancez le playbook pour déployer toute l'infrastructure :

```bash
ansible-playbook -i inventory/production playbooks/site.yml --become
```

## Dépannage

### Mode verbose
Si une erreur survient, utilisez le mode verbose pour plus de détails :

```bash
ansible-playbook -i inventory/production playbooks/site.yml --become -vvv
```

### Exécution ciblée
Pour exécuter un rôle spécifique :

```bash
ansible-playbook -i inventory/production playbooks/site.yml --tags "webserver"
```

Pour déployer uniquement sur une machine spécifique :

```bash
ansible-playbook -i inventory/production playbooks/site.yml --limit "webserver"
```

### Logs MongoDB
Si MongoDB ne fonctionne pas comme prévu, consultez les journaux :

```bash
journalctl -u mongod
```

### Redémarrage des services
Si un service ne fonctionne pas, redémarrez-le :

```bash
sudo systemctl restart nginx
sudo systemctl restart mongod
```

## Résultat attendu
Après l'exécution du playbook :
- **Serveur web** : Nginx est configuré et accessible.
- **Base de données** : MongoDB est configuré avec un utilisateur pour l'application.
- **Application** : Le projet expressCart est déployé et accessible sur le port 1111.

## Vérification de l'installation

### Accéder à l'application
Ouvrez un navigateur et visitez l'adresse suivante :

```http
http://localhost:1111
```

### Personnalisation
Vous pouvez modifier le port ou d'autres paramètres via `group_vars/all.yml`.
