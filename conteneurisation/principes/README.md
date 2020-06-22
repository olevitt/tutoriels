# Conteneurisation

## Prérequis

Il n'y a pas de prérequis pour suivre ce tutoriel.

## C'est quoi ?

La conteneurisation consiste à faire tourner un processus dans un environnement virtuel, isolé du système hôte et des autres processus.

Le moteur de conteneurisation le plus connu est [`Docker`](https://www.docker.com/) mais il en existe d'autres (`rkt`, `containerd`, `UCR` ...). Un abus de langage courant est de confondre la technologie (`Conteneurisation`) et une implémentation (`Docker`).

## Conteneur ou machine virtuelle ?

![](img/vm.png)
_Source : https://www.docker.com/resources/what-container_

Les machines virtuelles utilisent une isolation au niveau système. Cette virtualisation conduit à la création et à la vie d'un OS (Operating System) invité par machine virtuelle.

![](img/conteneurs.png)
_Source : https://www.docker.com/resources/what-container_

La conteneurisation est une isolation applicative et logique. Il n'y a qu'un seul système.

## Image et conteneur

Il y a deux concepts importants à ne pas mélanger : `image` et `conteneur`.  
Une `image` correspond à la définition de l'environnement.  
Un `conteneur` correspond à un processus en cours d'exécution.

Une même `image` peut donc être à l'origine de nombreux `conteneur`s. On publie des images et on fait tourner des conteneurs.

## Avantages de la conteneurisation

La conteneurisation propose 2 avantages majeurs :

- Légèreté : chaque conteneur ne vient qu'avec ce dont il a strictement besoin. Il n'y a donc pas de multiplication des OS. Chaque conteneur se lance donc beaucoup plus rapidement et consomme moins de ressources (CPU / mémoire). Il est donc possible de déployer, sur une même machine, beaucoup plus de conteneurs que l'équivalent en machines virtuelles. On parle de `densification`.
- Standardisation des livrables et process : le concept d'image est agnostique de la technologie sous-jacente (on peut faire cohabiter des conteneurs `python`, `Java`, `node` ... sans se poser de questions) et garanti le même comportement quel que soit l'environnement d'exécution.

## Comment ça marche ?

La conteneurisation s'appuie sur des technologies du noyau linux dont les `namespaces` et les `cgroups`.  
Un aperçu des différentes technologies sous-jacentes est disponible ici : https://medium.com/@BeNitinAgarwal/understanding-the-docker-internals-7ccb052ce9fe

## Aller plus loin

La suite logique à ce tutoriel est de prendre en main Docker : [Hello docker](../../docker/hello).
