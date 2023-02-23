# Instructions pour les TPs

---

Forker dans votre compte github le repo https://github.com/crunchy-devops/tp-coaching-webforce3.git  
Faire un git clone de ce repo en local dans votre directory c:\projet  
Faire un git clone dans la home directorie de votre VM fournie 
Ouvrir la directorie **tp-coaching-webforce3** dans PyCharm.

## Résultats et attentes
Vous devez compléter le fichier README.md avec toutes les instructions nécessaires pour
faire les différents exercices.

## Exercice 1  - Scrum 
Voici les détails de ce mini-projet.  
*Installer sur une VM (fournie), un serveur Web en Python Flask fonctionnant sur le port 30101. 
Ce serveur écrit les actions des utilisateurs dans un disque attaché à la VM. 
écrire les instructions pour le pare-feu*

**Préparer le dashboard Scrum pour ce projet.**

Utilisez un outil de dashboard Scrum/Kanban de votre choix, ou simplement créer un project dans github
Faire une copie d'écran de votre dashboard et placez le fichier de l'image 
dans git/github pour qu'il soit versionné 


## Exercice 2  - Linux 
Mettre à jour les packages de votre VM ubuntu  
Vérifier la version de python3 déjà installée  
Le programme python est executé avec le nom python3, créer un alias nommé python valide pour le user ubuntu de votre VM 
vérifier en faisant  ```python -V```  
Faire un ```pip install flask``` , suivre les instructions pour installer pip si nécessaire

## Exercice 3  - Storage 

Recherche le disque supplémentaire de 1Gb connecté à la VM, précisez la commande utilisée
Formattez ce disque au format ext4  
Monter (mount) ce disque sur le point montage /home/ubuntu/tp-coaching-webforce3/log


## Exercice 4  - Git/Github 
Dans PyCharm allez dans File->Settings->Version control->github  
Appuyer sur la croix, en haut a gauche de cette fenetre et selectionnez log in with token.   
entrez votre token github  
Vous pouvez maintenant faire des git commit et git push depuis PyCharm   

Faire régulierement des commit/push dans github de votre machine local vers github
Comme vous avez fait un git clone de votre projet sur votre VM, vous devez
faire des git pull pour mettre jour votre Web serveur sans utiliser d'éditeur 
dans votre VM, ne pas faire de git push à partir de la VM sinon vous risquez 
d'avoir des conflits à résoudre entre les push faits de votre machine locale et ceux faits à partir de la VM. 

## Exercice 5  - Python
Créez un fichier blogs.py  
Copier le script python suivant  

```python
from flask import Flask
import logging
 
app = Flask(__name__)
 
logging.basicConfig(filename='log/record.log', level=logging.DEBUG, format=f'%(asctime)s %(levelname)s %(name)s %(threadName)s : %(message)s')
 
@app.route('/blogs')
def blog():
    app.logger.info('Info level log')
    app.logger.warning('Warning level log')
    return f"Welcome to the Blog"
 
app.run(host='localhost', debug=True)
```
Avec l'aide de la documentation Flask, et de la documentation Python mettre des commentaires 
dans ce script.

Ajouter une variable d'environnement ```FLASK_APP=blogs``` , mettre cette variable dans le fichier ~/.bashrc
de votre user ubuntu  
Lancer le web server avec la commande ```flask run --host=0.0.0.0 -p 30101```  
Vérifier avec votre navigateur en utilisant l'url ```http://<ip_de_votre_vm>:30101/blogs```    
Vérifier que le fichier record.log existe bien dans la directory log    


## Exercice 6  - Pare-feu  
Trouvez la commande de gestion du firewall sous ubuntu 20.04
Exemple : fermer le port 5000 et autoriser le port 30101
Vérifier l'application Web sur ces ports


# TP sur Docker

---
**Creez un compte DockerHub https://hub.docker.com/**
**Créez un compte chatGPT sur https://openai.com/blog/chatgpt/**  
**Mettez vous sur la branche git nommée docker**  

Dans votre VM (fournie), faire les instructions du fichier    
   ~/tp-coaching-webforce3/install_docker/DOCKER.md

Allez dans chatGPT et tapez :  
     **code ansible de l'installation de docker**


## Exercice 7: Compare les codes Ansible 
Comparez mon code ```install_docker_ubuntu.yml``` avec le code généré par chatGPT  
Ajouter des commentaires 


## Exercice 8: Comparez les Dockerfiles

Générer avec chatGPT le dockerfile de l'application blogs.    
Allez dans chatGPT et tapez :      
     **écrit un dockerfile ubuntu web flask python**    
Faire la mise au point du script généré.  
Tapez la commande pour voir la taille de l'image docker.   

Allez dans chatGPT et tapez :      
     **écrit un dockerfile alpine web flask python**  
Faire la mise au point du script généré.  
Tapez la commande pour voir la taille de l'image docker.    

Choisir le dockerfile de l'image la plus petite pour la suite du TP.   

## Exercice 9: Démarrez votre container Web Flask Serveur  

Précisez la commande pour démarrer le container nommé **web** sur le port 30101.    
Vérifier les logs de votre container.   
Modifier votre application pour que les données de logging soient placées dans le 
disque de stockage de l'exercice 3.    
Troubleshooter l'application et le container.     
Vérifier si votre application fonctionne dans un navigateur, port 30101.  
---
Dans votre VM (fournie), faire les instructions du fichier    
   ~/tp-coaching-webforce3/projet-docker-compose/DOCKER_COMPOSE.md

## Exercice 10 : Mettre votre image Docker dans Docker Hub

