# QCM_CCNA_TP-FINAL

### Étape 1
j'ai mis une croix devant la reponse
## Catégorie A — OSPF

### A1. OSPF appartient à quelle catégorie de protocole de routage ?

* a) Vecteur de distance
* b) État de liens **X**
* c) Hybride
* d) Vecteur de chemin

### A2. Quel algorithme utilise OSPF pour calculer le meilleur chemin ?

* a) Bellman-Ford
* b) Dijkstra (SPF) **X**
* c) DUAL
* d) A*

### A3. Quelle est la bande passante de référence par défaut utilisée pour calculer le coût OSPF ?

* a) 10 Mbps
* b) 100 Mbps **X**
* c) 1000 Mbps
* d) 10000 Mbps

### A4. Quelle commande modifie la bande passante de référence utilisée dans le calcul du coût OSPF ?

* a) bandwidth reference **X**
* b) auto-cost reference-bandwidth
* c) ip ospf cost
* d) reference-bandwidth ospf

### A5. Comment est déterminé le Router-ID si aucun n'est configuré manuellement ?

* a) IP la plus basse de toutes les interfaces
* b) IP la plus haute des interfaces actives, priorité aux loopbacks **X**
* c) Adresse MAC la plus haute
* d) Toujours 0.0.0.0

### A6. Pourquoi recommande-t-on une interface loopback pour fixer le Router-ID ?

* a) Elle est toujours active, ce qui le stabilise
* b) Elle est plus rapide
* c) Elle consomme moins de mémoire
* d) C'est obligatoire pour OSPF **X**

### A7. Quel est le rôle du DR (Designated Router) sur un segment multi-accès ?

* a) Répartir la charge du trafic
* b) Centraliser les échanges LSA pour réduire le nombre d'adjacences **X**
* c) Chiffrer le trafic OSPF
* d) Remplacer le Router-ID

### A8. Sur un lien point-à-point, l'élection DR/BDR a-t-elle lieu ?

* a) Oui, systématiquement
* b) Non, elle n'a pas de sens sur ce type de lien
* c) Seulement si configurée manuellement **X**
* d) Seulement en IPv6

### A9. Valeurs par défaut des timers Hello/Dead sur un réseau broadcast (Ethernet) ?

* a) 5s / 20s **X**
* b) 10s / 40s
* c) 30s / 120s
* d) 60s / 180s

### A10. Que se passe-t-il si les timers Hello/Dead diffèrent entre deux voisins ?

* a) L'adjacence se forme quand même
* b) L'adjacence ne peut pas se former **X**
* c) Seul le Hello timer compte
* d) OSPF ignore cette différence

### A11. Quel état de voisinage OSPF indique une adjacence pleinement établie ?

* a) 2-Way
* b) ExStart
* c) Full **X**
* d) Loading

### A12. Quel type de LSA décrit les liens directement connectés d'un routeur ?

* a) Router LSA (type 1)
* b) Network LSA
* c) Summary LSA **X**
* d) External LSA

### A13. Quelle commande affiche l'état des adjacences OSPF ?

* a) show ip ospf database
* b) show ip ospf neighbor **X**
* c) show ip route ospf
* d) show ip protocols

### A14. Lequel de ces critères N'EMPÊCHE PAS la formation d'une adjacence OSPF s'il diffère ?

* a) Area ID
* b) Masque de sous-réseau du lien
* c) Hostname du routeur **X**
* d) Type d'authentification

### A15. Quelle commande observe en temps réel la formation des adjacences OSPF ?

* a) show ip ospf adj
* b) debug ip ospf adj **X**
* c) trace ip ospf
* d) monitor ospf neighbor

### A16. Que représente l'aire 0 dans OSPF ?

* a) Une zone optionnelle
* b) L'aire de backbone, obligatoire en conception multi-aires **X**
* c) Une zone réservée aux liens WAN
* d) Une aire désactivée par défaut

### A17. Dans `network 10.0.12.0 0.0.0.3 area 0`, que représente `0.0.0.3` ?

* a) Un masque de sous-réseau classique
* b) Un wildcard mask
* c) Une adresse de broadcast **X**
* d) Un ID de zone

---

