# 🖥️ Homelab Proxmox — Infrastructure virtualisée, Active Directory & Supervision

**Projet personnel** · Auteur : [Mehdi Lemaire](https://github.com/MehdiUI) · Dernière mise à jour : Novembre 2026 · **Statut : ✅ Terminé**

---

## 🎯 Objectif du projet

Ce homelab reproduit, sur une infrastructure virtualisée, les composants essentiels d'un système d'information d'entreprise :

- Un **hyperviseur** (Proxmox VE) pour héberger plusieurs machines virtuelles
- Un **contrôleur de domaine** Windows Server avec Active Directory, DNS et DHCP
- Un **serveur Linux** (Ubuntu Server) pour les services applicatifs
- Un **pare-feu** (pfSense) avec segmentation réseau (VLAN)
- Une **supervision** centralisée (Zabbix)
- L'**analyse de trafic** réseau (Wireshark)

**Compétences mises en œuvre** : administration systèmes, gestion d'un annuaire, segmentation réseau, supervision, diagnostic d'incidents.

---

## 🏗️ Architecture du lab

```
                    ┌─────────────────────────────┐
                    │        PROXMOX VE           │
                    │      (hyperviseur)          │
                    │   192.168.1.10 (mgmt)       │
                    └──────────────┬──────────────┘
                                   │
              ┌────────────────────┼────────────────────┐
              │                    │                    │
       ┌──────▼──────┐      ┌──────▼──────┐      ┌──────▼──────┐
       │  pfSense    │      │ Win Server  │      │ Ubuntu Srv  │
       │  (firewall) │      │  (AD/DNS/   │      │  (services) │
       │             │      │   DHCP)     │      │             │
       └──────┬──────┘      └─────────────┘      └─────────────┘
              │
       ┌──────▼──────┐
       │   Zabbix    │
       │(supervision)│
       └─────────────┘
```

**Plan d'adressage** :

| VM | Rôle | IP | VLAN |
|---|---|---|---|
| Proxmox | Hyperviseur | 192.168.1.10 | — |
| pfSense | Pare-feu | 192.168.1.1 (LAN) | — |
| DC01 (Windows Server) | AD / DNS / DHCP | 192.168.10.10 | 10 |
| SRV-Ubuntu | Services Linux | 192.168.10.20 | 10 |
| Zabbix | Supervision | 192.168.20.10 | 20 |

---

## 🧱 Stack technique

| Composant | Rôle | Version |
|---|---|---|
| **Proxmox VE** | Hyperviseur | 8.x |
| **Windows Server** | Active Directory, DNS, DHCP | 2022 |
| **Ubuntu Server** | Services Linux | 22.04 LTS |
| **pfSense** | Pare-feu, routage, VLAN | 2.7.x |
| **Zabbix** | Supervision | 7.x |
| **Wireshark** | Analyse de trafic | dernière version |
| **VirtualBox / machine physique** | Hôte de Proxmox | — |

---

## ✅ Avancement du projet

### Couche 1 — Fondations (terminée)
- [x] Installation de Proxmox VE (dans VirtualBox / sur machine dédiée)
- [x] Configuration réseau de l'hyperviseur
- [x] Création d'une VM **Ubuntu Server** (services de base, SSH)
- [x] Création d'une VM **Windows Server**
- [x] Installation et configuration d'**Active Directory**
- [x] Création d'utilisateurs, groupes et unités d'organisation (OU)
- [x] Rejointure d'un poste client au domaine
- [x] Documentation et captures d'écran

### Couche 2 — Services Windows (terminée)
- [x] Configuration **DNS** sur le contrôleur de domaine (zones directe et inverse)
- [x] Configuration **DHCP** avec plage d'adresses et réservations
- [x] Gestion des **droits sur les partages de fichiers** (ACL)
- [x] Création de **GPO** (stratégies de groupe) simples
- [x] Intégration d'un serveur Linux au domaine (Samba / Kerberos)

### Couche 3 — Réseau et sécurité (terminée)
- [x] Déploiement d'une VM **pfSense**
- [x] Configuration des interfaces WAN / LAN
- [x] Création de **VLAN** (services, supervision, administration)
- [x] Règles de filtrage **pare-feu** inter-VLAN
- [x] Mise en place d'un **VPN** (WireGuard)
- [x] Test des règles avec **Wireshark**

### Couche 4 — Supervision et analyse (terminée)
- [x] Déploiement de **Zabbix** (serveur + agent)
- [x] Ajout des hôtes (Windows Server, Ubuntu, pfSense)
- [x] Configuration d'alertes (CPU, RAM, disponibilité)
- [x] Création de **dashboards** personnalisés
- [x] Analyse de trafic avec **Wireshark** et filtres ciblés
- [x] Intégration de logs vers un SIEM (Wazuh) — *bonus*

> ✅ **Projet terminé en novembre 2026.** Les quatre couches sont opérationnelles et documentées.

---

## 🔧 Détail des composants

### 1. Proxmox VE
- Installation sur machine physique (hôte dédié)
- Accès à l'interface web : `https://192.168.1.10:8006`
- Bridges réseau configurés pour isoler les VLAN
- Snapshots réguliers des VM

### 2. Windows Server — Contrôleur de domaine
- Rôle **AD DS** installé
- Domaine : `lab.local`
- **DNS** : zone directe et inverse configurées
- **DHCP** : plage `192.168.10.100 – 192.168.10.200`
- **GPO** : stratégies de mot de passe, restriction d'accès, mappage de lecteurs
- Utilisateurs, groupes et OU créés pour simuler une petite entreprise

### 3. Ubuntu Server
- Services installés : **SSH**, **Nginx** (page de test), **rsyslog**
- Configuration réseau statique
- Utilisateur dédié + authentification par clé SSH
- Intégration au domaine via Samba / Kerberos

### 4. pfSense
- Interfaces WAN (bridge) et LAN (VLAN)
- Règles de filtrage :
  - VLAN 10 (services) → accès limité au LAN
  - VLAN 20 (supervision) → accès restreint
- VPN **WireGuard** configuré pour accès distant
- NAT configuré pour les tests

### 5. Zabbix
- Serveur Zabbix sur Ubuntu Server
- Agents installés sur Windows Server, Ubuntu et pfSense
- Alertes configurées sur :
  - Disponibilité (ping)
  - Utilisation CPU / RAM
  - Espace disque
- Dashboards personnalisés par hôte et par service

---

## 🧪 Scénarios testés

| Scénario | Résultat |
|---|---|
| Connexion d'un utilisateur AD au domaine | ✅ |
| Attribution d'une adresse IP par DHCP | ✅ |
| Accès à un partage de fichiers selon les droits AD | ✅ |
| Application d'une GPO sur un utilisateur / poste | ✅ |
| Ping entre VLAN 10 et VLAN 20 bloqué par pfSense | ✅ |
| Connexion VPN WireGuard depuis l'extérieur | ✅ |
| Alerte Zabbix déclenchée sur arrêt d'une VM | ✅ |
| Analyse d'un flux avec Wireshark (filtre HTTP) | ✅ |
| Centralisation de logs vers Wazuh | ✅ |

---

## 🚀 Pistes d'évolution (post-projet)

Ces évolutions ne font pas partie du périmètre initial, mais constituent des prolongements naturels :

- [ ] Ajouter un second contrôleur de domaine (redondance AD)
- [ ] Automatiser le déploiement de VM avec **Terraform** / **Ansible**
- [ ] Tester un scénario de **restauration de sauvegarde** (Veeam ou snapshot Proxmox)
- [ ] Documenter une procédure de **durcissement** (hardening) des serveurs
- [ ] Étendre la supervision à un cluster Proxmox multi-nœuds

---

## 📸 Captures d'écran

Les captures suivantes sont disponibles dans le dossier `captures/` :

- `captures/proxmox-dashboard.png` — Vue d'ensemble des VM
- `captures/ad-users.png` — Utilisateurs et groupes Active Directory
- `captures/gpo-config.png` — Configuration d'une GPO
- `captures/pfsense-rules.png` — Règles de filtrage pfSense
- `captures/wireguard-vpn.png` — Configuration VPN WireGuard
- `captures/zabbix-dashboard.png` — Dashboard de supervision
- `captures/wireshark-capture.png` — Analyse de trafic Wireshark

---

## 📚 Ce que j'ai appris

- **Proxmox** : création et gestion de VM, bridges réseau, snapshots, ressources
- **Active Directory** : structure d'un domaine, utilisateurs, groupes, OU, GPO, droits sur fichiers
- **DNS / DHCP** : configuration en environnement Windows Server, zones directe et inverse
- **Segmentation réseau** : création de VLAN et isolation via pfSense, règles de filtrage
- **VPN** : configuration WireGuard pour accès distant sécurisé
- **Supervision** : installation et configuration de Zabbix, création d'alertes et de dashboards
- **Analyse réseau** : utilisation de Wireshark pour diagnostiquer un flux
- **Centralisation de logs** : intégration vers Wazuh (SIEM)
- **Documentation** : rigueur dans la rédaction de procédures et de schémas d'architecture

---

## 🔗 Liens utiles

- [Proxmox VE Documentation](https://pve.proxmox.com/pve-docs/)
- [Microsoft Learn – Active Directory](https://learn.microsoft.com/fr-fr/windows-server/identity/active-directory-domain-services)
- [pfSense Documentation](https://docs.netgate.com/pfsense/en/latest/)
- [Zabbix Documentation](https://www.zabbix.com/documentation/current/en/manual)
- [Wireshark User's Guide](https://www.wireshark.org/docs/wsug_html_chunked/)
- [WireGuard Documentation](https://www.wireguard.com/)

---

## 📬 Contact

- **GitHub** : [github.com/MehdiUI](https://github.com/MehdiUI)
- **LinkedIn** : [linkedin.com/in/mehdi-lemaire](https://www.linkedin.com/in/mehdi-lemaire)
- **Email** : mehdi.lemaire.pro@gmail.com

---

> Projet réalisé dans le cadre de ma recherche d'alternance en **Licence Cybersécurité (Ynov Campus)** — objectif : consolider mes compétences en administration systèmes et réseaux, supervision et sécurité.
