# TP Introduction devOps 
#### `Hugo Hersemeule, Tom Kubasik`

## Explication du projet
Le projet est constitué d'un jeu d'arcade dévelopé sur Unity pour WebGL. 
Le but du jeu est de finir le circuit le plus rapidement possible.

Quand on fini un tour, notre temps est envoyé sur un backend node.js qui stoque les scores dans une base MariaDb.

## Docker
Nous avons un _docker compose_ qui orchestre 3 containers :
 - MariaDB sur le port 3306
 - Server Node sur le port 8000
 - Serveur Http Nginx pour le client sur le port 81

Dans le Dockerfile du client, on copy le jeu contenu dans ./client/app/

Pour lancer l'orchestration, on lance la commande 

``` bash
docker-compose up --build
```

## Kubernetes
Nous avons trois yaml, un pour le serveur, un pour le web, un pour le mariaDB.

Les images du serveur node et du client ont été push au préalable sur docker Hub.

MariaDB étant accédé uniquement par le Node, il n'est pas ouvert vers l'exerieur et est accessible grace a un service de type ClusterIP.

Sachant que le client unity doit accédé au serveur node depuis l'extérieur du minikube, les service node et webserver sont des LoadBalancer.

Setup:
Install Minikube

``` bash
minikube start
kubectl apply -f mariadb.yaml
kubectl apply -f web.yaml
kubectl apply -f server.yaml

minikube tunnel
```

On accède a jeu sur 127.0.0.1:80


