# Catégorie B — Configuration OSPF (Module 2)

### B1. Dans `router ospf 1`, que représente le « 1 » ?

* a) L'area ID
* b) Le process ID, localement significatif au routeur
* c) Le Router-ID **X**
* d) Le numéro d'AS

### B2. Le process ID OSPF doit-il être identique sur tous les routeurs du domaine ?

* a) Oui, obligatoirement
* b) Non, il est local à chaque routeur **X**
* c) Oui, en zone unique seulement
* d) Non, seulement en IPv6

### B3. Quel est l'effet de `passive-interface g0/0` ?

* a) L'interface arrête d'annoncer son réseau
* b) L'interface continue d'annoncer son réseau mais n'envoie plus de Hello **X**
* c) L'interface est désactivée
* d) L'interface devient une interface de secours

### B4. Pourquoi rendre passive une interface connectée à un LAN utilisateur ?

* a) Pour économiser de la bande passante uniquement
* b) Pour empêcher un hôte non autorisé de former une adjacence OSPF **X**
* c) Parce que c'est obligatoire dans OSPF
* d) Pour activer le DR sur ce segment

### B5. Quelle commande rend toutes les interfaces passives par défaut ?

* a) passive-interface all
* b) passive-interface default
* c) no active-interface **X**
* d) shutdown-interface default

### B6. Que fait `default-information originate` sur un routeur OSPF ?

* a) Elle génère une route par défaut même sans route locale **X**
* b) Elle injecte une route par défaut dans OSPF, à condition d'en posséder déjà une
* c) Elle supprime toute route par défaut
* d) Elle active OSPF automatiquement

### B7. Quelle différence apporte le mot-clé `always` dans `default-information originate always` ?

* a) Force l'injection même sans route par défaut locale active **X**
* b) Diminue le coût de la route
* c) Change le type de LSA en type 1
* d) Active l'authentification automatiquement

### B8. Quel type de LSA transporte une route par défaut externe injectée ?

* a) Type 1
* b) Type 2 **X**
* c) Type 3
* d) Type 5 (External)

### B9. Quelle commande active l'authentification MD5 sur une interface OSPF ?

* a) `ip ospf authentication message-digest` **X**
* b) `ip ospf md5-key`
* c) `area 0 md5`

### B10. Avant d'activer l'authentification MD5, quelle commande faut-il configurer sur l'interface ?

* a) `ip ospf priority 0`
* b) `ip ospf message-digest-key <n> md5 <clé>` **X**
* c) `ip ospf network point-to-point`
* d) `ip ospf cost 10`

### B11. Que se passe-t-il si la clé MD5 diffère entre deux routeurs voisins ?

* a) L'adjacence se forme avec un avertissement
* b) L'adjacence échoue (mismatch authentication) **X**
* c) Le trafic passe en clair
* d) OSPF désactive l'authentification automatiquement

### B12. Quelle commande vérifie si l'authentification est active sur une interface OSPF ?

* a) `show ip ospf interface <if> | include Auth` **X**
* b) `show running-config ospf`
* c) `show ip ospf auth-status`
* d) `debug ip ospf auth`

### B13. Quelle commande liste les réseaux annoncés et les interfaces passives d'un processus OSPF ?

* a) `show ip ospf database`
* b) `show ip protocols` **X**
* c) `show ip ospf neighbor`
* d) `show ip route`

### B14. Sur quel type d'interface faut-il éviter `passive-interface` ?

* a) Les interfaces LAN utilisateur
* b) Les interfaces backbone reliant deux routeurs OSPF **X**
* c) Les interfaces loopback
* d) Toutes les interfaces

### B15. Un voisin reste bloqué à l'état EXSTART. Cause fréquente ?

* a) MTU différente entre les deux interfaces **X**
* b) Adresse IP incorrecte
* c) VLAN natif différent
* d) Câble défectueux uniquement

### B16. Quelle commande force le recalcul complet du processus OSPF ?

* a) `clear ip ospf process` **X**
* b) reload ospf
* c) restart ip ospf
* d) Supprimer puis recréer router ospf 1 uniquement

### B17. Pourquoi sauvegarder (`write memory`) après chaque modification OSPF validée ?

