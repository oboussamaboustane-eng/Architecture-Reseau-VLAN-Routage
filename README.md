# Architecture Réseau d'Entreprise : Switching, Routing & WAN

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue?logo=cisco&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success)

## Description du Projet
Ce projet, réalisé dans le cadre du module **Réseaux Informatiques**, consiste en la conception et le déploiement d'une infrastructure réseau complète simulant un siège social connecté à deux sites distants.

L'objectif est de démontrer la maîtrise des protocoles de **Commutation (Switching)**, de **Routage Inter-VLAN** et d'**Interconnexion WAN**.

* **Étudiant :** [Boustane Oussama](https://www.linkedin.com/in/oussama-boustane-22a990298/)
* **Année :** 2025/2026

---

## Topologie et Architecture

Le réseau est constitué de :
* **3 Routeurs Cisco 2811** (Zone WAN et Cœur de réseau).
* **2 Switchs Cisco 2960-24TT** (Zone LAN Access/Distribution).
* **8 Postes Clients** répartis sur différents VLANs et sites.

![Topologie du Réseau](topologie.png)

### Inventaire du Matériel
| Équipement | Modèle | Rôle Principal |
| :--- | :--- | :--- |
| **R1** | Cisco 2811 | Siège : Passerelle LAN (RoaS) & Hub WAN |
| **R2** | Cisco 2811 | Site Distant 1 (Liaison Série) |
| **R3** | Cisco 2811 | Site Distant 2 (Liaison Série) |
| **S1** | Cisco 2960 | Distribution Siège (EtherChannel) |
| **S2** | Cisco 2960 | Accès Siège (EtherChannel) |

### Plan d'Adressage IP Clé
| Périphérique | Interface | IP / Masque | Description |
| :--- | :--- | :--- | :--- |
| **R1** | Fa0/0.10 | 172.18.10.14 /28 | GW VLAN 10 (Utilisateurs) |
| | S0/3/0 | 10.0.30.177 /30 | Vers R2 |
| **R3** | Fa0/0 | 10.0.30.158 /27 | GW Site Distant 2 (PC8) |
| **S2** | Vlan60 | 172.18.60.2 /28 | IP de Management |

---

## Fonctionnalités Configurées

### 1. Commutation (Switching)
* **VLANs :** Segmentation du réseau en 5 VLANs (10, 20, 30, 50 Natif, 60 Admin).
* **EtherChannel (LACP) :** Agrégation de liens active entre S1 et S2.
* **Trunking (802.1Q) :** Transport des VLANs vers le routeur R1.

### 2. Routage (Routing)
* **Router-on-a-Stick :** Configuration de sous-interfaces sur R1 (Fa0/0.10, .20, etc.).
* **Routage WAN :** Liaisons séries point-à-point.
* **Routage Statique :** Configuration manuelle des routes pour l'interconnexion des 3 sites.

---

## Preuves de Fonctionnement

### Test de Connectivité WAN (Ping)
Le test ci-dessous démontre que les paquets traversent correctement le réseau local du siège, le routeur central (R1) pour atteindre le site distant R3.

![Test Ping](ping.png)

### Validation du Chemin (Traceroute)
On visualise ici les sauts (hops) : Passerelle R1 -> Interface WAN R3 -> PC Distant.

![Preuve Traceroute](tracert.png)

### Table de Routage (R2)
Validation des routes statiques (S) et de la route par défaut (S*) vers le siège.

![Preuve Routage](routage.png)

---

## Structure du Dépôt

* `projet_final.pkt` : Le fichier de simulation Packet Tracer (Source).
* `topologie.png` : Vue d'ensemble de l'architecture.
* `ping.png` : Preuve de connectivité.
* `tracert.png` : Preuve de routage.
* `routage.png` : Preuve de configuration routeur.

---
*Projet académique.*
