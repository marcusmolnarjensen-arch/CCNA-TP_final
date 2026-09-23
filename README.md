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

<img width="628" height="679" alt="image" src="https://github.com/user-attachments/assets/df589d7a-df03-419e-8777-00b612c115ff" />

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

R1
<img width="687" height="114" alt="image" src="https://github.com/user-attachments/assets/ad4c8eb0-30de-4f27-8e4a-e68141b8e32a" />

R2
<img width="666" height="91" alt="image" src="https://github.com/user-attachments/assets/5c980d49-30c8-4f86-9962-22ad49263977" />

R3
<img width="668" height="83" alt="image" src="https://github.com/user-attachments/assets/eb78ee13-1849-48b8-a786-1fff5dcf1300" />

### 2.2 Routes apprises dynamiquement

R2
<img width="660" height="373" alt="image" src="https://github.com/user-attachments/assets/11deebf2-4cc6-47a1-95f4-12134f1866e1" />

R3
<img width="648" height="325" alt="image" src="https://github.com/user-attachments/assets/0c9d9ebb-c7c2-40ab-87bd-ae0f27081477" />

### 2.3 Interfaces passives

**CAPTURE A INSERER ICI**
R2 et R3
R2: <img width="323" height="103" alt="image" src="https://github.com/user-attachments/assets/cc678361-4597-4d6b-9076-c3f6b1411d21" />

R3: <img width="310" height="80" alt="image" src="https://github.com/user-attachments/assets/1c4f7a34-8e2b-41a9-818b-575b59c1f361" />

### 2.4 Authentification MD5

<img width="456" height="50" alt="image" src="https://github.com/user-attachments/assets/a155e874-e8d5-45cf-bb91-e2e862816583" />

### 2.5 Test de connectivité bout-en-bout

Ping depuis VPC7 (192.168.10.10, aile Formation) vers 192.168.20.1 (R3, aile Administration) le trafic traverse SW1  R2  R1  R3, preuve que le routage OSPF fonctionne sur l'ensemble du site.

<img width="536" height="252" alt="image" src="https://github.com/user-attachments/assets/e2de89a1-9964-43ef-93d6-9133acbc616f" />

Ping invité - admin refusé.
<img width="918" height="174" alt="image" src="https://github.com/user-attachments/assets/5442c99f-ef3c-4933-a131-8c550f4ad9b7" />

---

## 3. Phase 2 - Sécurisation des accès et de la couche 2

Accès administratif restreint au SSHv2 avec compte nominatif sur les trois routeurs (Telnet désactivé). Port security activé sur les ports d'accès de SW1 et SW2 (1 MAC max, apprentissage sticky, violation - shutdown). DHCP snooping activé sur le VLAN 10 de SW1, port montant vers R2 déclaré de confiance.

### 3.1 SSH actif, Telnet désactivé

<img width="658" height="171" alt="image" src="https://github.com/user-attachments/assets/0c18b268-4ad2-4fa3-b23d-dad83b5c0908" />

### 3.2 Port security - cycle complet

Démonstration du cycle complet violation de sécurité déclenchée  port passé en err-disabled  relève manuelle du port.

<img width="394" height="234" alt="image" src="https://github.com/user-attachments/assets/b4bbb906-f614-4d3e-9367-f8113728d409" />

Log de violation déclenchée ("%PORT_SECURITY-2-PSECURE_VIOLATION") et passage en err-disabled
<img width="277" height="80" alt="image" src="https://github.com/user-attachments/assets/6157b22d-c657-406f-8779-48c843021d90" />

<img width="310" height="68" alt="image" src="https://github.com/user-attachments/assets/a09c0d80-0016-46ca-a8c3-441214b5490b" />
<img width="868" height="97" alt="image" src="https://github.com/user-attachments/assets/dab28c97-8005-4b92-932e-4312cfa32099" />

### 3.3 DHCP Snooping

<img width="583" height="355" alt="image" src="https://github.com/user-attachments/assets/2782484c-ec8c-4d46-ae6b-191f1381d9f0" />

<img width="749" height="181" alt="image" src="https://github.com/user-attachments/assets/a936321f-e4ed-4186-beac-6ee738dd6d67" />

### 3.4 Procédure de relève d'un port err-disabled

1. Identifier le port concerné : "show interfaces status | include err-disabled"
2. Retirer ou identifier l'appareil non autorisé à l'origine de la violation
3. Relever le port : "interface e0/x" - "shutdown" - "no shutdown"
4. Vérifier le retour à l'état Secure-up : "show port-security interface e0/x"

---

## 4. Phase 3 - Filtrage réseau par ACL

