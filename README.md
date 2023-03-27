# tp-coaching-webforce3
Instruction pour la session de coaching
Instruction pour la session de coaching

# on commence pour faire un gitclone 

git clone https://github.com/crunchy-devops/tp-coaching-webforce3.git

# creer un scrum/kanban pour le projet

# verifier la version python et creer un alias creer un alias python 3

python3 --version
alias python=python3
python -V

# installer flask

sudo apt install python3-flask

# verifier la VM de 1GB

sudo fdisk -l

Disk /dev/vdc: 1 GiB, 1073741824 bytes, 2097152 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

sudo mount /dev/vdc /home/ubuntu/tp-coaching-webforce3/log
sudo vi /etc/fstab
#write file
dev/vdc /home/ubuntu/tp-coaching-webforce3/log

# Git/Github 
Dans PyCharm allez dans File->Settings->Version control->github 
et generer et copie le token

# Créez un fichier blogs.py  
Copier le script python suivant  
Le script ci-dessus est un script Python utilisant le framework Flask pour créer 
une application web simple qui affiche un message "Welcome to the Blog" lorsqu'on visite 
la page "/blogs". Le script utilise également le module de journalisation logging pour 
enregistrer des messages de journalisation dans un fichier.

Voici une analyse détaillée du script avec des commentaires :

# Importer le module Flask pour créer l'application web
from flask import Flask

# Importer le module logging pour enregistrer les messages de journalisation
import logging

# Créer une instance de l'application Flask avec le nom du module (__name__)
app = Flask(__name__)

# Configurer le module de journalisation pour enregistrer les messages de journalisation dans le fichier "log/record.log"
# avec un niveau de journalisation minimum de DEBUG
# Le format des messages de journalisation inclut la date et l'heure de l'enregistrement, le niveau de journalisation, 
# le nom du logger, le nom du thread et le message lui-même.

logging.basicConfig(filename='log/record.log', level=logging.DEBUG, format=f'%(asctime)s %(levelname)s %(name)s %(threadName)s : %(message)s')

# Définir une route pour la page "/blogs"
@app.route('/blogs')
def blog():
    # Enregistrer un message de journalisation de niveau INFO
    app.logger.info('Info level log')
    # Enregistrer un message de journalisation de niveau WARNING
    app.logger.warning('Warning level log')
    # Retourner un message de bienvenue à l'utilisateur
    return f"Welcome to the Blog"

# Démarrer l'application Flask sur l'hôte "localhost" avec le mode debug activé
app.run(host='localhost', debug=True)

# Ajouter une variable d'environnement 

nano ~/.bashrc
```FLASK_APP=blogs``` , mettre cette variable dans le fichier en bas.
sudo flask run --host=0.0.0.0 -p 30101

verifier dans le navigateur 
un message "welcome to the blog" s'affiche dans le navigateur.

# pare-feu

sudo ufw allow 30101 
sudo ufw deny 5000
sudo ufw enable
 pour verifier 
sudo ufw status