Tagger l'image avec votre compte Docker hub, faire un ```docker login```
et ```docker push```  
Testez votre image mise dans docker hub en demarrant un nouveau container avec ```docker run``` 

## Exercice 11: Créer un fichier docker-compose de 3 containers

Faire un ```cd ~/tp-coaching-web-force3```  
Allez dans chatGPT et tapez :      
     **écrit un docker-compose de 3 containers et un network**    

Dans ce script généré que vous nommez docker-compose.yml, changez l'image du container web, elle doit etre celle de l'exercice 5      
L'image de la base de données postgresql doit etre celle de **bitnami/postgresql**  
suivre les intructions pour utiliser postgresql  
L'image du container app doit etre **dpage/pgadmin4**     
Vérifier les logs de chaque container. 
---
### Port forwarding a preciser dans votre docker-compose  

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

### Installation de docker-compose
Appliquez les commandes du fichier project-docker-compose/DOCKER_COMPOSE.md  

## Exercice 12: Persistance des données dans votre script docker-compose.yml  

Mettez en place la persistance des données des containers: web et db.    
Le container web doit écrire les data de log dans la directory log sur le disque défini de l'exercice 3.
La base de données postgresql doit etre dans un docker volume nommé **data** dans la directory log sur le disque de l'exercice 3.
Démarrez les containers et vérifiez les logs des containers
Faire la mise au point, attention aux erreurs de permission et de proprietaire des directories 
---
Tips:   
Le volume docker pour le container db doit etre défini comme cela: 
```yaml
volumes:
  data:
    driver: local
    driver_opts:
      type: none
      o: bind
      device: /home/ubuntu/tp-coaching-webforce3/log
```
Le mapping de volume pour le container db est :  
```yaml
volumes:
      - data:/bitnami/postgresql
```

# TP Kubernetes with kind

## Install the cluster
```shell
cd   # back to the home directory
docker ps   # check the docker containers
docker --version  # check docker version 
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.17.0/kind-linux-amd64 # version of kind
chmod +x ./kind # it's now executable
sudo mv ./kind /usr/local/bin/kind # move to /usr/local/bin available for all users
kind version  # check the version of kind 
cd ~/tp-coaching-webforce3/kind # change to the project
kind create cluster --name=webforce3  --config kind-config.yml # install kind kubernetes cluster # Notice: should be version 1.25.3
# work in a container to keep my vm/desktop clean
docker run -d -it --rm --name work -v ${HOME}:/root/ -v ${PWD}:/work -w /work --net host alpine sh
docker exec -it work ash 
```

## Install kubectl
```shell
# inside the container named work , beware it's alpine OS 
apk add --no-cache --virtual .build-deps make bash gcc musl-dev openssl go curl vim # Install useful packages
cd /root
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" # Download a version of kubectl
install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl # install kubectl
kubectl version --short # Check 
alias k='kubectl'  # useful alias
k get nodes  # Kubernetes command for checking the cluster
```

## Exercice Kubernetes 1 : Porter le container web dans kubernetes

Ecrire le Deployment yaml code pour l'application blogs.  
**Tips:** Voir l'application Kompose https://kompose.io/ pour obtenir un exemple de scripts    
Dans le script web-deployment.yaml mettre votre docker hub image  
Dans le script web-service.yaml modifier votre script en specifiant le NodePort identique a celui du Cluster    
que vous pouvez voir dans le fichier kind-config.yml    
Mettre le changement dans web-service.yaml
```yaml
...
spec:
  type: NodePort
  ports:
    - name: "5000"
      port: 5000
      targetPort: 5000
      nodePort: 30950
```

Testez avec votre navigateur avec le port 30950, qui doit etre ouvert par le pare-feu 

## Exercice Kubernetes 2 : Installer EDB-postgresql dans kubernetes 
Dans votre container work 
Nous allons utiliser un script kubernetes/yaml fourni par la societe EDB pour installer Postgresql.   
```shell
kubectl apply -f https://get.enterprisedb.io/cnp/postgresql-operator-1.18.0.yaml # Run the operator
kubectl get deploy -n postgresql-operator-system postgresql-operator-controller-manager # check 
k get pod -A 
```
### Create postgresql cluster 
Creer ce fichier nomme cluster-example.yaml avec le code suivant:
```yaml
# Example of PostgreSQL cluster
apiVersion: postgresql.k8s.enterprisedb.io/v1
kind: Cluster
metadata:
  name: cluster-example
spec:
  instances: 3

  # Example of rolling update strategy:
  # - unsupervised: automated update of the primary once all
  #                 replicas have been upgraded (default)
  # - supervised: requires manual supervision to perform
  #               the switchover of the primary
  primaryUpdateStrategy: unsupervised

  # Require 1Gi of space
  storage:
    size: 1Gi
```
Appliquer le script dans kubernetes: 
```shell
k apply -f cluster-example.yaml
```
Faire des ```k get pod``` plusieurs fois de suite pour voir que les pods cluster-example-1, 2 et 3 sont disponibles. 

**Retrouver la version de Postgresql dans le fichier yaml du cluster**   
commande 
```shell
k get cluster cluster-example -o yaml
```


### Obtenir les Secrets
```shell
k get Secret cluster-example-app -o yaml # find the secret
# select username
echo "YXBw" | base64 -d # get the username
echo "YTHxxxx...xxx" | base64 -d # get the password
kubectl port-forward service/cluster-example-rw 3000:5432 --address='0.0.0.0'
```
**Verifier la connectivite a la base donnees avec goland de jetbrains avec l'onglet a droite nomme database**