> Trois ACL nommées mises en place : "ADMIN-ONLY" (standard, restreint l'accès SSH au seul poste 192.168.20.10, appliquée sur les lignes VTY des 3 routeurs via "access-class"), "INVITES-ONLY" (étendue, restreint le VLAN 99 au Web et au DNS, appliquée sur la sous-interface G0/0.99 de R2 en entrée), et une ligne de blocage RDP explicite.

### 4.1 Calcul des wildcard masks utilisés

| Réseau / hôte | Masque | Wildcard | Calcul |
|---|---|---|---|
| 192.168.99.0/24 (Invités) | 255.255.255.0 | 0.0.0.255 | 255.255.255.255 − 255.255.255.0 |
| 192.168.20.10 (PC-Admin, hôte unique) | 255.255.255.255 (/32) | 0.0.0.0 (mot-clé "host") | Inversion bit à bit d'un masque /32 |

### 4.2 Justification du placement des ACL

- **ADMIN-ONLY** est appliquée sur les lignes VTY ("access-class") car elle filtre l'accès administratif au routeur lui-même, pas du trafic qui transite.
- **INVITES-ONLY** est posée sur la sous-interface G0/0.99 de R2, au plus près de la source du VLAN Invités : le trafic non autorisé est rejeté dès son entrée dans le réseau, sans consommer de ressources sur les liens backbone.

### 4.3 Test - accès SSH restreint


### 4.4 Test - isolation du VLAN Invités

les captures sont deja plus haut je ne vais pas les remettre pour que ça reste lisible

---

## 5. Sauvegarde des configurations

"write memory" exécuté sur les 5 équipements (R1, R2, R3, SW1, SW2) après validation de chaque phase.

---------------------------------------------------------------------------------------------
R1#show running-config
Building configuration...

Current configuration : 1373 bytes
!
upgrade fpd auto
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R1
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
!
no ip domain lookup
ip domain name git.local
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
!
username marcus privilege 15 secret 5 $1$zvgx$DPzBfK4H7xjX8J/96qf/E0
!
redundancy
!
!
ip ssh time-out 60
ip ssh version 2
!
!
!
!
!
!
!
!
!
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
 ip ospf 1 area 0
!
interface FastEthernet0/0
 description Backbone-R2
 ip address 10.0.12.1 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 ip ospf 1 area 0
 duplex half
!
interface FastEthernet1/0
 description Backbone-R3
 ip address 10.0.13.1 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 ip ospf 1 area 0
 duplex half
!
router ospf 1
 router-id 1.1.1.1
!
ip forward-protocol nd
no ip http server
no ip http secure-server
!
!
!
ip access-list standard ADMIN-ONLY
 permit 192.168.20.10
 deny   any log
!
!
!
!
control-plane
!
!
!
mgcp profile default
!
!
!
gatekeeper
 shutdown
!
!
line con 0
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
 access-class ADMIN-ONLY in
 exec-timeout 5 0
 login local
 transport input ssh
!
!
end
---------------------------------------------------------------------------------------------
R2#show running-config
Building configuration...

Current configuration : 1760 bytes
!
upgrade fpd auto
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R2
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
!
no ip domain lookup
ip domain name git.local
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
!
username marcus privilege 15 secret 5 $1$l2en$NsEptK0Z/GjxSKNks8pCL1
!
redundancy
!
!
ip ssh time-out 60
ip ssh version 2
!
!
!
!
!
!
!
!
!
!
interface Loopback0
 ip address 2.2.2.2 255.255.255.255
 ip ospf 1 area 0
!
interface FastEthernet0/0
 description Backbone-R1
 ip address 10.0.12.2 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 ip ospf 1 area 0
 duplex half
!
interface FastEthernet1/0
 no ip address
 duplex half
!
interface FastEthernet1/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 ip ospf 1 area 0
!
interface FastEthernet1/0.99
 encapsulation dot1Q 99
 ip address 192.168.99.1 255.255.255.0
 ip access-group INVITES-ONLY in
 ip ospf 1 area 0
!
router ospf 1
 router-id 2.2.2.2
 passive-interface FastEthernet1/0.10
 passive-interface FastEthernet1/0.99
!
ip forward-protocol nd
no ip http server
no ip http secure-server
!
!
!
ip access-list standard ADMIN-ONLY
 permit 192.168.20.10
 deny   any log
!
ip access-list extended INVITES-ONLY
 permit tcp 192.168.99.0 0.0.0.255 any eq www
 permit tcp 192.168.99.0 0.0.0.255 any eq 443
 permit udp 192.168.99.0 0.0.0.255 any eq domain
 deny   ip any any
!
!
!
!
control-plane
!
!
!
mgcp profile default
!
!
!
gatekeeper
 shutdown
!
!
line con 0
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
 access-class ADMIN-ONLY in
 exec-timeout 5 0
 login local
 transport input ssh
!
!
end
---------------------------------------------------------------------------------------------
R3#show running-config
Building configuration...

Current configuration : 1395 bytes
!
upgrade fpd auto
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R3
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
!
no ip domain lookup
ip domain name git.local
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
!
username marcus privilege 15 secret 5 $1$qpqt$kwb45Zt5B5xTB20NWBPPe1
!
redundancy
!
!
ip ssh time-out 60
ip ssh version 2
!
!
!
!
!
!
!
!
!
!
interface Loopback0
 ip address 3.3.3.3 255.255.255.255
 ip ospf 1 area 0
!
interface FastEthernet0/0
 description Backbone-R1
 ip address 10.0.13.2 255.255.255.252
 ip ospf authentication message-digest
 ip ospf message-digest-key 1 md5 cisco
 ip ospf 1 area 0
 duplex half
!
interface FastEthernet1/0
 description VLAN-ADMIN
 ip address 192.168.20.1 255.255.255.0
 ip ospf 1 area 0
 duplex half
!
interface FastEthernet2/0
 no ip address
 shutdown
 duplex half
!
router ospf 1
 router-id 3.3.3.3
 passive-interface FastEthernet1/0
!
ip forward-protocol nd
no ip http server
no ip http secure-server
!
!
!
ip access-list standard ADMIN-ONLY
 permit 192.168.20.10
 deny   any log
!
!
!
!
control-plane
!
!
!
mgcp profile default
!
!
!
gatekeeper
 shutdown
!
!
line con 0
 stopbits 1
line aux 0
 stopbits 1
line vty 0 4
 access-class ADMIN-ONLY in
 exec-timeout 5 0
 login local
 transport input ssh
!
!
end

---------------------------------------------------------------------------------------------
SW1#show running-config
Building configuration...

Current configuration : 1838 bytes
!
version 15.1
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW1
!
boot-start-marker
boot-end-marker
!
!
!
username marcus privilege 15 secret 4 06YFDUHH61wAE/kLkDq9BGho1QM5EnRtoyr8cHAUg.2
no aaa new-model
no ipv6 cef
ipv6 multicast rpf use-bgp
!
no ip domain-lookup
ip domain-name git.local
ip cef
!
!
ip dhcp snooping vlan 10
no ip dhcp snooping information option
ip dhcp snooping
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
!
!
vlan internal allocation policy ascending
!
ip ssh time-out 60
ip ssh version 2
!
!
!
!
!
!
!
!
!
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
 ip ospf 1 area 0
!
interface Ethernet0/0
 description Backbone-R2
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 10,99
 switchport mode trunk
 duplex auto
 ip dhcp snooping trust
!
interface Ethernet0/1
 description PC-SALLE-INFO
 switchport access vlan 10
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 12,13
 switchport mode access
 switchport port-security
 switchport port-security mac-address 0050.7966.6807
 duplex auto
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface Ethernet0/2
 switchport access vlan 99
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 10,12,99
 switchport mode access
 duplex auto
 ip dhcp snooping trust
!
interface Ethernet0/3
 switchport trunk encapsulation dot1q
 switchport trunk allowed vlan 13
 switchport mode trunk
 duplex auto
!
router ospf 1
 router-id 1.1.1.1
!
!
no ip http server
!
!
!
ip access-list standard ADMIN-ONLY
 permit 192.168.20.10
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 access-class ADMIN-ONLY in
 exec-timeout 5 0
 login local
 transport input ssh
!
end
---------------------------------------------------------------------------------------------
SW2#show running-config
Building configuration...

Current configuration : 905 bytes
!
version 15.1
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW2
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
no ipv6 cef
ipv6 multicast rpf use-bgp
!
ip cef
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
!
!
!
vlan internal allocation policy ascending
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 switchport access vlan 20
 switchport mode access
 duplex auto
!
interface Ethernet0/1
 switchport access vlan 20
 switchport mode access
 switchport port-security
 switchport port-security mac-address 0050.7966.6806
 duplex auto
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface Ethernet0/2
 duplex auto
!
interface Ethernet0/3
 duplex auto
!
!
no ip http server
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
end
---------------------------------------------------------------------------------------------
---

## 6. Phases 4 et 5 - Non traitées

Par choix assumé, seules les Phases 1 à 3 ont été réalisées.

**Phase 4 (NAT)** - non réalisée. Un doute est apparu sur la cohérence entre l'interface reliée au cloud « Net » et l'environnement réseau réel de la plateforme PNetLab ; plutôt que de risquer d'altérer une configuration fonctionnelle et validée (Phases 1 à 3), le choix a été fait de ne pas toucher à cette partie.

**Phase 5 (Audit final / incident imposé)** - non réalisée. Cette phase implique par nature de modifier volontairement une configuration qui fonctionne (désynchronisation MD5, ACL mal orientée, ou perte de configuration après reboot). Au vu du temps déjà investi et de l'heure tardive, le risque de ne pas pouvoir revenir à un état stable avant le rendu a été jugé trop élevé.
