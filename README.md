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



13.docker network create my-net :

e54d568731a656a164f5d5f89390bbbb9f5631465c6cc11da60c3457c1d1131f

Note : On crée un réseau personnalisé pour la communication entre les conteneurs



14.docker run --name mysql-container --network my-net -e MYSQL_ROOT_PASSWORD=rootpassword -e MYSQL_DATABASE=testdb -e MYSQL_USER=testuser -e MYSQL_PASSWORD=testpass mysql:5.7

=>      2025-06-04 09:35:14+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.44-1.el7 started.
        2025-06-04 09:35:14+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
        2025-06-04 09:35:14+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.44-1.el7 started.
        2025-06-04 09:35:14+00:00 [Note] [Entrypoint]: Initializing database files
        2025-06-04T09:35:14.910708Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
        2025-06-04T09:35:15.102329Z 0 [Warning] InnoDB: New log files created, LSN=45790
        2025-06-04T09:35:15.129787Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
        2025-06-04T09:35:15.183683Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 3841923a-4127-11f0-91c9-562f505c71ff.
        2025-06-04T09:35:15.184839Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
        2025-06-04T09:35:15.275532Z 0 [Warning] A deprecated TLS version TLSv1 is enabled. Please use TLSv1.2 or higher.
        2025-06-04T09:35:15.275542Z 0 [Warning] A deprecated TLS version TLSv1.1 is enabled. Please use TLSv1.2 or higher.
        2025-06-04T09:35:15.275891Z 0 [Warning] CA certificate ca.pem is self signed.
        2025-06-04T09:35:15.320947Z 1 [Warning] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
        2025-06-04 09:35:17+00:00 [Note] [Entrypoint]: Database files initialized
        2025-06-04 09:35:17+00:00 [Note] [Entrypoint]: Starting temporary server
        2025-06-04 09:35:17+00:00 [Note] [Entrypoint]: Waiting for server startup
        2025-06-04T09:35:17.239203Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
        2025-06-04T09:35:17.240417Z 0 [Note] mysqld (mysqld 5.7.44) starting as process 125 ...
        2025-06-04T09:35:17.242718Z 0 [Note] InnoDB: PUNCH HOLE support available
        2025-06-04T09:35:17.242729Z 0 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
        2025-06-04T09:35:17.242732Z 0 [Note] InnoDB: Uses event mutexes
        2025-06-04T09:35:17.242737Z 0 [Note] InnoDB: GCC builtin __atomic_thread_fence() is used for memory barrier
        2025-06-04T09:35:17.242740Z 0 [Note] InnoDB: Compressed tables use zlib 1.2.13
        2025-06-04T09:35:17.242743Z 0 [Note] InnoDB: Using Linux native AIO
        2025-06-04T09:35:17.242948Z 0 [Note] InnoDB: Number of pools: 1
        2025-06-04T09:35:17.243034Z 0 [Note] InnoDB: Using CPU crc32 instructions
        2025-06-04T09:35:17.244443Z 0 [Note] InnoDB: Initializing buffer pool, total size = 128M, instances = 1, chunk size = 128M
        2025-06-04T09:35:17.249870Z 0 [Note] InnoDB: Completed initialization of buffer pool
        2025-06-04T09:35:17.250991Z 0 [Note] InnoDB: If the mysqld execution user is authorized, page cleaner thread priority can be changed. See the man page of setpriority().
        2025-06-04T09:35:17.262663Z 0 [Note] InnoDB: Highest supported file format is Barracuda.
        2025-06-04T09:35:17.272230Z 0 [Note] InnoDB: Creating shared tablespace for temporary tables
        2025-06-04T09:35:17.272296Z 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
        2025-06-04T09:35:17.291568Z 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
        2025-06-04T09:35:17.292104Z 0 [Note] InnoDB: 96 redo rollback segment(s) found. 96 redo rollback segment(s) are active.
        2025-06-04T09:35:17.292109Z 0 [Note] InnoDB: 32 non-redo rollback segment(s) are active.
        2025-06-04T09:35:17.292558Z 0 [Note] InnoDB: 5.7.44 started; log sequence number 2768291
        2025-06-04T09:35:17.292669Z 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool
        2025-06-04T09:35:17.292789Z 0 [Note] Plugin 'FEDERATED' is disabled.
        2025-06-04T09:35:17.293445Z 0 [Note] InnoDB: Buffer pool(s) load completed at 250604  9:35:17
        2025-06-04T09:35:17.296254Z 0 [Note] Found ca.pem, server-cert.pem and server-key.pem in data directory. Trying to enable SSL support using them.
        2025-06-04T09:35:17.296261Z 0 [Note] Skipping generation of SSL certificates as certificate files are present in data directory.
        2025-06-04T09:35:17.296263Z 0 [Warning] A deprecated TLS version TLSv1 is enabled. Please use TLSv1.2 or higher.
        2025-06-04T09:35:17.296265Z 0 [Warning] A deprecated TLS version TLSv1.1 is enabled. Please use TLSv1.2 or higher.
        2025-06-04T09:35:17.296636Z 0 [Warning] CA certificate ca.pem is self signed.
        2025-06-04T09:35:17.296663Z 0 [Note] Skipping generation of RSA key pair as key files are present in data directory.
        2025-06-04T09:35:17.297735Z 0 [Warning] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
        2025-06-04T09:35:17.303292Z 0 [Note] Event Scheduler: Loaded 0 events
        2025-06-04T09:35:17.303427Z 0 [Note] mysqld: ready for connections.
        Version: '5.7.44'  socket: '/var/run/mysqld/mysqld.sock'  port: 0  MySQL Community Server (GPL)
        2025-06-04 09:35:18+00:00 [Note] [Entrypoint]: Temporary server started.
        '/var/lib/mysql/mysql.sock' -> '/var/run/mysqld/mysqld.sock'
        2025-06-04T09:35:18.111014Z 3 [Note] InnoDB: Stopping purge
        2025-06-04T09:35:18.118634Z 3 [Note] InnoDB: Resuming purge
        2025-06-04T09:35:18.119553Z 3 [Note] InnoDB: Stopping purge
        2025-06-04T09:35:18.120957Z 3 [Note] InnoDB: Resuming purge
        2025-06-04T09:35:18.121466Z 3 [Note] InnoDB: Stopping purge
        2025-06-04T09:35:18.122644Z 3 [Note] InnoDB: Resuming purge
        2025-06-04T09:35:18.123470Z 3 [Note] InnoDB: Stopping purge
        2025-06-04T09:35:18.124952Z 3 [Note] InnoDB: Resuming purge
        Warning: Unable to load '/usr/share/zoneinfo/iso3166.tab' as time zone. Skipping it.
        Warning: Unable to load '/usr/share/zoneinfo/leapseconds' as time zone. Skipping it.
        Warning: Unable to load '/usr/share/zoneinfo/tzdata.zi' as time zone. Skipping it.
        Warning: Unable to load '/usr/share/zoneinfo/zone.tab' as time zone. Skipping it.
        Warning: Unable to load '/usr/share/zoneinfo/zone1970.tab' as time zone. Skipping it.
        2025-06-04 09:35:18+00:00 [Note] [Entrypoint]: Creating database testdb
        2025-06-04 09:35:18+00:00 [Note] [Entrypoint]: Creating user testuser
        2025-06-04 09:35:18+00:00 [Note] [Entrypoint]: Giving user testuser access to schema testdb

        2025-06-04 09:35:18+00:00 [Note] [Entrypoint]: Stopping temporary server
        2025-06-04T09:35:18.869542Z 0 [Note] Giving 0 client threads a chance to die gracefully
        2025-06-04T09:35:18.869560Z 0 [Note] Shutting down slave threads
        2025-06-04T09:35:18.869562Z 0 [Note] Forcefully disconnecting 0 remaining clients
        2025-06-04T09:35:18.869566Z 0 [Note] Event Scheduler: Purging the queue. 0 events
        2025-06-04T09:35:18.869596Z 0 [Note] Binlog end
        2025-06-04T09:35:18.870114Z 0 [Note] Shutting down plugin 'ngram'
        2025-06-04T09:35:18.870120Z 0 [Note] Shutting down plugin 'partition'
        2025-06-04T09:35:18.870123Z 0 [Note] Shutting down plugin 'BLACKHOLE'
        2025-06-04T09:35:18.870126Z 0 [Note] Shutting down plugin 'ARCHIVE'
        2025-06-04T09:35:18.870129Z 0 [Note] Shutting down plugin 'PERFORMANCE_SCHEMA'
        2025-06-04T09:35:18.870159Z 0 [Note] Shutting down plugin 'MRG_MYISAM'
        2025-06-04T09:35:18.870162Z 0 [Note] Shutting down plugin 'MyISAM'
        2025-06-04T09:35:18.870168Z 0 [Note] Shutting down plugin 'INNODB_SYS_VIRTUAL'
        2025-06-04T09:35:18.870171Z 0 [Note] Shutting down plugin 'INNODB_SYS_DATAFILES'
        2025-06-04T09:35:18.870173Z 0 [Note] Shutting down plugin 'INNODB_SYS_TABLESPACES'
        2025-06-04T09:35:18.870174Z 0 [Note] Shutting down plugin 'INNODB_SYS_FOREIGN_COLS'
        2025-06-04T09:35:18.870176Z 0 [Note] Shutting down plugin 'INNODB_SYS_FOREIGN'
        2025-06-04T09:35:18.870178Z 0 [Note] Shutting down plugin 'INNODB_SYS_FIELDS'
        2025-06-04T09:35:18.870179Z 0 [Note] Shutting down plugin 'INNODB_SYS_COLUMNS'
        2025-06-04T09:35:18.870181Z 0 [Note] Shutting down plugin 'INNODB_SYS_INDEXES'
        2025-06-04T09:35:18.870183Z 0 [Note] Shutting down plugin 'INNODB_SYS_TABLESTATS'
        2025-06-04T09:35:18.870184Z 0 [Note] Shutting down plugin 'INNODB_SYS_TABLES'
        2025-06-04T09:35:18.870186Z 0 [Note] Shutting down plugin 'INNODB_FT_INDEX_TABLE'
        2025-06-04T09:35:18.870188Z 0 [Note] Shutting down plugin 'INNODB_FT_INDEX_CACHE'
        2025-06-04T09:35:18.870189Z 0 [Note] Shutting down plugin 'INNODB_FT_CONFIG'
        2025-06-04T09:35:18.870191Z 0 [Note] Shutting down plugin 'INNODB_FT_BEING_DELETED'
        2025-06-04T09:35:18.870193Z 0 [Note] Shutting down plugin 'INNODB_FT_DELETED'
        2025-06-04T09:35:18.870194Z 0 [Note] Shutting down plugin 'INNODB_FT_DEFAULT_STOPWORD'
        2025-06-04T09:35:18.870197Z 0 [Note] Shutting down plugin 'INNODB_METRICS'
        2025-06-04T09:35:18.870198Z 0 [Note] Shutting down plugin 'INNODB_TEMP_TABLE_INFO'
        2025-06-04T09:35:18.870200Z 0 [Note] Shutting down plugin 'INNODB_BUFFER_POOL_STATS'
        2025-06-04T09:35:18.870202Z 0 [Note] Shutting down plugin 'INNODB_BUFFER_PAGE_LRU'
        2025-06-04T09:35:18.870205Z 0 [Note] Shutting down plugin 'INNODB_BUFFER_PAGE'
        2025-06-04T09:35:18.870207Z 0 [Note] Shutting down plugin 'INNODB_CMP_PER_INDEX_RESET'
        2025-06-04T09:35:18.870209Z 0 [Note] Shutting down plugin 'INNODB_CMP_PER_INDEX'
        2025-06-04T09:35:18.870210Z 0 [Note] Shutting down plugin 'INNODB_CMPMEM_RESET'
        2025-06-04T09:35:18.870212Z 0 [Note] Shutting down plugin 'INNODB_CMPMEM'
        2025-06-04T09:35:18.870214Z 0 [Note] Shutting down plugin 'INNODB_CMP_RESET'
        2025-06-04T09:35:18.870216Z 0 [Note] Shutting down plugin 'INNODB_CMP'
        2025-06-04T09:35:18.870218Z 0 [Note] Shutting down plugin 'INNODB_LOCK_WAITS'
        2025-06-04T09:35:18.870219Z 0 [Note] Shutting down plugin 'INNODB_LOCKS'
        2025-06-04T09:35:18.870221Z 0 [Note] Shutting down plugin 'INNODB_TRX'
        2025-06-04T09:35:18.870223Z 0 [Note] Shutting down plugin 'InnoDB'
        2025-06-04T09:35:18.870263Z 0 [Note] InnoDB: FTS optimize thread exiting.
        2025-06-04T09:35:18.870300Z 0 [Note] InnoDB: Starting shutdown...
        2025-06-04T09:35:18.970429Z 0 [Note] InnoDB: Dumping buffer pool(s) to /var/lib/mysql/ib_buffer_pool
        2025-06-04T09:35:18.970697Z 0 [Note] InnoDB: Buffer pool(s) dump completed at 250604  9:35:18
        2025-06-04T09:35:20.179652Z 0 [Note] InnoDB: Shutdown completed; log sequence number 12219253
        2025-06-04T09:35:20.183156Z 0 [Note] InnoDB: Removed temporary tablespace data file: "ibtmp1"
        2025-06-04T09:35:20.183167Z 0 [Note] Shutting down plugin 'MEMORY'
        2025-06-04T09:35:20.183170Z 0 [Note] Shutting down plugin 'CSV'
        2025-06-04T09:35:20.183173Z 0 [Note] Shutting down plugin 'sha256_password'
        2025-06-04T09:35:20.183175Z 0 [Note] Shutting down plugin 'mysql_native_password'
        2025-06-04T09:35:20.183293Z 0 [Note] Shutting down plugin 'binlog'
        2025-06-04T09:35:20.183903Z 0 [Note] mysqld: Shutdown complete

        2025-06-04 09:35:20+00:00 [Note] [Entrypoint]: Temporary server stopped

        2025-06-04 09:35:20+00:00 [Note] [Entrypoint]: MySQL init process done. Ready for start up.

        2025-06-04T09:35:21.037282Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
        2025-06-04T09:35:21.038485Z 0 [Note] mysqld (mysqld 5.7.44) starting as process 1 ...
        2025-06-04T09:35:21.040773Z 0 [Note] InnoDB: PUNCH HOLE support available
        2025-06-04T09:35:21.040785Z 0 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
        2025-06-04T09:35:21.040789Z 0 [Note] InnoDB: Uses event mutexes
        2025-06-04T09:35:21.040794Z 0 [Note] InnoDB: GCC builtin __atomic_thread_fence() is used for memory barrier
        2025-06-04T09:35:21.040798Z 0 [Note] InnoDB: Compressed tables use zlib 1.2.13
        2025-06-04T09:35:21.040800Z 0 [Note] InnoDB: Using Linux native AIO
        2025-06-04T09:35:21.041019Z 0 [Note] InnoDB: Number of pools: 1
        2025-06-04T09:35:21.041115Z 0 [Note] InnoDB: Using CPU crc32 instructions
        2025-06-04T09:35:21.042384Z 0 [Note] InnoDB: Initializing buffer pool, total size = 128M, instances = 1, chunk size = 128M
        2025-06-04T09:35:21.047458Z 0 [Note] InnoDB: Completed initialization of buffer pool
        2025-06-04T09:35:21.049070Z 0 [Note] InnoDB: If the mysqld execution user is authorized, page cleaner thread priority can be changed. See the man page of setpriority().
        2025-06-04T09:35:21.061882Z 0 [Note] InnoDB: Highest supported file format is Barracuda.
        2025-06-04T09:35:21.072247Z 0 [Note] InnoDB: Creating shared tablespace for temporary tables
        2025-06-04T09:35:21.072325Z 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
        2025-06-04T09:35:21.091841Z 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
        2025-06-04T09:35:21.092217Z 0 [Note] InnoDB: 96 redo rollback segment(s) found. 96 redo rollback segment(s) are active.
        2025-06-04T09:35:21.092221Z 0 [Note] InnoDB: 32 non-redo rollback segment(s) are active.
        2025-06-04T09:35:21.092691Z 0 [Note] InnoDB: 5.7.44 started; log sequence number 12219253
        2025-06-04T09:35:21.092925Z 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/ib_buffer_pool
        2025-06-04T09:35:21.093050Z 0 [Note] Plugin 'FEDERATED' is disabled.
        2025-06-04T09:35:21.094796Z 0 [Note] InnoDB: Buffer pool(s) load completed at 250604  9:35:21
        2025-06-04T09:35:21.097810Z 0 [Note] Found ca.pem, server-cert.pem and server-key.pem in data directory. Trying to enable SSL support using them.
        2025-06-04T09:35:21.097818Z 0 [Note] Skipping generation of SSL certificates as certificate files are present in data directory.
        2025-06-04T09:35:21.097820Z 0 [Warning] A deprecated TLS version TLSv1 is enabled. Please use TLSv1.2 or higher.
        2025-06-04T09:35:21.097822Z 0 [Warning] A deprecated TLS version TLSv1.1 is enabled. Please use TLSv1.2 or higher.
        2025-06-04T09:35:21.098221Z 0 [Warning] CA certificate ca.pem is self signed.
        2025-06-04T09:35:21.098251Z 0 [Note] Skipping generation of RSA key pair as key files are present in data directory.
        2025-06-04T09:35:21.098469Z 0 [Note] Server hostname (bind-address): '*'; port: 3306
        2025-06-04T09:35:21.098496Z 0 [Note] IPv6 is available.
        2025-06-04T09:35:21.098505Z 0 [Note]   - '::' resolves to '::';
        2025-06-04T09:35:21.098554Z 0 [Note] Server socket created on IP: '::'.
        2025-06-04T09:35:21.099349Z 0 [Warning] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
        2025-06-04T09:35:21.104760Z 0 [Note] Event Scheduler: Loaded 0 events
        2025-06-04T09:35:21.104896Z 0 [Note] mysqld: ready for connections.
        Version: '5.7.44'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server (GPL)