* a) Ce n'est pas nécessaire, OSPF sauvegarde automatiquement
* b) Pour éviter de perdre la configuration au prochain redémarrage **X**
* c) Uniquement requis pour les ACL
* d) Cela relance automatiquement les adjacences

---

# Catégorie C — Sécurité réseau (Module 3)

### C1. Pourquoi Telnet est-il déconseillé pour l'administration des équipements ?

* a) Il est plus lent que SSH
* b) Il transmet identifiants et commandes en clair **X**
* c) Il ne fonctionne pas sur IPv6
* d) Il nécessite une clé RSA

### C2. Quelle commande génère la paire de clés RSA nécessaire à SSH ?

* a) `crypto key generate rsa modulus 2048` **X**
* b) generate ssh-key rsa
* c) ip ssh key generate
* d) enable ssh rsa

### C3. Quel prérequis, avant la génération de clé RSA, sert d'identifiant à la clé (FQDN) ?

* a) hostname R1 seul
* b) `ip domain-name <domaine>` **X**
* c) ip ssh enable
* d) crypto pki enable

### C4. Que fait `ip ssh version 2` ?

* a) Active SSHv1 et v2 simultanément
* b) Force l'utilisation exclusive de SSHv2 **X**
* c) Désactive SSH
* d) Configure le port SSH

### C5. Sous `line vty 0 4`, quelle commande restreint l'accès au protocole SSH uniquement ?

* a) access-class ssh-only
* b) `transport input ssh` **X**
* c) login ssh
* d) ssh only

### C6. Quelle commande authentifie les connexions vty via la base de comptes locaux ?

* a) `login local` **X**
* b) login authentication
* c) aaa new-model seul
* d) password local

### C7. Entre `enable secret` et `enable password`, lequel est prioritaire si les deux existent ?

* a) enable password
* b) enable secret **X**
* c) Les deux s'appliquent en même temps
* d) Aucun, il faut choisir explicitement

### C8. Intérêt de `exec-timeout 5 0` sur une ligne vty ?

* a) Limite la bande passante disponible
* b) Déconnecte une session inactive après 5 minutes **X**
* c) Force une reconnexion toutes les 5 minutes
* d) Bloque les connexions après 5 tentatives

### C9. Que fait `switchport port-security mac-address sticky` ?

* a) Bloque toutes les adresses MAC
* b) Apprend dynamiquement la première MAC et l'ajoute à la configuration **X**
* c) Change l'adresse MAC du port
* d) Désactive le port

### C10. Quel mode de violation port security place le port en état err-disabled ?

* a) protect
* b) restrict
* c) shutdown **X**
* d) monitor

### C11. Comment relever un port passé en err-disabled ?

* a) reload du switch obligatoire
* b) shutdown puis no shutdown sur l'interface **X**
* c) clear port-security
* d) no switchport port-security

### C12. Quelle commande active globalement le DHCP snooping sur un switch ?

* a) `ip dhcp snooping` **X**
* b) dhcp snooping enable
* c) ip dhcp inspect
* d) switchport dhcp snooping

### C13. Qu'est-ce qu'un port « trusted » en DHCP snooping ?

* a) Un port où le filtrage MAC est désactivé
* b) Le seul type de port autorisé à relayer des réponses DHCP **X**
* c) Un port avec port security activé
* d) Un port du VLAN natif

### C14. Objectif de `ip dhcp snooping limit rate <n>` sur un port non fiable ?

* a) Limiter la bande passante totale du port
* b) Limiter le nombre de paquets DHCP/seconde (anti flood/DoS) **X**
* c) Limiter le nombre d'adresses MAC apprises
* d) Limiter le débit DHCP à un seul bail

### C15. Risque principal d'un serveur DHCP non autorisé (rogue) sur le réseau ?

* a) Il ralentit uniquement le Wi-Fi
* b) Il peut distribuer de fausses adresses/passerelles et détourner le trafic **X**
* c) Aucun impact si le VLAN est isolé
* d) Il désactive automatiquement OSPF

### C16. Quelle fonctionnalité complète le DHCP snooping contre l'usurpation via ARP ?

