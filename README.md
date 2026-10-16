# Projets Cisco Packet Tracer

Ce dépôt rassemble deux projets de conception et de configuration d'infrastructures réseau réalisés sous **Cisco Packet Tracer** dans le cadre de ma Licence Informatique (Université de Tours). Chaque projet est accompagné des configurations CLI de chaque équipement.

| Projet | Description | Technologies principales |
|---|---|---|
| [Fac Grandmont](#fac-grandmont) | Reconstitution du réseau de mon propre campus universitaire | VLAN, switch L3, DHCP, ACL, port security, NAT, VoIP, Wi-Fi |
| [Financial Network](#financial-network) | Réseau d'entreprise du secteur financier sur deux sites | VLAN, OSPF, double opérateur, NAT/PAT, VPN IPsec, VoIP, SSH |

## Sommaire

1. [Fac Grandmont](#fac-grandmont)
2. [Financial Network](#financial-network)
3. [Structure du dépôt](#structure-du-dépôt)
4. [Utilisation](#utilisation)
5. [Compétences mises en œuvre](#compétences-mises-en-œuvre)
6. [Auteur](#auteur)

---

## Fac Grandmont

Reconstitution réaliste du réseau du campus Grandmont, à partir du plan du site. La topologie en **étoile étendue** s'articule autour d'un switch de niveau 3 installé dans le bâtiment D (antenne technique), qui interconnecte les switchs d'accès de chaque bâtiment.

### Objectifs

- Reproduire une logique réseau cohérente avec l'organisation réelle du campus (bâtiments H, L, G, D, bibliothèque).
- Segmenter le réseau par VLAN selon l'usage (administration, technique, téléphonie, étudiants).
- Fournir une sortie Internet et une liaison vers l'IUT GEII.
- Sécuriser le réseau : n'accorder l'accès aux ressources sensibles qu'au personnel.

### Architecture

<img width="1447" height="685" alt="image" src="https://github.com/user-attachments/assets/70564846-6b7e-46f0-a6f1-403f5d70416e" />


| Équipement | Rôle |
|---|---|
| `COEUR_BAT_D` (switch L3) | Cœur de réseau : routage inter-VLAN, serveur DHCP, ACL, route par défaut |
| `ACCES_BAT_L` | Switch d'accès du bâtiment L (VLAN 60) |
| `ACCES_BAT_H` | Switch d'accès du bâtiment H (VLAN 50, imprimante en VLAN 10) |
| `ACCES_BAT_G` | Switch d'accès du bâtiment G (VLAN 10 et voix VLAN 40) |
| `ROUTER_INTERNET` | Sortie Internet, NAT, liaison vers le serveur DNS Google (8.8.8.8) et l'IUT GEII |
| IUT GEII | Routeur relié par câble série, avec un switch et un poste pour tester l'envoi et la réception de paquets |
| Wi-Fi bibliothèque | Accès sans fil pour les étudiants (test depuis le portable, onglet *Desktop > PC Wireless*) |

Tous les switchs d'accès sont reliés au switch L3 par des liens **trunk** (`switchport mode trunk`).

### Plan d'adressage

| VLAN | Nom | Réseau | Passerelle |
|---|---|---|---|
| 10 | ADMIN | 192.168.10.0/24 | 192.168.10.254 |
| 20 | TECHNIQUE | 192.168.20.0/24 | 192.168.20.254 |
| 40 | VOIP | 192.168.40.0/24 | 192.168.40.254 |
| 50 | ÉTUDIANTS (bibliothèque) | 192.168.50.0/24 | 192.168.50.254 |
| 60 | ÉTUDIANTS (bâtiment L) | 192.168.60.0/24 | 192.168.60.254 |

| Liaison | Réseau |
|---|---|
| `COEUR_BAT_D` ↔ `ROUTER_INTERNET` | 192.168.100.0/24 (.1 et .2) |
| `ROUTER_INTERNET` ↔ serveur DNS Google | 8.8.8.0/24 (routeur en 8.8.8.254) |

### Fonctionnalités

- **VLAN et trunk :** un VLAN par usage et par bâtiment, interconnectés par le switch L3.
- **Routage inter-VLAN** par interfaces SVI sur `COEUR_BAT_D` (`ip routing`), avec route par défaut vers le routeur Internet.
- **DHCP** centralisé sur le switch L3 : un pool par VLAN, adresses des passerelles exclues, option 150 pour la téléphonie.
- **NAT avec surcharge** sur `ROUTER_INTERNET` : les appareils sortent sur Internet avec une adresse publique du pool 197.44.173.1 à 197.44.173.30.
- **Téléphonie IP :** VLAN voix dédié (40), avec un poste de téléphonie pour le secrétariat.
- **Imprimante** raccordée au VLAN ADMIN (10).
- **Wi-Fi** pour la bibliothèque.

### Sécurité

| Mesure | Détail |
|---|---|
| ACL 100 | Interdit aux VLAN étudiants (50 et 60) l'accès aux VLAN ADMIN (10) et VOIP (40), appliquée en entrée sur les SVI 50 et 60 |
| Port security | Une seule adresse MAC par port (`maximum 1`, apprentissage `sticky`, violation en `shutdown`) pour empêcher le branchement d'un poste personnel, le scan du réseau ou une attaque par saturation de la table MAC (MAC flooding) |
| NAT | Masque le plan d'adressage interne lors des sorties vers Internet |

### Difficultés rencontrées et enseignements

L'interconnexion de l'ensemble des équipements et le repérage dans la configuration après une erreur ont été les principales difficultés. Ce premier projet de grande ampleur a permis de comprendre concrètement les mécanismes qui assurent la communication entre les postes d'un réseau, de façon sécurisée.

### Pistes d'amélioration

- Étendre les ACL au VLAN TECHNIQUE (20).
- Sécuriser l'administration des équipements (SSH, mots de passe chiffrés, accès restreint).
- Généraliser le port security à tous les switchs d'accès.

---

## Financial Network

Simulation du réseau d'une entreprise du secteur financier : siège segmenté en VLAN par département, routage dynamique OSPF, double accès Internet, VPN IPsec site à site, téléphonie IP et durcissement des équipements.

### Objectifs

- Segmenter le réseau du siège par département et assurer le routage inter-VLAN.
- Interconnecter le siège et le site serveurs par un routage dynamique redondant (OSPF).
- Fournir un accès Internet via deux opérateurs et la traduction d'adresses (NAT).
- Protéger les échanges entre les sites par un tunnel VPN IPsec.
- Déployer un service de téléphonie IP et sécuriser l'administration des équipements.

### Architecture

<img width="1372" height="957" alt="image" src="https://github.com/user-attachments/assets/e211b775-074e-4c98-9086-ba6096cd3d63" />


| Zone | Équipements |
|---|---|
| Accès (départements) | 5 switchs Catalyst 2960 : `HR-SW`, `CS-SW`, `MK-SW`, `LM-SW`, `IT-SW` |
| Distribution / cœur | 2 switchs de niveau 3 Catalyst 3650 : `HQ-MSLW1`, `HQ-MSLW2` |
| Périphérie du siège | Routeur 2911 `HQ-Router` (NAT, VPN, sortie WAN) |
| Téléphonie | Routeur 2811 `HQ VOIP` (Cisco Unified Communications Manager Express) |
| Site serveurs | Routeur 2911 `Server-Side-Router` et switch 2960 `SERVER-SIDE-SW` |
| Opérateurs | Routeurs 2911 `Safaricorm` et `JTL-ISP` |

Le siège et le site serveurs sont reliés par deux opérateurs distincts, ce qui assure une redondance des chemins grâce à OSPF.

### Plan d'adressage

**VLAN du siège**

| VLAN | Nom | Réseau | Passerelle (`HQ-MSLW1` / `HQ-MSLW2`) |
|---|---|---|---|
| 10 | HR | 192.168.20.0/26 | 192.168.20.1 / 192.168.20.2 |
| 20 | CS | 192.168.20.64/26 | 192.168.20.65 / 192.168.20.66 |
| 30 | MK | 192.168.20.128/26 | 192.168.20.129 / 192.168.20.130 |
| 40 | LM | 192.168.20.192/27 | 192.168.20.193 / 192.168.20.194 |
| 50 | IT | 192.168.20.224/27 | 192.168.20.225 / 192.168.20.226 |
| 120 | VOICE | 10.10.10.0/24 | 10.10.10.1 (routeur `HQ VOIP`) |

**Liaisons point à point et site serveurs**

| Liaison | Réseau |
|---|---|
| `HQ-MSLW1` ↔ `HQ-Router` | 192.168.21.16/30 |
| `HQ-MSLW2` ↔ `HQ-Router` | 192.168.21.20/30 |
| `HQ-Router` ↔ `Safaricorm` | 190.200.100.0/30 |
| `HQ-Router` ↔ `JTL-ISP` | 190.200.100.4/30 |
| `Server-Side-Router` ↔ `Safaricorm` | 190.200.100.8/30 |
| `Server-Side-Router` ↔ `JTL-ISP` | 190.200.100.12/30 |
| LAN du site serveurs | 192.168.21.0/28 (passerelle 192.168.21.1, serveur DHCP 192.168.21.5) |

### Fonctionnalités

**Couche 2**
- VLAN de données par département et VLAN de voix commun (120) sur chaque switch d'accès.
- Liens trunk 802.1Q entre switchs d'accès et switchs de niveau 3.

**Couche 3 et routage**
- Routage inter-VLAN par interfaces SVI sur `HQ-MSLW1` et `HQ-MSLW2`.
- OSPF (processus 10, aire 0) sur l'ensemble des routeurs et switchs de niveau 3.
- Relais DHCP (`ip helper-address`) vers le serveur 192.168.21.5.

**Accès Internet et interconnexion des sites**
- NAT avec surcharge (PAT) sur `HQ-Router` pour le réseau 192.168.20.0/24.
- Tunnel VPN IPsec site à site (ISAKMP, AES-256, ESP AES/SHA-HMAC) pour le trafic entre 192.168.20.0/24 et le réseau serveurs.

**Téléphonie IP**
- Cisco CME sur routeur 2811 : sous-interface 802.1Q (VLAN 120), pool DHCP dédié (option 150), 20 téléphones maximum, numéros 401 à 410.

### Sécurité

| Mesure | Équipements |
|---|---|
| Mots de passe chiffrés (`service password-encryption`) | Tous |
| Bannière d'avertissement (`banner motd`) | Tous |
| Résolution DNS désactivée (`no ip domain lookup`) | Tous |
| Accès SSH v2 uniquement (authentification locale) | Switchs de niveau 3, routeurs |
| Accès distant limité au VLAN IT par ACL (192.168.20.224/27) | Switchs de niveau 3, routeurs |
| Port security (1 adresse MAC, sticky, mode shutdown) | Ports d'accès du site serveurs |
| Ports inutilisés désactivés et placés dans le VLAN 99 | Site serveurs |
| VPN IPsec entre le siège et le site serveurs | `HQ-Router` |

### Pistes d'amélioration

- Remplacer `enable password` par `enable secret`.
- Ajouter un protocole de redondance de passerelle (HSRP ou VRRP) entre `HQ-MSLW1` et `HQ-MSLW2`.
- Étendre SSH et le relais DHCP à tous les équipements concernés.
- Ajouter un serveur de journalisation (syslog) et une supervision SNMP.

---

## Structure du dépôt

```
.
├── README.md
├── fac-grandmont/
│   ├── fac-grandmont.pkt
│   └── CLI-FAC-GRANDMONT-CISCO-PACKET-TRACER.md
├── financial-network/
│   ├── financial-network.pkt
│   └── CLI-PROJET-FINANCIAL-NETWORK.md
└── docs/
    └── financial-network-topologie.png
```

## Utilisation

1. Ouvrir le fichier `.pkt` du projet avec Cisco Packet Tracer.
2. Pour reproduire une configuration, appliquer les commandes du fichier CLI correspondant équipement par équipement.

Commandes de vérification courantes :

```
show vlan brief                  # VLAN et affectation des ports
show ip route                    # table de routage
show ip ospf neighbor            # voisinages OSPF (Financial Network)
show ip nat translations         # traductions NAT
show crypto isakmp sa            # état du tunnel IPsec (Financial Network)
show port-security interface fa0/2
```

Tests de connectivité : ping entre VLAN, vers Internet et entre sites, vérification des refus par ACL, connexion SSH depuis le VLAN autorisé.

## Compétences mises en œuvre

VLAN et trunking 802.1Q · routage inter-VLAN · OSPF · routage statique · NAT/PAT · ACL · VPN IPsec · DHCP et relais DHCP · VoIP (CME) · SSH · port security · durcissement d'équipements Cisco

## Auteur

**Vincent Deschamps** – Étudiant en Licence Informatique, Université de Tours
