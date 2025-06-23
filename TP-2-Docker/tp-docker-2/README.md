████████╗██████╗      ██╗  ██╗    
╚══██╔══╝██╔══██╗     ██║  ██║ 
   ██║   ██████╔╝     ██║  ██║  
   ██║   ██╔═══╝      ██║  ██║  
   ██║   ██║          ██║  ██║ 
   ╚═╝   ╚═╝          ╚═╝  ╚═╝
LAFON Rafael - B3 Cyber

-------------------------------------------------------------------------------------------------------------

Ci-dessous, toutes les commandes réalisées par ordre chronologique ainsi que leur retour dans le terminal,
le tout accompagné d'une note personnel sur l'utilité de chaque commande :

-------------------------------------------------------------------------------------------------------------

1.Téléchargement du ZIP.



2.Nouveau repository :

=> J'utilise le fichier décompresser dans le repository prééxistant



3.Modification du fichier Dockerfile.

=>  FROM node:12-alpine3.9
    WORKDIR /app
    COPY src/package*.json ./
    RUN npm install --only=production
    COPY src/ .
    EXPOSE 3000
    CMD ["node", "index.js"]

Note : Je vais crée un image de base dans le dossier de travail que j'aurais crée juste avant. Je vais copier le fichier json pour installer uniquement les dépendances nécessaires et lancer l'éxécution. Ensuite j'éxpose le port 3000 vie Express, puis je lance un commande pour lancer le serveur.



4.docker build -t masuperapp .

=> [+] Building 4.3s (10/10)                         FINISHED                                                                                                                                               docker:default
    => [internal] load build definition from Dockerfile                                                                                                                                       0.0s
    => => transferring dockerfile: 185B                                                                                                                                                       0.0s
    => [internal] load metadata for docker.io/library/node:12-alpine3.9                                                                                                                       1.4s
    => [internal] load .dockerignore                                                                                                                                                          0.0s
    => => transferring context: 2B                                                                                                                                                            0.0s
    => [internal] load build context                                                                                                                                                          0.0s
    => => transferring context: 939B                                                                                                                                                          0.0s
    => [1/5] FROM docker.io/library/node:12-alpine3.9@sha256:16d40e6c2858ee41cc7e19bb36f8a92718ad935ceae036e88dcffb68041dea6c                                                                 1.8s
    => => resolve docker.io/library/node:12-alpine3.9@sha256:16d40e6c2858ee41cc7e19bb36f8a92718ad935ceae036e88dcffb68041dea6c                                                                 0.0s
    => => sha256:16d40e6c2858ee41cc7e19bb36f8a92718ad935ceae036e88dcffb68041dea6c 1.43kB / 1.43kB                                                                                             0.0s
    => => sha256:ed9251aca55330890ef48a274c6ce03052e5438e87b6101b0bab5362ac79b5e5 1.16kB / 1.16kB                                                                                             0.0s
    => => sha256:a7d6e4c06dd4f80d3ba52db58f3c7421f6c29497ddf084928103d71b6635314c 6.73kB / 6.73kB                                                                                             0.0s
    => => sha256:31603596830fc7e56753139f9c2c6bd3759e48a850659506ebfb885d1cf3aef5 2.77MB / 2.77MB                                                                                             0.2s
    => => sha256:f5b0d1ce6f59dcbee12d1ce6f4093fa9a7939c7751f00ff2137b4651dcf2de8f 24.49MB / 24.49MB                                                                                           1.0s
    => => sha256:55922cf6f31687a003e818aab6c5e42db644b2be19a1c74040d750db3d63354c 2.24MB / 2.24MB                                                                                             0.4s
    => => extracting sha256:31603596830fc7e56753139f9c2c6bd3759e48a850659506ebfb885d1cf3aef5                                                                                                  0.0s
    => => sha256:e8a7c0650cafa577c71ba40f4423d3486d700990904de11327a69bdff61d1a92 283B / 283B                                                                                                 0.4s
    => => extracting sha256:f5b0d1ce6f59dcbee12d1ce6f4093fa9a7939c7751f00ff2137b4651dcf2de8f                                                                                                  0.7s
    => => extracting sha256:55922cf6f31687a003e818aab6c5e42db644b2be19a1c74040d750db3d63354c                                                                                                  0.0s
    => => extracting sha256:e8a7c0650cafa577c71ba40f4423d3486d700990904de11327a69bdff61d1a92                                                                                                  0.0s
    => [2/5] WORKDIR /app                                                                                                                                                                     0.1s
    => [3/5] COPY src/package*.json ./                                                                                                                                                        0.0s
    => [4/5] RUN npm install --only=production                                                                                                                                                0.9s
    => [5/5] COPY src/ .                                                                                                                                                                      0.0s
    => exporting to image                                                                                                                                                                     0.0s
    => => exporting layers                                                                                                                                                                    0.0s
    => => writing image sha256:12bdc2ae41c004b042f1bb167a3dd388f41d2afa6fd6ebbfaad5810cf7807417                                                                                               0.0s
    => => naming to docker.io/library/masuperapp                   



5.Vérification du build : docker images

=>  REPOSITORY                 TAG       IMAGE ID       CREATED          SIZE
    masuperapp                 latest    12bdc2ae41c0   20 seconds ago   88.5MB
    image-httpd-container      latest    a6804a90214b   2 weeks ago      148MB
    nginx                      latest    be69f2940aaf   2 months ago     192MB
    phpmyadmin                 latest    a227ea564a55   4 months ago     570MB
    httpd                      latest    958373fdd7e8   5 months ago     148MB
    mysql                      5.7       5107333e08a8   18 months ago    501MB
    praqma/network-multitool   latest    1631e536ed7d   3 years ago      39.9MB



6.QUESTION : 2.3 : Quelle option npm permet d’installer seulement le nécessaire ?

=> "--only=production" une option de npm install qui permet d’installer uniquement les dépendances nécessaires, pour aléger l'image docker et minimiser la surface d'attaque.



7.Je complète le fichier docker-compsoe.yml

=>  version: '3.9'

    services:
    app:
        build: ./src
        container_name: express_app
        ports:
        - "3000:3000"
        environment:
        - DB_HOST=db
        - DB_USER=appuser
        - DB_PASSWORD=apppass
        - DB_NAME=appdb
        depends_on:
        - db

    db:
        image: mysql:8
        container_name: express_db
        restart: always
        environment:
        - MYSQL_DATABASE=appdb
        - MYSQL_USER=appuser
        - MYSQL_PASSWORD=apppass
        - MYSQL_ROOT_PASSWORD=rootpass
        volumes:
        - db_data:/var/lib/mysql

    volumes:
    db_data:

Note : Ca va permettre d'éxécuter mon docker avec sa BDD.
