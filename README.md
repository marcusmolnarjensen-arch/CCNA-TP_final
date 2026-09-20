# TP Final - Modules 1 à 6

## Déploiement réseau évolutif d'un site d'entreprise

![Statut](https://img.shields.io/badge/statut-phases_1_a_3-yellow) ![Plateforme](https://img.shields.io/badge/plateforme-PNetLab-blue) ![OSPF](https://img.shields.io/badge/OSPF-valide-brightgreen) ![Securite](https://img.shields.io/badge/securite_L2%2FSSH-valide-brightgreen) ![ACL](https://img.shields.io/badge/ACL-valide-brightgreen) ![NAT](https://img.shields.io/badge/NAT-non_traite-lightgrey) ![Audit](https://img.shields.io/badge/audit_final-non_traite-lightgrey)

**Satom IT & Learning Solutions · Geneva Institute of Technology (GIT)**

| | |
|---|---|
| **Étudiant** | Marcus - Formation CFC + Bachelor, IT Infrastructure & Networking |
| **Plateforme** | PNetLab |
| **Périmètre réalisé** | Phases 1 à 3 |
| **Non traité** | Phase 4 (NAT), Phase 5 (Audit final) - justification en section 6 |

### Sommaire

1. [Topologie du lab](#1-topologie-du-lab)
2. [Phase 1 - Routage dynamique OSPF](#2-phase-1--routage-dynamique-ospf)
3. [Phase 2 - Sécurisation des accès et de la couche 2](#3-phase-2--sécurisation-des-accès-et-de-la-couche-2)
4. [Phase 3 - Filtrage réseau par ACL](#4-phase-3--filtrage-réseau-par-acl)
5. [Sauvegarde des configurations](#5-sauvegarde-des-configurations)
6. [Phases 4 et 5 - Justification](#6-phases-4-et-5--non-traitées)

---

## 1. Topologie du lab

Topologie physique utilisée sur PNetLab, adaptée du plan de référence du TP : câblage point-à-point R1↔R2 et R1↔R3, sans switch intermédiaire sur le backbone.

**CAPTURE A INSERER ICI**
Vue complète de la topologie PNetLab (R1, R2, R3, SW1, SW2, VPC7, VPC, VPC-Admin, cloud Net)

### Correspondance des rôles

| Équipement (lab) | Rôle (cahier des charges) | Router-ID / IP |
|---|---|---|
| **R1** | Routeur cœur - relie R2 et R3 | Lo0 : 1.1.1.1 |
| **R2** | R-FORMATION - VLAN 10 (Salle Info) + VLAN 99 (Invités) | Lo0 : 2.2.2.2 |
| **R3** | R-ADMIN - VLAN 20 (Administration) | Lo0 : 3.3.3.3 |
| **SW1** | Switch d'accès aile Formation (sous R2) | - |
| **SW2** | Switch d'accès aile Administration (sous R3) | - |
| **VPC7** | PC-SALLE-INFO | 192.168.10.10/24 |
| **VPC** | PC-INVITE | 192.168.99.10/24 |
| **VPC-Admin** (VPC6) | PC-ADMIN (seul poste autorisé SSH) | 192.168.20.10/24 |

---

## 2. Phase 1 - Routage dynamique OSPF

> OSPFv2, processus 1, aire 0, activé sur R1, R2 et R3. Router-ID explicite via interface loopback sur chaque routeur. Toutes les interfaces orientées utilisateur sont passives ; seules les interfaces backbone (R1↔R2, R1↔R3) forment des adjacences. Authentification MD5 activée sur les deux liens backbone.

### 2.1 Adjacences OSPF

**CAPTURE A INSERER ICI**
`show ip ospf neighbor` sur R1 - 2 voisins attendus, état FULL

**CAPTURE A INSERER ICI**
`show ip ospf neighbor` sur R2 - 1 voisin attendu, état FULL

**CAPTURE A INSERER ICI**
`show ip ospf neighbor` sur R3 - 1 voisin attendu, état FULL

### 2.2 Routes apprises dynamiquement

**CAPTURE A INSERER ICI**
`show ip route ospf` sur R2 - doit voir les réseaux de R3

**CAPTURE A INSERER ICI**
`show ip route ospf` sur R3 - doit voir les réseaux de R2

### 2.3 Interfaces passives

**CAPTURE A INSERER ICI**
`show ip protocols | section Passive` sur R2 et R3

### 2.4 Authentification MD5

**CAPTURE A INSERER ICI**
`show ip ospf interface FastEthernet0/0 | include Auth` sur R1

### 2.5 Test de connectivité bout-en-bout

Ping depuis VPC7 (192.168.10.10, aile Formation) vers 192.168.20.1 (R3, aile Administration) : le trafic traverse SW1 → R2 → R1 → R3, preuve que le routage OSPF fonctionne sur l'ensemble du site.

**CAPTURE A INSERER ICI**
Ping VPC7 → 192.168.20.1 réussi (ttl=253, 2 sauts)

**CAPTURE A INSERER ICI**
Ping VPC-Admin → 192.168.20.1 réussi (ttl=255, réseau local)

---

## 3. Phase 2 - Sécurisation des accès et de la couche 2

> Accès administratif restreint au SSHv2 avec compte nominatif sur les trois routeurs (Telnet désactivé). Port security activé sur les ports d'accès de SW1 et SW2 (1 MAC max, apprentissage sticky, violation → shutdown). DHCP snooping activé sur le VLAN 10 de SW1, port montant vers R2 déclaré de confiance.

### 3.1 SSH actif, Telnet désactivé

**CAPTURE A INSERER ICI**
`show ip ssh` sur R1 - SSH version 2.0 activé

### 3.2 Port security - cycle complet

Démonstration du cycle complet : violation de sécurité déclenchée → port passé en err-disabled → relève manuelle du port.

**CAPTURE A INSERER ICI**
`show port-security interface e0/1` sur SW1 - état Secure-up, Maximum 1, MAC sticky apprise

**CAPTURE A INSERER ICI**
Log de violation déclenchée (`%PORT_SECURITY-2-PSECURE_VIOLATION`) et passage en err-disabled

**CAPTURE A INSERER ICI**
`show interfaces e0/1 status` après relève (shutdown / no shutdown) - retour à l'état connected

### 3.3 DHCP Snooping

**CAPTURE A INSERER ICI**
`show ip dhcp snooping` sur SW1 - VLAN 10 surveillé, port montant trusted

**CAPTURE A INSERER ICI**
`show ip dhcp snooping binding` sur SW1

### 3.4 Procédure de relève d'un port err-disabled

1. Identifier le port concerné : `show interfaces status | include err-disabled`
2. Retirer ou identifier l'appareil non autorisé à l'origine de la violation
3. Relever le port : `interface e0/x` → `shutdown` → `no shutdown`
4. Vérifier le retour à l'état Secure-up : `show port-security interface e0/x`

---

## 4. Phase 3 - Filtrage réseau par ACL

> Trois ACL nommées mises en place : `ADMIN-ONLY` (standard, restreint l'accès SSH au seul poste 192.168.20.10, appliquée sur les lignes VTY des 3 routeurs via `access-class`), `INVITES-ONLY` (étendue, restreint le VLAN 99 au Web et au DNS, appliquée sur la sous-interface G0/0.99 de R2 en entrée), et une ligne de blocage RDP explicite.

### 4.1 Calcul des wildcard masks utilisés

| Réseau / hôte | Masque | Wildcard | Calcul |
|---|---|---|---|
| 192.168.99.0/24 (Invités) | 255.255.255.0 | 0.0.0.255 | 255.255.255.255 − 255.255.255.0 |
| 192.168.20.10 (PC-Admin, hôte unique) | 255.255.255.255 (/32) | 0.0.0.0 (mot-clé `host`) | Inversion bit à bit d'un masque /32 |

### 4.2 Justification du placement des ACL

- **ADMIN-ONLY** est appliquée sur les lignes VTY (`access-class`) car elle filtre l'accès administratif au routeur lui-même, pas du trafic qui transite.
- **INVITES-ONLY** est posée sur la sous-interface G0/0.99 de R2, au plus près de la source du VLAN Invités : le trafic non autorisé est rejeté dès son entrée dans le réseau, sans consommer de ressources sur les liens backbone.

### 4.3 Test - accès SSH restreint

Tentative de connexion SSH vers R1 depuis une adresse source différente de 192.168.20.10 (test réalisé depuis R2, faute de client SSH natif sur l'émulateur VPCS utilisé pour les postes) :

**CAPTURE A INSERER ICI**
Connexion SSH refusée depuis R2 (`% Connection refused by remote host`)

**CAPTURE A INSERER ICI**
Log `%SEC-6-IPACCESSLOGNP` confirmant le rejet par la ligne deny de l'ACL ADMIN-ONLY

**CAPTURE A INSERER ICI**
`show access-lists ADMIN-ONLY` - compteur de matches sur la ligne deny

### 4.4 Test - isolation du VLAN Invités

**CAPTURE A INSERER ICI**
Ping VPC (Invités) → 192.168.20.1 refusé (ICMP administratively prohibited)

---

## 5. Sauvegarde des configurations

`write memory` exécuté sur les 5 équipements (R1, R2, R3, SW1, SW2) après validation de chaque phase.

**CAPTURE A INSERER ICI**
`show running-config` de R1

**CAPTURE A INSERER ICI**
`show running-config` de R2

**CAPTURE A INSERER ICI**
`show running-config` de R3

**CAPTURE A INSERER ICI**
`show running-config` de SW1

**CAPTURE A INSERER ICI**
`show running-config` de SW2

---

## 6. Phases 4 et 5 - Non traitées

Par choix assumé, seules les Phases 1 à 3 ont été réalisées.

**Phase 4 (NAT)** - non réalisée. Un doute est apparu sur la cohérence entre l'interface reliée au cloud « Net » et l'environnement réseau réel de la plateforme PNetLab ; plutôt que de risquer d'altérer une configuration fonctionnelle et validée (Phases 1 à 3), le choix a été fait de ne pas toucher à cette partie.

**Phase 5 (Audit final / incident imposé)** - non réalisée. Cette phase implique par nature de modifier volontairement une configuration qui fonctionne (désynchronisation MD5, ACL mal orientée, ou perte de configuration après reboot). Au vu du temps déjà investi et de l'heure tardive, le risque de ne pas pouvoir revenir à un état stable avant le rendu a été jugé trop élevé.