Note : Démarage du conteneur MySQL dans un premier terminal, le tout connecter au réseau personnalisé qu'on a crée juste avant.



15.docker run --name phpmyadmin-container --network my-net -e PMA_HOST=mysql-container -p 8081:80 phpmyadmin :

=>      AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.18.0.3. Set the 'ServerName' directive globally to suppress this message
        AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.18.0.3. Set the 'ServerName' directive globally to suppress this message
        [Wed Jun 04 09:41:38.264622 2025] [mpm_prefork:notice] [pid 1:tid 1] AH00163: Apache/2.4.62 (Debian) PHP/8.2.28 configured -- resuming normal operations
        [Wed Jun 04 09:41:38.264650 2025] [core:notice] [pid 1:tid 1] AH00094: Command line: 'apache2 -D FOREGROUND'

Note : Même chose pour PhpMyAdmin (user= testuser ; mdp= testpass).



16.Connexion et créations d'éléments dans la DB (nome de la table : Shtroumpfs):

 	# 	Name 	Type 	Collation 	Attributes 	Null 	Default 	Comments 	Extra 	Action
	1 	Name 	varchar(20) 	latin1_swedish_ci 		Yes 	NULL 			Change Change 	Drop Drop 	
	2 	Age 	int(11) 			Yes 	NULL 			Change Change 	Drop Drop 	
	3 	Size 	int(11) 			No 	None 			Change Change 	Drop Drop 	
	4 	Sexe 	varchar(6) 	latin1_swedish_ci 		No 	None 			Change Change 	Drop Drop 	


Note : Les noms ne sont qu'un teste, je ne fait aucun ressencement des Shtroumph en fonction de leur ethnie , cela étant interdit par la Loi n° 2007-297 du 5 mars 2007 .
