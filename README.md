# TP2

## Pré requis

Installation de Docker (voir le cours)

## EXO 1

Le but de cet exercice est de creer une image d'une application afin de pouvoir la deployer sur un conteneur. L'application en question est un serveur en golang s'executant sur le port 8080.

Vous êtes libre d'utiliser l'image que vous souhaitez

L'ensemble du code de l'application devra etre present dans un repertoire app dans l'image.

La commande à executer pour installer les dependences golang est:
```GO
go mod download
```  
Il sera necessaire de réaliser un build du code afin de pouvoir l'executer. Vous pouvez utiliser la commande suivante:
```GO
CGO_ENABLED=0 GOOS=linux go build -o /docker-gs-ping
```
Le serveur s'execute grace à la commande "/docker-gs-ping"

Une fois votre conteneur actif, test l'application avec la commande suivante:
```sh
curl -v localhost:8080
```


## EXO 2

Le but de cet exercice est d'optimiser une image en utilisant le multi build. Votre but va être de fournir une image ne contenant que le binaire executable de l'application. 

Quelques conseils:
- il est possible de donner un nom de référence à vos différents phase de builde en utilisant le format suivant :
```DOCKERFILE
FROM image_name AS ref_name
```
- decoupez votre Dockerfile en deux partie, une partie build où vous construisez votre binaire et une partie release où vous récupérerez uniquement le binaire et l'exécuté
- pour la partie release, vous pouvez utiliser l'image suivante:
```DOCKERFILE
FROM gcr.io/distroless/base-debian11
```
- Il est possible de copier des fichiers entre deux phases de build à l'aide de la commande suivante:
```DOCKERFILE
COPY --from=ref_name scr_file dest
```
- Le server devra s'executer en tant qu'utilisateur nonroot. Vous pouvez utiliser la commande suivante:
```DOCKERFILE
USER nonroot:nonroot
```
 
Une fois votre conteneur actif, test l'application avec la commande suivante:
```sh
curl -v localhost:8080
```