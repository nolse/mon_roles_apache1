# Rôle Ansible : webapp (mon_roles_apache1)

Ce rôle déploie une application web simple dans un conteneur Docker Apache (`httpd`).  
Il fonctionne aussi bien sur CentOS 7 que dans un environnement Docker‑in‑Docker.

## 🚀 Fonctionnalités

- Installation des dépendances (EPEL, pip, docker-py)
- Vérification de Docker (installation si absent, skip si présent)
- Déploiement d’un conteneur Apache
- Copie d’un template HTML personnalisable
- Redémarrage automatique du conteneur si le template change
- Tags pour exécuter uniquement certaines parties du rôle

## 📁 Structure du rôle


webapp/
├── defaults/
│   └── main.yml
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   ├── main.yml
│   ├── dependencies.yml
│   ├── docker.yml
│   └── webapp.yml
├── templates/
│   └── index.html.j2
└── tests/
├── inventory
└── test.yml


## 🔧 Variables

| Variable | Description | Valeur par défaut |
|---------|-------------|-------------------|
| `webapp_user` | Utilisateur propriétaire du fichier HTML | `admin` |
| `webapp_index_src` | Template HTML source | `index.html.j2` |
| `webapp_index_dest` | Chemin du fichier HTML généré | `/home/admin/index.html` |
| `webapp_container_name` | Nom du conteneur | `webapp` |
| `webapp_image` | Image Docker utilisée | `httpd` |
| `webapp_http_port` | Port exposé | `80` |

## ▶️ Exemple d’utilisation

```yaml
- hosts: prod
  become: true
  roles:
    - webapp

