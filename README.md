████████╗██████╗      ██╗      
╚══██╔══╝██╔══██╗     ██║   
   ██║   ██████╔╝     ██║    
   ██║   ██╔═══╝      ██║    
   ██║   ██║          ██║   
   ╚═╝   ╚═╝          ╚═╝


-------------------------------------------------------------------------------------------------------------

Ci-dessous, toutes les commandes réalisées par ordre chronologique ainsi que leur retour dans le termnal,
le tout accompagné d'une note personnel sur l'utilité de chaque commandes :

-------------------------------------------------------------------------------------------------------------

1.sudo su

=> root

Note : Docker demande systématique de sudo pour executer ses commandes, je passe donc en root pour mener à bien chaque commande et ne pas perdre de temps.



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



4.