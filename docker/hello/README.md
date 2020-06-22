# Hello Docker

## Prérequis

- [Principes de la conteneurisation](../../conteneurisation/principes/README.md)

## C'est quoi ?

L'objectif de ce tutoriel est de prendre en main le moteur de conteneurisation `Docker`.

## Environnement

Docker est une technologie s'appuyant sur des technologies du noyau Linux. Il est donc naturel de l'installer et de l'utiliser sur des machines Linux. Pour Mac ou Windows, des solutions existent, notamment [Docker desktop](https://docs.docker.com/get-docker/).

Dans ce tutoriel, on utilisera une machine virtuelle GCP `f1-micro` (1 vCPU, 614 Mo) avec `Debian 10` mais n'importe quelle machine Linux sur laquelle vous avez les droits `root` devrait convenir.

## Installation

L'installation est documentée ici : [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/) et, plus particulièrement, pour Debian ici : [https://docs.docker.com/engine/install/debian/](https://docs.docker.com/engine/install/debian/).

Les étapes de post-installation, disponibles [ici](https://docs.docker.com/engine/install/linux-postinstall/) sont optionnelles mais permettent d'accorder à des utilisateurs non `root` les permissions pour lancer Docker.  
Attention : avoir le droit de lancer des conteneurs Docker est un droit très fort, souvent considéré comme équivalent à avoir les droits `root` sur la machine.

## Hello Docker

Le `Hello world` Docker consiste à afficher Hello world depuis un processus isolé du reste du système.

```
docker run hello-world
```

(note : il peut être nécessaire de préfixer les commandes par `sudo` pour les exécuter avec les droits root si vous n'avez pas suivi les étapes de post-installation précédentes).

Résultat :

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete
Digest: sha256:d58e752213a51785838f9eed2b7a498ffa1cb3aa7f946dda11af39286c3db9a9
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
[...]
```

`docker run` permet de lancer un conteneur à partir d'une image.  
Si l'image n'existe pas localement (ie elle n'a jamais été utilisée / téléchargée sur la machine locale), `Docker` va directement la télécharger depuis le registre central d'image : [Dockerhub](https://hub.docker.com/).

Relançons la commande :

```
docker run hello-world
```

Résultat :

```
Hello from Docker!
[...]
```

Quand l'image a déjà été téléchargée une fois, elle peut être réutilisée directement.  
On retrouve le paradigme : une même image peut servir à lancer plusieurs conteneurs.

## Un peu plus loin

On a vu comment lancer un conteneur affichant un message puis s'arrêtant.  
La plupart du temps, Docker est utilisé pour lancer des serveurs (`postgres`, `tomcat`, `nginx`, `node` ...).  
Commençons par un serveur web classique : `nginx`.  
Une recherche sur Docker hub montre qu'il existe des images Docker officielles : https://hub.docker.com/_/nginx

Nous pouvons donc le lancer simplement :

```
docker run nginx
```

Félicitations ! Vous avez un serveur web `nginx` qui tourne !  
Mais, il est impossible d'y accéder. Habituellement, un serveur web écoute sur le port `80` mais, si vous tentez d'accéder au port 80 local : `curl http://localhost:80` vous aurez une erreur.  
_Par défaut, un conteneur est isolé du monde extérieur_ : nginx écoute bien sur le port `80` à l'intérieur du conteneur mais ce port ne correspond à rien sur le système hôte.

(Note : ctrl + c permet (en général) de tuer le conteneur. Il est aussi possible d'utilier la commande `docker kill`, qui sera abordée dans le tutorial `Docker avancé`.)

## Mapping de port

L'option `-p` permet de faire une correspondance entre un port de l'hôte (le système sur lequel Docker est installé) et un port du conteneur.

```
docker run -p 5678:80 nginx
```

Cette commande va lancer un conteneur `nginx` et faire correspondre le port `5678` de l'hôte avec le port `80` du conteneur.  
Depuis un autre terminal, vous pouvez vérifier que le nginx répond bien :

```
curl http://localhost:5678
```

(Si votre machine a un navigateur internet, vous pouvez tester directement dans le navigateur [http://localhost:5678](http://localhost:5678)).

## Aller plus loin

La suite logique à ce tutoriel est d'utiliser Docker : [Utilisation avancée de Docker](../../docker/utilisation).
