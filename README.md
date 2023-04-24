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

arduino
Copy code
docker run -d -p 30101:80 --name web myflaskapp
Cette commande démarre un nouveau conteneur à partir de l'image myflaskapp avec le nom web et le relie au port 30101 de votre machine hôte.

Pour vérifier les logs de votre conteneur, vous pouvez utiliser la commande suivante :

Copy code
docker logs web
Pour modifier votre application pour que les données de logging soient placées dans le disque de stockage de l'exercice 3, vous devez modifier la configuration de logging de votre application Flask. Voici un exemple de code pour configurer le logging pour enregistrer les logs dans un fichier app.log :

python
Copy code
import logging
from logging.handlers import RotatingFileHandler

app = Flask(__name__)

# Configuration du logging
handler = RotatingFileHandler('/path/to/exercise3/app.log', maxBytes=10000, backupCount=1)
handler.setLevel(logging.INFO)
app.logger.addHandler(handler)

# ... autres configurations et définitions de routes ...
Cette configuration enregistrera tous les logs de l'application Flask dans le fichier /path/to/exercise3/app.log.

Si vous rencontrez des problèmes avec votre application ou votre conteneur, vous pouvez utiliser les commandes docker ps pour lister les conteneurs en cours d'exécution et docker inspect pour obtenir des informations détaillées sur un conteneur particulier.

Pour vérifier si votre application fonctionne dans un navigateur sur le port 30101, ouvrez votre navigateur et accédez à l'URL http://localhost:30101. Si tout est correctement configuré, vous devriez voir votre application Flask en cours d'exécution.