# Rôle Ansible : webapp

Déploie une petite application web statique dans un conteneur **Docker Apache (httpd)**.

Fonctionne sur :
- CentOS 7 / Rocky 8 / AlmaLinux 8 / RHEL 7–8
- Environnements avec Docker déjà installé
- Docker-in-Docker (DinD)

## Fonctionnalités principales

- Installation des prérequis (EPEL, python3-pip, docker-py)
- Vérification + installation conditionnelle de Docker
- Lancement d’un conteneur httpd
- Génération et copie d’une page `index.html` à partir d’un template Jinja2
- Redémarrage automatique du conteneur quand la page change (via handler)
- Tags disponibles pour exécuter uniquement certaines parties

## Structure du rôle

mon_roles_apache1/
├── defaults/
│   └── main.yml
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   ├── dependencies.yml
│   ├── docker.yml
│   ├── main.yml
│   └── webapp.yml
├── templates/
│   └── index.html.j2
└── tests/
    ├── inventory
    └── test.yml
    
text## Variables principales (defaults/main.yml)

| Variable                     | Description                                  | Valeur par défaut             |
|------------------------------|----------------------------------------------|-------------------------------|
| `webapp_user`                | Utilisateur propriétaire du fichier HTML     | `admin`                       |
| `webapp_index_src`           | Nom du template source                       | `index.html.j2`               |
| `webapp_index_dest`          | Chemin destination sur la machine cible      | `/home/admin/index.html`      |
| `webapp_container_name`      | Nom du conteneur Docker                      | `webapp`                      |
| `webapp_image`               | Image Docker à utiliser                      | `httpd`                       |
| `webapp_http_port`           | Port exposé sur l’hôte                       | `80`                          |

→ Tu peux surcharger toutes ces variables dans un fichier `group_vars` / `host_vars` ou directement dans le play.

## Exemples d’utilisation

### 1. Utilisation basique (recommandé)

```yaml
- hosts: webservers
  become: true

  roles:
    - webapp
2. Avec quelques personnalisations
YAML- hosts: prod
  become: true

  vars:
    webapp_user: "www-data"
    webapp_index_dest: "/var/www/html/index.html"
    webapp_http_port: 8080
    webapp_container_name: "site-prod"

  roles:
    - webapp
3. Exécuter seulement certaines parties (tags)
Bash# Seulement installer les dépendances et Docker
ansible-playbook site.yml --tags "dependencies,docker"

# Juste régénérer la page HTML et redémarrer si besoin
ansible-playbook site.yml --tags "webapp"

# Tout sauf l'installation de Docker
ansible-playbook site.yml --skip-tags "docker_install"
Tester le rôle en local
Bash# Depuis le dossier du rôle
ansible-playbook -i tests/inventory tests/test.yml
