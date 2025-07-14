# 🛰️ NetLab API – Simulateur d’architectures réseau orchestré

## ✅ 1. Cahier des charges

### Objectif général :

Créer une API REST qui permet de simuler des architectures réseau dynamiques en orchestrant des containers Docker via Kubernetes. Cette API doit permettre de :

* Créer, modifier, lister et supprimer des topologies réseau (POST/GET).
* Déployer des simulations réseau dans un cluster Kubernetes.
* Exécuter des commandes sur des nœuds (pods) simulant du matériel réseau.
* Récupérer les logs et résultats d’exécution des commandes.
* Gérer un catalogue de topologies prédéfinies et personnalisables.
* Utiliser une base de données pour sauvegarder, charger et versionner des configurations.

### Livrables :

* Spécification fonctionnelle (PDF)
* Schéma de topologie réseau simulée
* Organisation GitHub + Trello
* Structure du dépôt (Back/API – Chart Helm – Infra)

---

## 🏗️ 2. Infrastructure

### Conteneurisation :

* Docker avec images réseau spécialisées : FRRouting, Open vSwitch, VyOS (si containerisable).

### Orchestration :

* Kubernetes (EKS, Minikube, Kind) avec Helm pour déployer des topologies réseau comme des graphes de pods/services.

### Connectivité réseau :

* CNI Calico ou Cilium pour gérer le routage entre les pods et les politiques réseau.

### Exposition de l’API :

* Ingress NGINX avec TLS (Let's Encrypt ou ACM)
* API Gateway (option AWS)

### Haute disponibilité & Sauvegardes :

* StatefulSets (si nécessaire), snapshots de volumes, sauvegarde dans S3 via scripts (Bash ou Ansible).

---

## 📦 3. Gestion des données

### Base de données :

* MongoDB ou PostgreSQL pour :

  * Stocker les topologies sauvegardées
  * Gérer les configurations personnalisées (JSON/YAML)
  * Historique des commandes exécutées
  * Logs de simulation

### Logs & commandes :

* Exécution de commandes sur les nœuds via l’API
* Résultats et logs persistés (BDD ou S3)
* Filtrage, affichage, historique par utilisateur

### Sécurité d’accès :

* Authentification JWT
* RBAC (admin / user / readonly)

---

## 🚀 4. CI/CD

### Pipeline DevOps :

* **Outils** : GitHub Actions, Jenkins ou CircleCI
* **Étapes** :

  * Lint / Tests (Pytest, Shellcheck)
  * Build d’images Docker (ex: frr\:latest)
  * Push sur registry
  * Déploiement via kubectl ou Helm upgrade
  * Blue/Green deployment (namespace isolés)

### Indicateurs suivis :

* Fréquence des déploiements
* Durée de build
* Rollback rate
* Logs de tests automatiques

---

## 📊 5. Supervision

### Outils de monitoring :

* Prometheus + Grafana pour les métriques réseau et Kubernetes
* Loki ou CloudWatch pour les logs
* AlertManager (ou Datadog) pour les alertes configurables

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

* Un dossier Helm chart par topologie ou environnement (dev, test, prod)
* Templates YAML adaptables (variables pour les nœuds, interfaces, commandes)

### Sécurisation :

* Variables sensibles dans AWS Secrets Manager ou Vault
* Accès restreint via RBAC/ACL (API et K8s)

---

## 📦 Images Docker spécialisées (Catalogue réseau NetLab)

### 🔁 Routeurs / Protocoles de routage

| Image Docker  | Description                          | Notes                       |
| ------------- | ------------------------------------ | --------------------------- |
| frrouting/frr | Suite complète OSPF, BGP, RIP, IS-IS | Stable, bien maintenue      |
| cznic/bird    | Routeur léger BGP et OSPF            | Très utilisé dans les IXP   |
| osrg/gobgp    | Routeur BGP en Go                    | Léger, facile à script      |
| Quagga        | Ancien projet remplacé par FRR       | Obsolète mais parfois utile |

### 🔀 Switching & SDN

| Image Docker            | Description             | Notes                          |
| ----------------------- | ----------------------- | ------------------------------ |
| socketplane/openvswitch | Switch virtuel OpenFlow | Nécessite des scripts de setup |
| lagopus/lagopus         | Switch SDN user space   | Moins connu                    |
| Pantheon OVS (Custom)   | Version optimisée OVS   | Build manuel parfois requis    |

### 🔐 Firewall / IDS / VPN

| Image Docker               | Description         | Notes                       |
| -------------------------- | ------------------- | --------------------------- |
| strongswan/strongswan      | VPN IPsec IKEv2     | Stable                      |
| linuxserver/wireguard      | VPN rapide, moderne | Très facile à containeriser |
| Alpine + iptables/nftables | Firewall simple     | Pour simuler pfSense/IPFire |

### 🌐 Load Balancing / Proxy / DNS / DHCP

| Image Docker                    | Description            | Notes                       |
| ------------------------------- | ---------------------- | --------------------------- |
| haproxytech/haproxy-alpine      | Load balancer TCP/HTTP | Excellente intégration K8s  |
| nginx                           | Reverse proxy HTTP/S   | Peut simuler du web traffic |
| andyshinn/dnsmasq               | DNS/DHCP simplifié     | Idéal pour test locaux      |
| internetsystemsconsortium/bind9 | DNS complet            | Configuration avancée       |

### 🧪 Outils de test réseau

| Image Docker             | Description                  | Notes                        |
| ------------------------ | ---------------------------- | ---------------------------- |
| networkstatic/iperf3     | Test de bande passante       | Classique                    |
| praqma/network-multitool | ping, curl, netcat, ip, etc. | Utile pour tests automatisés |
| leodido/mtr              | Traceroute + stats           | Alternative à traceroute     |
| tcpdump                  | Sniffer réseau               | Intégrable en conteneur      |

### 🧱 OS réseau containerisables (niveau avancé)

| Projet              | Containerisable ?   | Notes                              |
| ------------------- | ------------------- | ---------------------------------- |
| VyOS                | Oui (custom build)  | À partir de QEMU ou image modifiée |
| MikroTik / RouterOS | Non officiellement  | Préférer VM                        |
| Juniper vSRX/vMX    | Non (VM uniquement) | Licence requise                    |
| Cumulus Linux       | VM uniquement       | Pour cas SDN pro                   |

---

### 🧠 Recommandations NetLab API :

* Utiliser **FRR** pour le routage dynamique
* **Open vSwitch** ou **Lagopus** pour switching/SDN
* **WireGuard** pour VPN
* **iptables/nftables** (dans Alpine) pour firewall léger
* Scripts ou containers de tests (iperf3, multitool, mtr, tcpdump)
* Créer un **Helm chart par rôle** : routeur, switch, client test, etc.
