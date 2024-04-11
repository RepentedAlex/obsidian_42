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
### Les avantages d'une VM
### Qu'est-ce que SELinux ?
### Qu'est-ce que DNF ?
## Simple configuration
## User
## Hostname and partitions
## SUDO
## UFW / Firewalld
## SSH
## Script monitoring
# Bonus
## Bonus