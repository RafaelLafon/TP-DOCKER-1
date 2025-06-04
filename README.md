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