* a) Port security
* b) Dynamic ARP Inspection (DAI) **X**
* c) OSPF authentication
* d) NAT overload

---

# Catégorie D — Concepts ACL (Module 4)

### D1. Que fait une ACL Cisco par défaut si aucune règle ne correspond à un paquet ?

* a) Elle l'autorise par défaut
* b) Elle le refuse (deny any implicite) **X**
* c) Elle génère une erreur
* d) Elle le redirige vers le CPU

### D2. Rôle d'un wildcard mask dans une ACL ?

* a) Identique au masque de sous-réseau
* b) Indique quels bits doivent correspondre (0) et lesquels sont ignorés (1) **X**
* c) Chiffre l'adresse IP
* d) Définit la durée de vie de la règle

### D3. Wildcard mask correspondant à `192.168.10.0/24` ?

* a) 255.255.255.0
* b) 0.0.0.255 **X**
* c) 0.0.0.0
* d) 0.255.255.255

### D4. Quel mot-clé remplace le wildcard `0.0.0.0` pour un hôte unique ?

* a) any
* b) all
* c) host **X**
* d) single

### D5. Quel mot-clé remplace le wildcard `255.255.255.255` pour toute adresse ?

* a) any **X**
* b) all
* c) host
* d) none

### D6. Plage de numéros d'une ACL IP standard numérotée ?

* a) 1-99 (et 1300-1999) **X**
* b) 100-199
* c) 200-299
* d) 1-999

### D7. Plage de numéros d'une ACL IP étendue numérotée ?

* a) 1-99
* b) 100-199 (et 2000-2699) **X**
* c) 300-399
* d) 1000-1099

### D8. Sur quel(s) critère(s) une ACL standard filtre-t-elle le trafic ?

* a) Adresse source uniquement **X**
* b) Source et destination
* c) Source, destination, protocole et port
* d) Adresse MAC uniquement

### D9. Sur quel(s) critère(s) une ACL étendue filtre-t-elle le trafic ?

* a) Source uniquement
* b) Source et destination, protocole, ports **X**
* c) Port de destination uniquement
* d) Protocole uniquement

### D10. Où placer de préférence une ACL étendue ?

* a) Le plus près de la destination
* b) Le plus près de la source **X**
* c) Peu importe
* d) Toujours sur le routeur de bordure Internet

### D11. Où placer de préférence une ACL standard ?

* a) Le plus près de la source
* b) Le plus près de la destination **X**
* c) Sur n'importe quelle interface
* d) Uniquement sur les lignes VTY

### D12. Dans quel ordre les lignes d'une ACL sont-elles évaluées ?

* a) Aléatoire
* b) De la dernière à la première
* c) Séquentiellement, arrêt au premier match **X**
* d) Toutes en parallèle

### D13. Une ACL standard ne contient qu'une ligne `deny 192.168.20.0 0.0.0.255`. Effet sur le reste du trafic ?

* a) Autorisé par défaut
* b) Bloqué par le deny any implicite **X**
* c) Erreur générée
* d) Redirigé

### D14. Combien d'ACL peut-on appliquer sur une même interface, pour un même protocole et un même sens ?

* a) Autant que nécessaire
* b) Une seule **X**
* c) Deux
* d) Trois

### D15. Pour bloquer un sous-réseau tout en laissant passer le reste, que faut-il ajouter après la ligne deny ?

* a) Rien
* b) Une ligne `permit any` explicite **X**
* c) Une seconde ligne deny any
* d) Un shutdown de l'interface

### D16. Avantage principal d'une ACL nommée par rapport à une ACL numérotée ?

* a) Plus rapide à traiter par le CPU
* b) Plus lisible, ajout/suppression de lignes individuelles **X**
* c) Pas de deny implicite
* d) Fonctionne uniquement avec OSPF

---

# Catégorie E — Configuration ACL IPv4 (Module 5)

### E1. Quelle commande crée une ACL standard nommée « ADMIN-ONLY » ?

* a) access-list standard ADMIN-ONLY
* b) `ip access-list standard ADMIN-ONLY` **X**
* c) ip access-group standard ADMIN-ONLY
* d) create acl standard ADMIN-ONLY

