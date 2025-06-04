████████╗██████╗      ██╗      
╚══██╔══╝██╔══██╗     ██║   
   ██║   ██████╔╝     ██║    
   ██║   ██╔═══╝      ██║    
   ██║   ██║          ██║   
   ╚═╝   ╚═╝          ╚═╝
LAFON Rafael - B3 Cyber

-------------------------------------------------------------------------------------------------------------

Ci-dessous, toutes les commandes réalisées par ordre chronologique ainsi que leur retour dans le termnal,
le tout accompagné d'une note personnel sur l'utilité de chaque commandes :

-------------------------------------------------------------------------------------------------------------

1.sudo adduser $(whoami) docker

Note : Docker demande systématique de sudo pour executer ses commandes, j'ajoute donc mon user dans docker pour mener à bien chaque commande et ne pas perdre de temps.



2.docker pull httpd :

=>  Using default tag: latest
    latest: Pulling from library/httpd
    Digest: sha256:09cb4b94edaaa796522c545328b62e9a0db60315c7be9f2b4e02204919926405
    Status: Image is up to date for httpd:latest
    docker.io/library/httpd:latest

Note : Image déjà téléchargé grace à une précédente tnetative de réalisation de ce TP. Ca à télécharger une image de Apache HTTPD avec le tag "Latest" pour obtenir la derniere version de l'image.



3.docker images

=>      REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
        nginx        latest    be69f2940aaf   6 weeks ago    192MB
        httpd        latest    958373fdd7e8   4 months ago   148MB

Note : l'image est bien stocker localement , en compagnie de nginx suite à un test précédent ce TP (= Basics Commands)



4.docker run -p 8080:80 -v ~/Desktop/TP-DOCKER-1:/usr/local/apache2/htdocs/ --name httpd-container httpd :

=>      AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.3. Set the 'ServerName' directive globally to       suppress this message
        AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.3. Set the 'ServerName' directive globally to suppress this message
        [Wed Jun 04 08:06:07.480452 2025] [mpm_event:notice] [pid 1:tid 1] AH00489: Apache/2.4.63 (Unix) configured -- resuming normal operations
        [Wed Jun 04 08:06:07.480536 2025] [core:notice] [pid 1:tid 1] AH00094: Command line: 'httpd -D FOREGROUND'
        172.17.0.1 - - [04/Jun/2025:08:06:19 +0000] "GET / HTTP/1.1" 200 247

Note : Lancement du conteneur à l'adresse 172.17.0.3, le Docker liant son port 80 au port 8080 de ma machine, avec le PATH du fichier HTML souahiter vers le dossier du conteneur pour affihcer la page et le nom et la nature conteneur.



5.docker stop httpd-container && docker rm httpd-container :

=>      Arret et suppression du conteneur en vu de l'étape suivatne.



6.docker run -d --name httpd-container -p 8080:80 httpd :

=>      AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.3. Set the 'ServerName' directive globally to suppress this message
        AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.3. Set the 'ServerName' directive globally to suppress this message
        [Wed Jun 04 08:21:34.335898 2025] [mpm_event:notice] [pid 1:tid 1] AH00489: Apache/2.4.63 (Unix) configured -- resuming normal operations
        [Wed Jun 04 08:21:34.335991 2025] [core:notice] [pid 1:tid 1] AH00094: Command line: 'httpd -D FOREGROUND'

Note : Recréation d'un conteneur et mise en route sans volume.



7.docker cp ~/Desktop/TP-DOCKER-1/index.html httpd-container:/usr/local/apache2/htdocs/index.html :

=>      Successfully copied 2.05kB to httpd-container:/usr/local/apache2/htdocs/index.html

Note : Copie manuellement l'index.html dans le conteneur.



8.nano Dockerfile :

=>      Création du Dockerfile

Note : Je le remplis du contenu suivant = 
            FROM httpd:latest
            COPY index.html /usr/local/apache2/htdocs/
Là j'utilise l'image d'Apache HTTPD comme base.



