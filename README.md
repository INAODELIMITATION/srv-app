# srv-app : montage du serveur sig-inao.fr

## Procédure d'installation du serveur : 

- Installation de Docker 
   ```
   apt-get update 
   apt-get install apt-transport-https ca-certificates curl software-properties-common 
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg |apt-key add - 
   add-apt-repository "deb [arch=amd64]    https://download.docker.com/linux/debian $(lsb_release -cs) stable"
   apt-get update
   apt-get install docker-ce

   ```

- Installation de Docker-Compose
   ```
   curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
   chmod +x /usr/local/bin/docker-compose
   ``` 
- Creation des répertoires suivant sur le serveur : 
  - **/home/app/config/config.json** : persistance des paramètre de conexion à la base de données.
  - **/home/certs** : certificats let's encrypt.
  - **/home/geoserver** : persistance des tuiles vectoriels. 
  - **/home/postgres** : persistance de la base de données. 

  ```
  mkdir /home/app/config/config.json
  mkdir /home/certs
  mkdir /home/geoserver
  mkdir /home/postgres
  ```

- Installation de git & récupération du répertoire 'srv-app'
   ```
   apt-get update
   apt-get install nano git
   cd /home
   git clone https://github.com/INAODELIMITATION/srv-app.git   
   cd srv-app
   ```
- Lancement des micro-services
  ```
  docker-compose up -d
  ```

- Suppression des micro-services
  ```
  docker-compose stop && docker-compose rm
  ```

## Système de sauvegarde d'OVH
OVH nous met à disposition un espace de 500Go de sauvegarde séparé de notre serveur mais hebergé par leurs services. 

- Effectuer une sauvegarde (mot de passe disponible dans le fichier mdp.txt)  :
```
tar czf - /data | ncftpput -u ns3045058.ip-51-255-93.eu -p mot-de-passe  -c ftpback-rbx3-186.ovh.net data.tar.gzapt
```
