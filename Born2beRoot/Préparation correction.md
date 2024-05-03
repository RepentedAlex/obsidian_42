# Préliminaire
## Tests préliminaires
# Instructions générales
## Règles générales
La signature dans `signature.txt` doit etre identique a celle du fichier `.vdi`.
# Partie obligatoire
## Project overview
### Comment fonctionne une VM ?
Une machine virtuel peut-etre decrite comme un ordinateur virtuel. C'est une emulation logicielle d'un systeme informatique qui cree un environnement qui se comporte comme un ordinateur independant de son host, avec son propre systeme d'exploitation, applications et ressources.
#### Hyperviseur
Au coeur de la machine virtuelle se trouve une couche logicielle appelee l'hyperviseur (aka Virtual Machine Monitor). L'hyperviseur agit comme un intermediaire entre le hardware et la/les machine.s virtuelle.s. Son role premier et d'allouer et gerer les ressources du CPU, de la memoire, du stockage et du reseau entre les machines virtuelles qui seraient executees sur le meme host.
#### La virtualisation
L'hyperviseur utilise des techniques de virtualisation pour creer une couche d'abstraction entre le hardware et les machines virtuelles. Cette couche d'abstraction permet a chaque VM d'operer comme si elle avait un acces direct aux ressources hardwares.
#### Hardware virtuel
Chaque VM se voit fournir ses propres composants hardware virtuels, comme un CPU virtuel, de la RAM virtuelle, des disques virtuels et des interfaces reseau virtuelles. Ces composants virtuels sont mappes au hardware physique du host par l'hyperviseur.
#### OS du guest
A l'interieur de chaque VM se trouve un systeme d'exploitation installe et qui s'execute comme s'il etait sur un ordinateur physique. L'OS guest est ignorant du fait qu'il tourne sur une VM et que le hardware avec lequel il interagit est virtuel.
#### Isolation et encapsulation
Les VM sont isolees les unes des autres et de l'OS du host. Cela signifie que les processus executes sur une VM ne peuvent pas acceder/interferer avec les processus executes sur une autre VM/le host. Chaque VM est encapsulee et s'execute independamment des autres ce qui permet d'avoir un environnement securise et isole.
#### Partage de ressources et allocation de celle-ci
L'hyperviseur est responsable de l'efficacite du partage et de l'allocation des ressources hardware entre les VM. Il alloue dynamiquement des cycles GPU, de la RAM, du stockage et des ressources reseaux a chaque VM selon leur charge et leurs besoins. Cela permet l'execution de plusieurs VM simultanement sur le meme host sans probleme.
#### Snapshots et Live Migration
La plupart des plateformes de virtualisation supportent des fonctionnalites telles que les snapshots et la Live migration. Les snapshots permettent de capturer l'etat complet d'une VM a un instant T, ce qui peut etre utile pour faire une backup ou restaurer une installation. La Live migration permet de migrer une VM live d'un host a un autre sans devoir l'arreter, ce qui est pratique pour equilibrer la workload ou en cas de maintenance.
### Le choix de l'OS

### Les grandes differences entre Rocky Linux et Debian
#### Philosophie
- Rocky Linux: RL est une distribution centree sur la communaute plutot orientee pour les entreprises. Cette distribution est compatible avec les binaires de Red Hat Enterprise Linux. Son objectif premier est d'offrir stabilite, reliability et compatibilite avec des applications pouvant satisfaire des demandes d'entreprises.
- Debian: Debian est une distribution centree sur la communaute, gratuite et open-source connue pour son adherence aux principes des logiciels gratuits et son commitment a la stabilite et a la securite.
#### Gestion des paquets
- Rocky Linux: Rocky Linux utilise le systeme de gestion de paquets RPM, aussi utilise par RHEL, il fournit les gestionnaire de paquets `yum` et `dnf`.
- Debian: Debian utilise le format de paquets `.deb` et Advanced Package Tool (APT) pour la gestion des paquets, ce qui inclut `apt-get` et `apt`.
#### Release cycle
- Rocky Linux: RL a un rythme de releases plutot lent, une nouvelle version majeure sort plusieurs annees apres la derniere release. Cela permet de mettre l'accent sur la stabilite et la compatibilite.
- Debian: Debian quant a lui a un rythme de release plus soutenu, une version majeure tous les 2/3 ans. De plus Debian a des branches testing et unstable qui sont mis a jour plus frequemment.
#### Communaute et support
- Rocky Linux: RL est plutot recente mais herite de l'ecosysteme de RHEL, qui dispose de support commercial de plusieurs vendeurs.
- Debian: Debian dispose d'une communaute tres large, donc d'une enorme documentation disponible via internet.
#### Software repository
- Rocky Linux: Le software repository de RL est compatible avec RHEL, ce qui donne acces a beaucoup de logiciels compatible a une utilisation en entreprise.
- Debian: Debian offre une bibliotheque logicielle contenant plus de 59.000 paquets, dont une large selection de logiciels open-source.
#### Desktop environment
- Rocky Linux: RL est plutot concentree sur une utilisation serveur/entreprise, meme si elle peut etre utilisee avec un environnement bureau si desire.
- Debian: Debian offre plusieurs environnements bureaux, tels que GNOME, KDE, Xcfe ou LXDE, ce qui la rend adequate a des utilisations bureautiques ou serveurs.
### Les avantages d'une VM
### Qu'est-ce que SELinux ?
Security-Enhanced Linux (SELinux) est une architecture de sécurité pour Linux qui permet aux admins de mieux contrôler les accès au système. 
### Qu'est-ce que DNF ?
`dnf` (DaNdified Yum) est un [[Gestionnaire de paquets|gestionnaire de paquets]] qui reprend le fonctionnement de `yum`.
## User
### Création d'un nouveau user
#### Expliquer comment a été mise en place la politique de mdp
##### Modif fichier 1
##### Modif fichier 2
### Création d'un nouveau groupe
### Assignation du nouvel user au groupe
### Pourquoi mettre en place une politique de mdp stricte
#### Avantages
#### Désavantages
## Hostname and partitions
### Comment afficher les partitions de la VM
lsblk
### Comment fonctionne LVM
#### Avantages de LVM
## SUDO
### Montrer comment assigner un user au groupe sudo
### Objectif et fonctionnement de sudo
### Le logging des commandes lancées avec sudo
## UFW / Firewalld
### Fonctionnement et but d'un firewall

### Suppression de la règle pour le port 8080
## SSH
### Fonctionnement et importance du SSH
## Script monitoring
### Expliquer fonctionnement du script
### Explication du fonctionnement de cron
#### Syntaxe
`1 2 3 4 5` = À 02h01, le 3 avril et les jeudi en avril
`0 0,12 1 */2 *` = À 00h00 et 12h00, le premier de chaque second mois
`[minute] [hour] [day (month)] [month] [day (week)]`
# Bonus
## Bonus
### Setup du wordpress avec une stack LLMP (Linux - Lighttpd - MariaDB - PHP)
### Service au choix : fail2ban
#### Pourquoi fail2ban