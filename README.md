# 🛰️ NetLab API – Simulateur d’architectures réseau orchestré

## ✅ 1. Cahier des charges

### Objectif général :

Créer une API REST qui permet de configurer des architectures réseau en orchestrant des containers Docker via Kubernetes. Cette API doit permettre de :

* Créer, modifier, lister et supprimer des configurations réseau (POST/GET).
* Déployer des configurations réseau dans une architecture reseau virtuelle via un cluster Kubernetes.
* Exécuter des commandes sur des nœuds (pods) simulant des configurations réseau.
* Récupérer les logs et résultats d’exécution des commandes.
* Gérer un catalogue des configurations réseaux prédéfinies et personnalisables.
* Utiliser une base de données pour sauvegarder, charger et versionner les configurations reseaux.

### Livrables :

* Spécification fonctionnelle de l'API, de l'architecture réseau simulée (PDF)
* Schéma de topologie réseau simulée
* Organisation GitHub 
* Structure du dépôt (Back/API – Chart Helm)

---

## 🏗️ 2. Infrastructure

### Conteneurisation :

* Docker avec images réseau spécialisées : FRRouting, Cisco XRd, Juniper vMX.

### Orchestration :

* Kubernetes avec clabernetes pour déployer des topologies réseau.
* Kubernetes avec helm pour déployer l'API et les différents services associé

### Exposition de l’API :

* Ingress NGINX avec TLS (Let's Encrypt ou ACM)
* FASTAPI (option AWS)

### Haute disponibilité & Sauvegardes :

* StatefulSets (si nécessaire), snapshots de volumes, sauvegarde dans S3 via scripts (Bash ou Ansible).

---

## 📦 3. Gestion des données

### Base de données :

* MongoDB ou PostgreSQL pour :

  * Stocker les configurations sauvegardées
  * Gérer les configurations personnalisées (JSON/YAML)
  * Historique des commandes exécutées
  * Logs de simulation

### Logs & commandes :

* Exécution de commandes sur les nœuds via l’API
* Résultats et logs persistés (BDD)
* Filtrage, affichage, historique par utilisateur

### Sécurité d’accès :

* Authentification par token 

---

## 🚀 4. CI/CD

### Pipeline DevOps :

* **Outils** : GitHub Actions, Jenkins 
* **Étapes** :

  * Lint / Tests (Pytest, Shellcheck)
  * Build d’images Docker 
  * Push sur registry
  * Déploiement via kubectl ou Helm upgrade

### Indicateurs suivis :

* Fréquence des déploiements
* Logs de tests automatiques

---

## 📊 5. Supervision

### Outils de monitoring :

* Prometheus + Grafana pour les métriques réseau et Kubernetes
* Kibana pour les logs
* Datadog pour les alertes configurables

### Métriques surveillées :

* État du cluster
* Logs applicatifs (API, simulations)
* Performances réseau simulées (latence, pertes, etc.)
* Résultats de commandes automatisées (ex: ping, ip route)

---

## ⚙️ 6. Automatisation (IaC)

### Provisionnement automatisé :

* Terraform pour l’infra (AWS, VPC, EKS, S3, RDS…)
* Ansible pour la configuration des nodes et images Docker réseau
* Helm pour déploiement des topologies dans K8s

### Modularité et réutilisabilité :

* Templates YAML adaptables (variables pour les nœuds, interfaces, commandes)

### Sécurisation :

* Variables sensibles dans AWS Secrets Manager ou Vault
* Accès restreint via RBAC/ACL (API et K8s)

---

## 📦 Images Docker spécialisées (Catalogue réseau NetLab)

### 🔁 Routeurs / Protocoles de routage

| Image Docker  | Description                          
| ------------- | ------------------------------------ | 
| frrouting/frr | Suite complète OSPF, BGP, RIP, IS-IS | 
| Cisco XRd     | Routeur virtuel Cisco XRd            | 
| Juniper vMX   | Routeur virtuel Juniter              | 

---