### E2. Quelle commande applique une ACL standard aux lignes VTY en entrée ?

* a) ip access-group <nom> in
* b) `access-class <nom> in` **X**
* c) line access-list <nom>
* d) vty filter <nom>

### E3. Quelle commande applique une ACL à une interface physique, en entrée ?

* a) access-class <nom> in
* b) `ip access-group <nom> in` **X**
* c) interface access-list <nom> in
* d) ip filter <nom> in

### E4. Pourquoi `access-class` plutôt que `access-group` sur les lignes VTY ?

* a) access-group est réservé aux ACL étendues
* b) access-class est la commande spécifique à l'accès administratif, différente des interfaces **X**
* c) Ce sont des synonymes
* d) access-class est obsolète

### E5. Quelle commande affiche le contenu d'une ACL avec le nombre de correspondances (matches) ?

* a) show running-config acl
* b) `show access-lists` **X**
* c) show ip interface
* d) show acl-counters

### E6. Quelle commande confirme sur quelle interface et dans quel sens une ACL est appliquée ?

* a) show access-lists
* b) `show ip interface <if> | include access list` **X**
* c) show ip protocols
* d) show running-config | include vty

### E7. Les compteurs matches restent à 0 malgré du trafic. Que vérifier en priorité ?

* a) La longueur du nom de l'ACL
* b) Que l'ACL est appliquée sur la bonne interface et le bon sens **X**
* c) La version d'IOS
* d) Le nombre de VLAN configurés

### E8. Dans une ACL étendue, quel mot-clé précède un numéro de port unique ?

* a) eq **X**
* b) at
* c) is
* d) port

### E9. Quel port TCP correspond au trafic HTTPS ?

* a) 80
* b) 443 **X**
* c) 53
* d) 22

### E10. Quel port UDP correspond au trafic DNS ?

* a) 80
* b) 443
* c) 53 **X**
* d) 67

### E11. Quel port TCP correspond au trafic RDP ?

* a) 3389 **X**
* b) 22
* c) 25
* d) 8080

### E12. Quelle syntaxe autorise le HTTP dans `permit tcp 192.168.99.0 0.0.0.255 any ...` ?

* a) eq 21
* b) eq 80 **X**
* c) eq 443
* d) eq 25

### E13. Dans une ACL nommée, comment supprimer uniquement la ligne numéro 20 ?

* a) `no 20` (en mode configuration de l'ACL) **X**
* b) delete line 20
* c) remove access-list 20
* d) clear acl 20

### E14. Une ACL RDP est appliquée en `out` sur l'interface d'entrée du trafic client, alors que le trafic ressort par une autre interface. Quel est le problème ?

* a) Aucun
* b) L'ACL doit être appliquée sur l'interface par laquelle le trafic sort réellement (ou en `in` côté client) **X**
* c) Il faut doubler l'ACL en out sur les deux interfaces
* d) RDP ne peut pas être filtré par ACL

### E15. Une ACL standard (source uniquement) suffit-elle pour bloquer un VLAN entier vers UN SEUL serveur précis, en laissant le reste accessible ?

* a) Oui
* b) Non, une ACL étendue est nécessaire pour cibler une destination précise **X**
* c) Cela dépend du masque
* d) Oui, avec host

### E16. Impact d'une ACL appliquée en entrée, au plus près de la source, sur la charge CPU du routeur ?

* a) Aucun impact
* b) Elle réduit la charge en filtrant le trafic indésirable au plus tôt **X**
* c) Elle augmente systématiquement la charge CPU
* d) Elle désactive le routage

### E17. Un poste non autorisé tente un SSH après application de l'ACL ADMIN-ONLY sur les VTY. Résultat attendu si la config est correcte ?

* a) La connexion réussit normalement
* b) La connexion est refusée **X**
* c) Le routeur redémarre
* d) Le SSH bascule en Telnet

---

# Catégorie F — NAT pour IPv4 (Module 6)

### F1. Que signifie l'acronyme NAT ?

* a) Network Address Translation **X**
* b) Network Access Tunneling
* c) Node Address Table
* d) Network Authentication Type

### F2. Comment appelle-t-on l'adresse privée d'un hôte interne, vue depuis l'intérieur du réseau ?

