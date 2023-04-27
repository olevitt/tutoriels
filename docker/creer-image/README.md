# Créer une image Docker

## Prérequis

- [Hello Docker](../hello/README.md)

## Environnement

On suppose `Docker` déjà installé (cf [Hello Docker](../hello/README.md))

## Registry Docker

Un `registry` Docker est un dépôt d'image. Il permet à la fois de récupérer les images (pour `docker run`) mais aussi d'en publier (via `docker push`). Il existe des registres publics ([Docker hub](https://hub.docker.com/) étant le plus connu) mais aussi des registres privés (par exemple des registres internes à une entreprise).

## Nom d'une image

Le nom d'une image est constitué de plusieurs parties :

`registry/organisation/nomimage:tag`

Le `nomimage` est obligatoire, les autres parties sont optionnelles.

Exemples :  
`postgres` : L'image officielle postgres du registry Docker hub en tag `latest`.  
`postgres:12` : La version (tag) 12 de l'image officielle postgres du registry Docker hub  
`inseefrlab/onyxia-ui` : L'image `onyxia-ui` de l'organisation `inseefrlab` sur Docker hub  
`gcr.io/distroless/base-debian10:nonroot` : l'image `base-debian10` de l'organisation `distroless` en tag `nonroot` sur le registre `gcr.io`

## Ma 1ère image Docker

Pour l'instant, on a fait tourner des conteneurs d'une image créee par quelqu'un d'autre. Il est temps de créer notre propre image.

### Dockerfile

Une image docker est décrite dans un fichier `Dockerfile` (convention de nommage, sans extension).  
Un Dockerfile commence par une instruction FROM indiquant l'image de base de laquelle on repart.  
Par exemple :

```Dockerfile
FROM nginx
```

est un Dockerfile valide correspondant exactement à l'image de nginx (sans préciser le tag donc la version `latest` au moment de la construction).  
On peut ensuite enchainer différentes commandes pour compléter l'environnement :  
`RUN`, `ADD`, `ENV` ... Les différentes commandes sont documentées ici : [https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)

### Construire (builder) une image

A partir d'un Dockerfile, on peut builder une image en utilisant la commande `docker build` :

```
docker build -t monnginx .
```

Explications :  
`docker` : on utilise la CLI (Command Line Interface) de docker  
`build` : commande pour builder une image à partir d'un Dockerfile  
`-t` : t pour tag, option pour que l'on veut nommer (tagger) l'image résultante  
`toto` : le nom à attribuer à l'image. En l'appelant toto, on imagine ne l'utiliser qu'en local.
`.` : à ne pas oublier. Le "." permet de préciser que le contexte de builder est le dossier courant

Docker va automatiquement lire le fichier `Dockerfile` et exécuter les instructions une par une. Si le fichier ne s'appelle pas `Dockerfile`, il est possible de le préciser en utilisant l'option `-f MonFichierDockerfile`

Félicitations, vous venez de construire **votre première image**.  
On peut la lancer simplement :

```
docker run -p 5555:80 monnginx
```

### Construire une belle image

Un `nginx` vide c'est bien, un `nginx` avec des fichiers c'est mieux ! On se propose de créer l'image d'un serveur web diffusant cette photo de chat :  
![](https://avatars0.githubusercontent.com/u/3159134?s=460&u=fca5f7a1912acbfb786e4933f44f927bf0288313&v=4)

- Télécharger cette image et la placer à côté du `Dockerfile` (on la nommera `miaouh.jpeg`)

On repart du Dockerfile précédent :

```Dockerfile
FROM nginx
```

on ajoute la photo :

```Dockerfile
ADD miaouh.jpeg /usr/share/nginx/html/miaouh.jpeg
```

Au final, notre Dockerfile fait 2 lignes :

```Dockerfile
FROM nginx
ADD miaouh.jpeg /usr/share/nginx/html/miaouh.jpeg
```

Il ne nous reste qu'à demander à docker de rebuilder l'image :

```
docker build -t monnginx .
```

Félicitations, vous venez de construire **votre première véritable image**.

On peut lancer un conteneur de notre image :

```
docker run -p 5555:80 monnginx
```

et apprécier le chat sur `http://localhost:5555/miaouh.jpeg`

### Publier (pusher) une image

Une fois une image construite, on peut la publier en utilisant `docker push`.  
En reprenant le chapitre précédent, on pourrait tenter de publier l'image `monnginx` :

```
docker push monnginx
```

Une image non préfixée est, par défaut, pushée vers le registry central : `dockerhub`. Ce registry est évidemment soumis à authentification, on va donc se manger un stop.

Il existe un registre non authentifié ouvert à tous avec une durée de vie des images limitée à 24h : https://ttl.sh/

On va donc renommer notre image (re-tag) et la pusher vers ce nouveau registre :

```
docker tag monnginx ttl.sh/lenomdevotrechoix:24h
docker push ttl.sh/lenomdevotrechoix:24h
```

Félicitations ! Tout le monde peut maintenant profiter de votre image simplement en faisant `docker run ttl.sh/lenomdevotrechoix:24h`
