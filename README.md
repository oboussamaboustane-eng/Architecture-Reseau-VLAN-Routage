# 🚀 Conception & Déploiement d'une Infrastructure Réseau Multisites

![Cisco](https://img.shields.io/badge/Tech-Cisco_IOS-blue?style=for-the-badge&logo=cisco)
![Packet Tracer](https://img.shields.io/badge/Simulation-Packet_Tracer_8.2-success?style=for-the-badge)
![Status](https://img.shields.io/badge/Statut-Projet_Validé_✅-brightgreen?style=for-the-badge)

> **Auteur :** Oussama Boustane  
> **Contexte :** Projet Final - Module Réseaux Informatiques (Fès, 2026)  
> [![LinkedIn](https://img.shields.io/badge/Connect-LinkedIn-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/oussama-boustane-22a990298/)

---

## 📑 Sommaire
1. [Introduction et Enjeux](#1-introduction-et-enjeux)
2. [Architecture Technique](#2-architecture-technique)
3. [Preuves de Fonctionnement (Analyses)](#3-preuves-de-fonctionnement-analyses)
4. [Détails de Configuration](#4-détails-de-configuration)
5. [Installation](#5-installation)

---

## 1. Introduction et Enjeux

Ce projet vise à simuler le déploiement d'une infrastructure réseau robuste pour une entreprise disposant d'un **Siège Social** et de deux **Sites Distants**. L'objectif principal était de garantir la segmentation, la sécurité et la continuité de service.

### 🎯 Objectifs atteints :
* **Sécurité & Segmentation :** Isolation des flux via VLANs (802.1Q).
* **Performance :** Agrégation de liens (EtherChannel LACP) pour doubler la bande passante.
* **Interconnexion :** Routage WAN optimisé entre les sites distants.
* **Scalabilité :** Plan d'adressage VLSM permettant l'ajout futur d'hôtes.

---

## 2. Architecture Technique

### 🗺️ Topologie Globale
![Architecture Globale](topologie.png)

**Analyse de la topologie :**
Cette vue d'ensemble illustre la hiérarchie du réseau. Au centre gauche, le **Siège Social** utilise une architecture "Router-on-a-Stick" (R1) couplée à deux commutateurs de distribution (S1, S2) interconnectés. À droite, les **Sites Distants** (R2 et R3) sont reliés via des liaisons Séries (câbles rouges), formant une boucle de redondance partielle via le routage statique.

---

## 3. Preuves de Fonctionnement (Analyses)

Cette section valide techniquement le projet à travers des scénarios de tests réels.

### 📸 Preuve n°1 : Connectivité End-to-End (Ping)
![Ping Test](ping.png)
**Analyse technique :**
* **Source :** PC1 (Siège - VLAN 10 - `172.18.10.1`)
* **Destination :** PC8 (Site Distant R3 - `10.0.30.129`)
* **Résultat :** `Reply from... TTL=126`
* **Interprétation :** Ce succès prouve que le paquet a traversé **toutes** les couches du réseau : il a été tagué par le VLAN 10, routé par R1, encapsulé sur le WAN, reçu par R3 et livré au destinataire. Le TTL de 126 indique qu'il a traversé 2 routeurs (128 - 2).

<br>

### 📸 Preuve n°2 : Traçage de Route (Traceroute)
![Traceroute Test](tracert.png)
**Analyse technique :**
La commande `tracert` permet de visualiser les sauts (hops) effectués par le paquet :
1.  **Saut 1 (`172.18.10.14`) :** Le paquet atteint sa passerelle par défaut (Sous-interface R1 Fa0/0.10).
2.  **Saut 2 (`10.0.30.182`) :** R1 route le paquet via la liaison série vers l'interface WAN de R3.
3.  **Saut 3 (`10.0.30.129`) :** R3 délivre le paquet au PC8 dans le réseau local distant.
> **Conclusion :** Le routage est optimal et suit le chemin le plus court défini.

<br>

### 📸 Preuve n°3 : Table de Routage (Routeur de Bordure)
![Routing Table](routage.png)
**Analyse technique :**
L'extrait de la commande `show ip route` sur R2 montre :
* **`C` (Connected) :** Les réseaux directement attachés (LAN local et liaisons WAN).
* **`S*` (Static Default) :** La route par défaut `0.0.0.0/0` pointant vers le Siège (R1), essentielle pour l'accès Internet ou inter-sites non explicites.
* **`S` (Static) :** Une route spécifique vers le réseau `10.0.30.128/27` via `10.0.30.186`, assurant la connectivité avec le site voisin R3 sans repasser par le siège.

---

## 4. Détails de Configuration

### 📊 Plan d'Adressage (VLSM)
| Zone | VLAN / Lien | Adresse Réseau | Masque (CIDR) |
| :--- | :--- | :--- | :--- |
| **LAN Siège** | VLAN 10 (Utilisateurs) | `172.18.10.0` | `/28` |
| **LAN Siège** | VLAN 20 (Utilisateurs) | `172.18.20.0` | `/28` |
| **LAN Siège** | VLAN 50 (Natif) | `172.18.50.0` | `/28` |
| **LAN Siège** | VLAN 60 (Admin) | `172.18.60.0` | `/28` |
| **WAN** | Liaison R1-R3 | `10.0.30.180` | `/30` |

### 🛠️ Technologies Clés
* **EtherChannel :** `channel-group 1 mode active` (LACP) utilisé entre S1 et S2 pour la tolérance aux pannes.
* **Trunking :** `switchport mode trunk` avec filtrage des VLANs autorisés.
* **Encapsulation :** `encapsulation dot1Q [VLAN]` sur les sous-interfaces du routeur R1.

---

## 5. Installation

Pour tester cette simulation sur votre machine :

1.  **Télécharger :** Clonez ce dépôt ou téléchargez le fichier ZIP.
    ```bash
    git clone [https://github.com/VOTRE_NOM/Architecture-Reseau-VLAN-Routage.git](https://github.com/VOTRE_NOM/Architecture-Reseau-VLAN-Routage.git)
    ```
2.  **Ouvrir :** Lancez **Cisco Packet Tracer** (version 8.0+).
3.  **Charger :** Ouvrez le fichier `projet_final.pkt`.
4.  **Simuler :** Les PC sont pré-configurés. Vous pouvez relancer les tests de Ping immédiatement.

---
*Fait avec ❤️ et rigueur par Oussama Boustane.*