* a) Inside global
* b) Inside local **X**
* c) Outside local
* d) Outside global

### F3. Comment appelle-t-on l'adresse publique représentant un hôte interne, vue depuis Internet ?

* a) Inside local
* b) Inside global **X**
* c) Outside global
* d) Outside local

### F4. Quelle commande marque une interface comme faisant face au réseau interne pour le NAT ?

* a) ip nat outside
* b) `ip nat inside` **X**
* c) ip nat local
* d) ip nat private

### F5. Quelle commande marque une interface comme faisant face au réseau externe pour le NAT ?

* a) ip nat inside
* b) ip nat public
* c) `ip nat outside` **X**
* d) ip nat wan

### F6. Quel type de NAT crée une correspondance fixe et permanente, idéale pour publier un serveur ?

* a) NAT dynamique
* b) NAT statique **X**
* c) PAT/overload
* d) NAT64

### F7. Quelle commande configure un NAT statique associant `10.0.0.50` à `203.0.113.10` ?

* a) `ip nat inside source static 10.0.0.50 203.0.113.10` **X**
* b) ip nat static 10.0.0.50 to 203.0.113.10
* c) ip nat inside source list 10.0.0.50 pool 203.0.113.10
* d) nat static 10.0.0.50 203.0.113.10

### F8. Quel type de NAT traduit vers une plage limitée d'adresses publiques, sur un modèle 1-pour-1 temporaire ?

* a) NAT statique
* b) NAT dynamique (pool) **X**
* c) PAT/overload
* d) NAT66

### F9. Toutes les adresses d'un pool NAT dynamique sont utilisées et un nouvel hôte tente une traduction. Que se passe-t-il ?

* a) Le NAT utilise une adresse au hasard
* b) La traduction échoue (compteur « misses ») **X**
* c) Le pool s'agrandit automatiquement
* d) Le trafic passe sans traduction

### F10. Quel mécanisme permet à de nombreux hôtes internes de partager une seule adresse publique ?

* a) NAT statique
* b) NAT dynamique simple
* c) PAT (NAT overload), basé sur les ports **X**
* d) DHCP relay

### F11. Quel mot-clé active le PAT dans `ip nat inside source list <acl> interface <if> ...` ?

* a) pat
* b) overload **X**
* c) share
* d) multiplex

### F12. Quelle commande affiche la table des traductions NAT actives ?

* a) show ip nat pool
* b) `show ip nat translations` **X**
* c) show nat table
* d) show ip nat active

### F13. Quelle commande affiche des statistiques globales sur le NAT, avec le compteur d'échecs ?

* a) show ip nat translations
* b) `show ip nat statistics` **X**
* c) show ip nat summary
* d) show running-config nat

### F14. Pourquoi préférer le NAT statique au PAT pour publier un serveur interne accessible depuis Internet ?

* a) Le PAT ne fonctionne pas avec les serveurs
* b) Le NAT statique garantit une IP:port fixe atteignable de l'extérieur ; le PAT seul ne permet pas d'initier une connexion entrante sans redirection de port **X**
* c) Le NAT statique est plus rapide en débit
* d) Aucune différence pratique

### F15. Une entreprise dispose d'une seule adresse publique pour tout son réseau interne. Quelle solution est la plus adaptée pour l'accès Internet sortant ?

* a) NAT statique
* b) NAT dynamique avec pool de 4 adresses
* c) PAT/overload sur l'unique adresse publique **X**
* d) Aucune solution n'est possible

### F16. Une ACL utilisée dans une règle NAT (`ip nat inside source list <acl> ...`) sert à :

* a) Bloquer du trafic comme une ACL de sécurité classique
* b) Définir quels hôtes/réseaux internes sont éligibles à la traduction **X**
* c) Chiffrer les paquets NAT
* d) Limiter la bande passante NAT

### F17. Que révèle une ligne de `show ip nat translations` où plusieurs entrées partagent la même adresse « Inside global » avec des ports différents ?

* a) Une erreur de configuration
* b) Un fonctionnement normal du PAT/overload **X**
* c) Un conflit d'adresses IP
* d) Une attaque par déni de service
