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
