# tp-coaching-webforce3
Instruction pour la session de coaching
# tp-coaching-webforce3
Instruction pour la session de coaching

## Exercice 7: Compare les codes Ansible 
Les deux scripts Ansible permettent également d'installer Docker sur une machine Ubuntu.

Le premier script utilise la tâche "apt" pour mettre à jour le cache APT, installer les paquets prérequis, ajouter la clé GPG officielle de Docker, ajouter le dépôt Docker et installer les packages Docker. Enfin, il ajoute l'utilisateur actuel au groupe "docker" pour permettre à l'utilisateur de lancer des commandes Docker.

Le deuxième script utilise également la tâche "apt" pour mettre à jour le cache APT et installer les paquets prérequis. Il ajoute la clé GPG officielle de Docker, ajoute le dépôt Docker et installe les packages Docker. Enfin, il utilise la commande "usermod" pour ajouter l'utilisateur "ubuntu" au groupe "docker" et utilise le module "pip" pour installer le package Docker.

Dans les deux scripts, l'option "become: true" est utilisée pour permettre à Ansible de devenir super-utilisateur (root) pour exécuter les commandes nécessaires.

écrit un dockerfile ubuntu web flask python.

## Exercice 8: Comparez les Dockerfiles

Bien sûr, voici un exemple de Dockerfile qui vous permettra de créer une image Docker pour exécuter une application Web Flask Python sur Ubuntu :

# Utilisez Ubuntu comme image de base
FROM ubuntu

# Mettre à jour les packages et installer Python et pip
RUN apt-get update && \
    apt-get install -y python3 python3-pip

# Copiez les fichiers de l'application dans le conteneur
COPY . /app

# Définissez le répertoire de travail comme le répertoire racine de l'application
WORKDIR /app

# Installez les dépendances de l'application
RUN pip3 install -r requirements.txt

# Exposez le port 5000 pour l'application Flask
EXPOSE 5000

# Définissez la commande pour lancer l'application
CMD ["python3", "app.py"]
Dans ce Dockerfile, nous avons utilisé Ubuntu comme image de base, mis à jour les packages et installé Python et pip. Ensuite, nous avons copié les fichiers de l'application dans le conteneur et défini le répertoire de travail comme le répertoire racine de l'application. Nous avons également installé les dépendances de l'application à l'aide de pip3. Enfin, nous avons exposé le port 5000 pour l'application Flask et défini la commande pour lancer l'application.

Notez que vous devrez adapter ce Dockerfile en fonction de votre propre application Flask Python et de ses dépendances.


écrit un dockerfile alpine web flask python
Certainement, voici un exemple de Dockerfile pour créer une image Docker pour exécuter une application Web Flask Python sur Alpine :

# Utilisez Alpine comme image de base
FROM alpine:latest

# Mettre à jour les packages et installer Python et pip
RUN apk update && \
    apk add python3 py3-pip

# Copiez les fichiers de l'application dans le conteneur
COPY . /app

# Définissez le répertoire de travail comme le répertoire racine de l'application
WORKDIR /app

# Installez les dépendances de l'application
RUN pip3 install -r requirements.txt

# Exposez le port 5000 pour l'application Flask
EXPOSE 5000

# Définissez la commande pour lancer l'application
CMD ["python3", "app.py"]
Dans ce Dockerfile, nous avons utilisé Alpine comme image de base, mis à jour les packages et installé Python et pip. Ensuite, nous avons copié les fichiers de l'application dans le conteneur et défini le répertoire de travail comme le répertoire racine de l'application. Nous avons également installé les dépendances de l'application à l'aide de pip3. Enfin, nous avons exposé le port 5000 pour l'application Flask et défini la commande pour lancer l'application.

Notez que vous devrez adapter ce Dockerfile en fonction de votre propre application Flask Python et de ses dépendances.

# pour voir la tailles des images 
sudo docker images 

## Exercice 9: Démarrez votre container Web Flask Serveur

Pour démarrer le container nommé web sur le port 30101, vous pouvez utiliser la commande Docker suivante :