9.docker build -t image-httpd-container . :

        [+] Building 0.1s (7/7) FINISHED                                                                                                                                                 docker:default
        => [internal] load build definition from Dockerfile                                                                                                                                       0.0s
        => => transferring dockerfile: 98B                                                                                                                                                        0.0s
        => [internal] load metadata for docker.io/library/httpd:latest                                                                                                                            0.0s
        => [internal] load .dockerignore                                                                                                                                                          0.0s
        => => transferring context: 2B                                                                                                                                                            0.0s
        => [internal] load build context                                                                                                                                                          0.0s
        => => transferring context: 286B                                                                                                                                                          0.0s
        => [1/2] FROM docker.io/library/httpd:latest                                                                                                                                              0.0s
        => [2/2] COPY index.html /usr/local/apache2/htdocs/                                                                                                                                       0.0s
        => exporting to image                                                                                                                                                                     0.0s
        => => exporting layers                                                                                                                                                                    0.0s
        => => writing image sha256:a6804a90214bde32e0b9f7235f5feb0b08287cb676d5a9dff822e78f397428d2                                                                                               0.0s
        => => naming to docker.io/library/image-httpd-container        

Note : Je build l'image dans le dossier actuel en se basant ducoup sur mon fichier Dockerfile



10.docker run -p 8080:80 --name httpd-container image-httpd-container :

=>      AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.3. Set the 'ServerName' directive globally to suppress this message
        AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.3. Set the 'ServerName' directive globally to suppress this message
        [Wed Jun 04 09:08:46.338407 2025] [mpm_event:notice] [pid 1:tid 1] AH00489: Apache/2.4.63 (Unix) configured -- resuming normal operations
        [Wed Jun 04 09:08:46.338500 2025] [core:notice] [pid 1:tid 1] AH00094: Command line: 'httpd -D FOREGROUND'
        172.17.0.1 - - [04/Jun/2025:09:08:50 +0000] "GET / HTTP/1.1" 200 247

Note : J'utilise l'image que j'ai crée précédament pour afficher ma page html sur le localhost.



11. QUESTION : Quelles différences observez-vous entre les procédures 5. et 6. ? Avantages et inconvénients de l’une et de l’autre méthode ? (Mettre en relation ce qui est observé avec ce qui a été présenté pendant le cours)

Réponse : Les grandes différence sont surtout sur ce qu'on a prévu d'en faire, la premiere procédure est plus local, adapté à la modification, ou encore un dev rapide. Pour le second on est plus sur de la production , tout est encapsuler dans un image, on fait de la gestion de version ce qui nous donnes tout ça un déploiement plus que fiable, moins voué à l'éxpérimentation.


12. docker pull mysql:5.7 && docker pull phpmyadmin :

=>      5.7: Pulling from library/mysql
        20e4dcae4c69: Pulling fs layer 
        1c56c3d4ce74: Pulling fs layer 
        e9f03a1c24ce: Pull complete 
        68c3898c2015: Pull complete 
        6b95a940e7b6: Pull complete 
        90986bb8de6e: Pull complete 
        ae71319cb779: Pull complete 
        ffc89e9dfd88: Pull complete 
        43d05e938198: Pull complete 
        064b2d298fba: Pull complete 
        df9a4d85569b: Pull complete 
        Digest: sha256:4bc6bc963e6d8443453676cae56536f4b8156d78bae03c0145cbe47c2aad73bb
        Status: Downloaded newer image for mysql:5.7
        docker.io/library/mysql:5.7

        Using default tag: latest
        latest: Pulling from library/phpmyadmin
        61320b01ae5e: Already exists 
        b4ce612dc732: Pulling fs layer 
        093338982d92: Pulling fs layer 
        0c2a0ba9eb0c: Pulling fs layer 
        442abaed7751: Pulling fs layer 
        aa4e51934eef: Pulling fs layer 
        924949db942b: Pulling fs layer 
        442abaed7751: Waiting 
        aa4e51934eef: Waiting 
        64a857ca8a6a: Waiting 
        924949db942b: Waiting 
        d4dfcfe68eba: Pull complete 
        91e76e37da73: Pull complete 
        4f4fb700ef54: Pull complete 
        708105cf3285: Pull complete 
        97739253e39f: Pull complete 
        6722b89322cb: Pull complete 
        9ebc473a2663: Pull complete 
        39ff72c5f974: Pull complete 
        2daa1390d242: Pull complete 
        Digest: sha256:73467128842bc4406372310f068bc9ccb6727a82c7b5dc9c4f3d815ead33eab8
        Status: Downloaded newer image for phpmyadmin:latest
        docker.io/library/phpmyadmin:latest


Note : Téléchargement des images de MySQL et PHPMyAdmin .