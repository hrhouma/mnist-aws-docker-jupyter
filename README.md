# Étape 1
- Lacez une instance AWS t2.xlarge
- se connecter via ssh

# Étape 2 - installer docker 
- sudo -s
- git clone https://github.com/hrhouma/install-docker.git
- cd install-docker/
- chmod +x install-docker.sh
- ./install-docker.sh
- docker version
- docker compose version
# Étape 3 - cloner le travail 
- cd install-docker/
- git clone https://github.com/hrhouma/mnist-aws-docker-jupyter.git
- cd mnist-aws-docker-jupyter

# Étape 4 - tester sur la machine virtuelle locale (sans Docker)

- apt update -y
- apt install docker-compose
- sudo pip3 install jupyter (ne fonctionne pas)
- sudo apt update && sudo apt upgrade -y
- sudo apt install python3-pip python3-dev python3-venv -y
- python3 -m venv monenv
- source monenv/bin/activate
- pip install jupyter
- (optinnel, deja fait ) git clone https://github.com/FraPochetti/KagglePlaygrounds.git
- (optinnel, deja fait ) cd KagglePlaygrounds/
- (optinnel, deja fait ) cd fashion_docker/
- cd mnist-aws-docker-jupyter (vérifier que je suis dans ce dossier)
- pip3 install -r requirements.txt
- pip install jupyter
- (optionnel) jupyter server list
- jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser --allow-root --NotebookApp.token='' --NotebookApp.password=''
- Le serveur est : mnist_convnet.ipynb et le client est prediction.ipynb
# serveur 
![image](https://github.com/hrhouma/mnist-aws-docker-jupyter/assets/10111526/3db70ec9-1bb2-4326-aa48-e60e38b28278)
# client
![image](https://github.com/hrhouma/mnist-aws-docker-jupyter/assets/10111526/db8c8587-e3b9-4fbd-b271-cb8ed1640609)

# adresses (uniquement jupyter) : 
- jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser --allow-root --NotebookApp.token='' --NotebookApp.password=''
- http://54.196.41.2:8888/
- http://54.196.41.2:8888/notebooks/mnist_convnet.ipynb
- http://54.196.41.2:8888/notebooks/prediction.ipynb

- (à la fin) history
- (à la fin) deactivate

# Étape 6 - tester avec Docker (TO DO , n'est pas fonctionnel à 100% pour le moment) 

- docker build -t fashion-mnist-app .
- docker run -p 5000:5000 --name fashion-mnist-container fashion-mnist-app
- docker ps
- docker ps -a
- docker images
- cat requirements.txt (nano requirements.txt en cas de besoin)
- (en cas de problèmes, supprimer les conteneurs et les images) 
- (en cas de problèmes) docker rm -f $(docker ps -a -q)
- (en cas de problèmes) docker rmi $(docker images -q)
- (en cas de problèmes) docker rmi $(docker images -q)
- (en cas de problèmes) modifier le Dockerfile avec nano Dockerfile et cat Dockerfile
- (en cas de problèmes) docker run -p 5000:5000 --name fashion-mnist-container fashion-mnist-app
- (en cas de problèmes) docker logs id_du_conteneur
- (en cas de problèmes) docker stop id_du_conteneur
- (en cas de problèmes) docker start id_du_conteneur

- (TO DO) *Laisser le conteneur rouler et tester un CLIENT*
- (TO DO) démarrer un client en utilisant un script python, streamlit (application web) , une interface desktop (tkinter) ou jupyter
- (TO DO) écrire un script app.py
- (TO DO) écrite un notebook comme le suivant :
- (TO DO NE FONCTIONNE PAS ) jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser --allow-root --NotebookApp.token='' --NotebookApp.password=''
- (TO DO NE FONCTIONNE PAS ) http://54.196.41.2:8888/notebooks/prediction.ipynb


# Étape 7 - Utile (les ports)

Pour gérer un serveur Flask écoutant sur le port 5000 et travailler avec des processus sur un système Linux, voici quelques commandes utiles :

1. **Trouver le processus écoutant sur le port 5000** :
   Utilisez la commande `lsof` pour lister les fichiers ouverts par les processus et trouver celui qui utilise le port 5000.
   ```
   sudo lsof -i :5000
   ```
   Cette commande affiche des informations sur le processus écoutant sur le port 5000, y compris son PID (identifiant de processus).

2. **Tuer le processus par le PID** :
   Après avoir identifié le PID du processus qui utilise le port 5000, vous pouvez le "tuer" avec la commande `kill`.
   ```
   kill <PID>
   ```
   Remplacez `<PID>` par l'identifiant de processus que vous avez trouvé avec la commande précédente.

3. **Utiliser `fuser` pour tuer le processus directement par le port** :
   La commande `fuser` permet de tuer tous les processus utilisant un port spécifique en une seule étape.
   ```
   sudo fuser -k 5000/tcp
   ```
   Cette commande arrête tous les processus qui écoutent sur le port 5000 TCP.

4. **Voir les processus avec `netstat`** :
   Vous pouvez également utiliser `netstat` pour voir les ports ouverts et les processus correspondants.
   ```
   sudo netstat -tulpn | grep 5000
   ```
   Cette commande filtre les informations pour ne montrer que celles concernant le port 5000.

Ces commandes sont couramment utilisées pour gérer des situations où un serveur comme Flask doit être redémarré ou lorsqu'un port doit être libéré.

# Étape 8 - Utile (les ports)

Pour arrêter Jupyter Notebook et l'exécution sur Flask dans le contexte que vous avez décrit, voici les étapes appropriées :

### Pour Jupyter Notebook :
1. **Trouver le processus de Jupyter Notebook :**
   Vous pouvez utiliser la commande `jupyter notebook list` pour voir les instances en cours. Cette commande vous donnera aussi les ports utilisés.

2. **Arrêter Jupyter Notebook :**
   Pour arrêter une instance spécifique de Jupyter Notebook, vous pouvez utiliser `Ctrl+C` dans le terminal où il s'exécute. Si vous avez démarré Jupyter en arrière-plan, utilisez le PID pour le tuer :
   ```bash
   kill $(pgrep -f 'jupyter-notebook')
   ```
   Cette commande recherche le processus de Jupyter Notebook et l'arrête.

### Pour Flask :
1. **Trouver le processus de Flask :**
   Utilisez la commande `lsof` ou `netstat` pour trouver le processus Flask écoutant sur un port spécifique (comme 5000).
   ```bash
   sudo lsof -i :5000
   ```
   ou
   ```bash
   sudo netstat -tulpn | grep 5000
   ```

2. **Arrêter Flask :**
   Une fois que vous avez le PID, utilisez la commande `kill` pour arrêter Flask :
   ```bash
   kill <PID>
   ```
   Remplacez `<PID>` par l'identifiant du processus que vous avez trouvé.

### Pour la machine virtuelle locale :
- Assurez-vous que toutes les étapes nécessaires soient effectuées dans l'ordre spécifié.
- N'oubliez pas d'activer votre environnement virtuel avant d'exécuter des commandes qui dépendent de paquets Python installés dans cet environnement.
- À la fin de vos tests, utilisez `deactivate` pour sortir de l'environnement virtuel.

Cela devrait vous aider à gérer proprement les processus de Jupyter et Flask sur votre machine virtuelle locale.

# Étape 9 - faire un push sur git
- git init
- git add .
- git commit -m "projet de session"
- git remote add origin https://github.com/hrhouma/mnist-aws-docker-jupyter.git
- git branch -M main
- git push -u origin main
- history
