# tp-coaching-webforce3
Instruction pour la session de coaching
COMMENTAIRES TP ansible 2

Analise du fichier ansible-2-filtre.yml

Le script est écrit en YAML et est destiné à être utilisé avec l'outil d'orchestration de configuration Ansible. Il comporte une tâche unique qui est de formater un disque dur.

La première tâche, intitulée "get disk structure", utilise la commande "fdisk -l" pour récupérer la structure du disque et stocke la sortie dans la variable "get_disk".

La seconde tâche, nommée "pour la mise au point", utilise le module "debug" d'Ansible pour afficher un message de débogage contenant le nom du dispositif enregistré dans la variable "get_disk". Le filtre "get_device" est utilisé pour extraire le nom du dispositif à partir de la sortie de la commande fdisk.

Le script utilise également la directive "become: true" pour s'assurer que les tâches sont exécutées en tant que superutilisateur.

En somme, ce script permet de récupérer la structure d'un disque dur et d'afficher le nom du dispositif correspondant à cette structure pour faciliter la mise au point du processus de formatage.

2) Filter_plugins my_filter get_device

a_filter : ce filtre prend une variable en entrée et ajoute la chaîne de caractères "CRAZY NEW FILTER" à la fin de la variable. Il retourne la nouvelle variable modifiée.

latest_version : ce filtre prend une liste de versions en entrée et retourne la version la plus récente. Pour ce faire, il utilise la librairie Python natsort pour trier la liste de versions de manière naturelle (afin de prendre en compte les numéros de version et les points). Il parcourt ensuite la liste triée de la fin vers le début et utilise une expression régulière pour extraire la version la plus récente, qui commence par la lettre 'v' suivie d'un chiffre suivi d'un point suivi d'un autre chiffre.

get_device : ce filtre prend une liste de périphériques de disque en entrée et retourne une liste des périphériques qui ne sont pas formatés avec l'un des types de systèmes de fichiers spécifiés. Il parcourt d'abord la liste de périphériques pour extraire les disques. Il boucle ensuite sur les disques et utilise la commande "lsblk -f" pour récupérer les informations sur le système de fichiers de chaque disque. Si le système de fichiers n'est pas l'un des types spécifiés, le périphérique est ajouté à la liste des périphériques à retourner.

En somme, ce script fournit trois filtres personnalisés pour effectuer des tâches spécifiques dans Ansible, notamment la modification de variables, la récupération de la dernière version dans une liste de versions et la récupération d'une liste de périphériques de disque qui ne sont pas formatés avec des types de systèmes de fichiers spécifiques.

la fonction "get_device" est utilisée pour extraire le nom du dispositif à partir de la sortie de la commande "fdisk -l". Elle utilise une expression régulière pour trouver la ligne qui contient le nom du dispositif et retourne la partie de la ligne correspondant au nom du dispositif. Ce filtre est utile pour faciliter la mise au point du processus de formatage du disque dur.

3)Ansible.cfg

Ce script est une configuration pour Ansible, il est destiné à être utilisé dans le fichier de configuration des paramètres par défaut (defaults) d'Ansible.

Les commentaires possibles pour chaque ligne sont les suivants :

deprecation_warnings=False : ce paramètre désactive les avertissements de dépréciation dans Ansible.

host_key_checking=False : ce paramètre désactive la vérification de la clé d'hôte lors de la connexion SSH.

ssh_args = -o ControlMaster=auto -o ControlPersist=30m : ce paramètre définit les arguments à utiliser lors de la connexion SSH. Ici, il active la fonctionnalité de contrôle de session SSH (ControlMaster) et spécifie que la session doit rester active pendant 30 minutes après la déconnexion (ControlPersist).

command_warnings=False : ce paramètre désactive les avertissements de commande dans Ansible.

pipelining=True : ce paramètre active le pipeline de tâches Ansible, ce qui permet d'optimiser la performance en envoyant plusieurs tâches simultanément.

callback_whitelist = profile_tasks : ce paramètre spécifie que seul le callback profile_tasks doit être utilisé. Le callback profile_tasks permet d'afficher les temps d'exécution de chaque tâche dans la sortie d'Ansible.

filter_plugins = filter_plugins : ce paramètre spécifie le répertoire où les plugins de filtre personnalisés doivent être stockés. Les plugins de filtre sont des scripts Python qui peuvent être utilisés pour manipuler les données dans les playbooks Ansible.

4)Modification du fichier ansible-2-filtre.yml

---
- name: format disk
  become: true
  hosts: localhost
  tasks:
    - name: get disk structure
      become: true
      command: fdisk -l
      register: get_disk
    - name: format disk
      become: true
      command: mkfs.ext4 {{ get_disk.stdout | get_device }}

Cette modification ajoute une nouvelle tâche qui utilise la commande mkfs.ext4 pour formater le disque obtenu à partir de la sortie de la commande fdisk -l.

La commande mkfs.ext4 est utilisée pour créer un système de fichiers ext4 sur le disque spécifié.

L'argument {{ get_disk.stdout | get_device }} est utilisé pour spécifier le nom du disque à formater. Cette valeur est obtenue en appelant la fonction get_device qui retourne le nom du premier disque qui ne contient pas de système de fichiers.

4) TP-4

creation du fichier ansible_inventory.yml dans le dossier tp-coaching-webforce3/almalinux 
[alma]
target2  ansible_host=172.17.0.4 ansible_ssh_user=root ansible_ssh_private_key=/home/tania/tp-coaching-webforce3/ansible/toor
pour executer le fichier:
ansible alma -i ansible_inventory.yml -m ping 

Commentaire de webfoce/postgressql/tasks/variables.yml
 
Ce code utilise la tâche Ansible "include_vars" pour inclure des variables spécifiques à l'OS (Alma) dans le playbook. La variable "ansible_distrib" est utilisée pour spécifier le nom de la distribution de l'OS.
# Variables configuration
- name: Include OS-specific variables (Alma)
  include_vars: "{{ ansible_distrib

-Pour quoi on a besoin d'un Handler?

Un handler est un type de tâche Ansible qui est exécuté uniquement lorsque les changements sont détectés sur les tâches précédentes. Il est utile pour effectuer des actions qui doivent être déclenchées seulement à la fin d'une série de tâches.

Par exemple, si vous avez une tâche pour redémarrer un service, vous pouvez créer un handler pour redémarrer ce service uniquement s'il y a eu des modifications dans la tâche précédente. Cela permet d'optimiser l'exécution de votre playbook en ne lançant pas des actions inutiles si elles ne sont pas nécessaires.

Les handlers sont également utiles pour éviter les conflits de tâches, par exemple si plusieurs tâches doivent redémarrer le même service. En utilisant un handler, tous les changements seront enregistrés et le service ne sera redémarré qu'une seule fois.