docker run -d -p 30101:80 --name web myflaskapp
Cette commande démarre un nouveau conteneur à partir de l'image myflaskapp avec le nom web et le relie au port 30101 de votre machine hôte.

Pour vérifier les logs de votre conteneur, vous pouvez utiliser la commande suivante :

docker logs web

Pour modifier votre application pour que les données de logging soient placées dans le disque de stockage de l'exercice 3, vous devez modifier la configuration de logging de votre application Flask. Voici un exemple de code pour configurer le logging pour enregistrer les logs dans un fichier app.log :

import logging
from logging.handlers import RotatingFileHandler

app = Flask(app.py)

# Configuration du logging
handler = RotatingFileHandler('/path/to/exercise3/app.log', maxBytes=10000, backupCount=1)
handler.setLevel(logging.INFO)
app.logger.addHandler(handler)

# ... autres configurations et définitions de routes ...
Cette configuration enregistrera tous les logs de l'application Flask dans le fichier /path/to/exercise3/app.log.

Si vous rencontrez des problèmes avec votre application ou votre conteneur, vous pouvez utiliser les commandes docker ps pour lister les conteneurs en cours d'exécution et docker inspect pour obtenir des informations détaillées sur un conteneur particulier.

Pour vérifier si votre application fonctionne dans un navigateur sur le port 30101, ouvrez votre navigateur et accédez à l'URL http://localhost:30101. Si tout est correctement configuré, vous devriez voir votre application Flask en cours d'exécution.

## Exercice 10 : Mettre votre image Docker dans Docker Hub

https://hub.docker.com/repository/docker/taniad/webflaskalpine
https://hub.docker.com/repository/docker/taniad/webflaskpython


## Exercice 11: Créer un fichier docker-compose de 3 containers.

version: '3'

services:
  db:
    image: bitnami/mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: mydatabase
    volumes:
      - db_data:/var/lib/mysql
    logging:
      driver: json-file
      options:
        max-size: 10m
    networks:
      - mynetwork

  app:
    image: bitnami/phpmyadmin
    restart: always
    depends_on:
      - db
    ports:
      - "8080:80"
    logging:
      driver: json-file
      options:
        max-size: 10m
    networks:
      - mynetwork

networks:
  mynetwork:

volumes:
  db_data:
container web
```yaml
ports:
      - "30101:5000"
```
container db
```yaml
ports:
      - "30432:5432"
```
container app
```yaml
ports:
      - "30500:80"
```

## Exercice 12: Persistance des données dans votre script docker-compose.yml 

Voici le fichier docker-compose.yml modifié pour prendre en compte la persistance des données :

version: '3'

services:
  db:
    image: bitnami/mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: mydatabase
    volumes:
      - data:/bitnami/mariadb
    networks:
      - mynetwork

  web:
    build:
      context: .
      dockerfile: Dockerfile
    restart: always
    ports:
      - "8000:8000"
    volumes:
      - ./log:/app/log
    depends_on:
      - db
    networks:
      - mynetwork

networks:
  mynetwork:

volumes:
  data:
    driver: local
    driver_opts:
      type: none
      o: bind
      device: /home/ubuntu/tp-coaching-webforce3/log

Dans cet exemple, j'ai défini un volume nommé data pour stocker les données de la base de données. Ce volume est défini comme un point de montage de type bind, qui pointe vers le répertoire log sur le disque de l'exercice 3. Le conteneur db utilise ce volume pour stocker les données de la base de données.

Le conteneur web est configuré pour stocker les fichiers journaux dans le répertoire log sur le disque de l'exercice 3. J'ai ajouté un point de montage pour ce répertoire dans la section volumes du conteneur web.

Lorsque on démarrez les conteneurs en utilisant docker-compose up, le volume data sera créé sur le disque de l'exercice 3 et sera utilisé pour stocker les données de la base de données. Les fichiers journaux du conteneur web seront également stockés sur le disque de l'exercice 3, dans le répertoire log.