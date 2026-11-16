# 🖥️ Homelab Proxmox — Infrastructure virtualisée, Active Directory & Supervision

**Projet personnel** · Auteur : [Mehdi Lemaire](https://github.com/MehdiUI) · Dernière mise à jour : Novembre 2026

---

## 🎯 Objectif du projet

Ce homelab a pour but de reproduire, sur une infrastructure virtualisée, les composants essentiels d'un système d'information d'entreprise :

- Un **hyperviseur** (Proxmox VE) pour héberger plusieurs machines virtuelles
- Un **contrôleur de domaine** Windows Server avec Active Directory, DNS et DHCP
- Un **serveur Linux** (Ubuntu Server) pour les services applicatifs
- Un **pare-feu** (pfSense) avec segmentation réseau (VLAN)
- Une **supervision** centralisée (Zabbix)
- L'**analyse de trafic** réseau (Wireshark)

**Compétences visées** : administration systèmes, gestion d'un annuaire, segmentation réseau, supervision, diagnostic d'incidents.

---

## 🏗️ Architecture du lab
