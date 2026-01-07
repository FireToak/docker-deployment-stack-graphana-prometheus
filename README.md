# 📝 Déploiement Supervision (Prometheus & Grafana) - Docker Compose

Ce dépôt contient la configuration "Infrastructure as Code" pour déployer une stack de supervision complète comprenant **Prometheus** et **Grafana**.

L'architecture intègre une authentification centralisée (SSO) via **Authentik** et une gestion fine des ressources via Docker Compose.

## 🏗️ Architecture Technique

Le déploiement orchestre les composants suivants :

* **Collecte de métriques :** Prometheus (`v3.9.0`) - Rétention de 15 jours
* **Visualisation :** Grafana (`v12.4.0`)
* **Authentification :** OAuth2 générique via Authentik (LoutikSSO)
* **Persistance :** Volumes Docker nommés pour les données et bind mount pour la configuration

## 📂 Structure du dépôt

* [`docker-compose.yaml`](docker-compose.yaml) : Définition des services, réseaux et volumes
* [`env.example`](env.example) : Modèle des variables d'environnement (Secrets SSO)
* [`.gitignore`](.gitignore) : Exclusion du fichier `.env` contenant les secrets

## 🚀 Prérequis

* Docker et Docker Compose installés
* Git est installé
* Une application créée sur Authentik (Provider OAuth2)
* Le fichier de configuration Prometheus présent sur l'hôte : `/home/docker/prometheus/prometheus.yml`

## 🛠️ Installation

1.  **Cloner le dépôt :**
    ```bash
    git clone https://github.com/FireToak/docker-deployment-stack-grafana-prometheus.git
    cd docker-deployment-stack-grafana-prometheus
    ```

2.  **Configuration de l'environnement :**
    Dupliquez le fichier d'exemple et configurez vos secrets Authentik :
    ```bash
    cp env.example .env
    nano .env
    ```
    *Remplissez `SSO_ID` et `SSO_SECRET` avec les valeurs de votre provider Authentik.*

3.  **Déployer la stack :**
    ```bash
    docker compose up -d
    ```

4.  **Vérification :**
    ```bash
    docker compose ps
    ```
    Accès à Grafana : `https://supervision.loutik.fr`

## ⚙️ Configuration

### Authentification SSO (Authentik)
Le mapping des rôles est configuré automatiquement via `GF_AUTH_GENERIC_OAUTH_ROLE_ATTRIBUTE_PATH`:
* **Admin :** Groupe 'Grafana Admins'
* **Editor :** Groupe 'Grafana Editors'
* **Viewer :** Autres utilisateurs connectés

### Ressources allouées (Par conteneur)
Des limites strictes sont appliquées pour garantir la stabilité du VPS:
- **Limits :** 1024M RAM / 0.50 CPU
- **Reservations :** ~128M RAM / 0.10 CPU

## 👤 Auteur

**Louis MEDO** - *Étudiant BTS SIO & Passionné par l'administration système ❤️*
