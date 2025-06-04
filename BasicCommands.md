1.docker run hello-world :

=>      Hello from Docker!
        This message shows that your installation appears to be working correctly.

        To generate this message, Docker took the following steps:
        1. The Docker client contacted the Docker daemon.
        2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
            (amd64)
        3. The Docker daemon created a new container from that image which runs the
            executable that produces the output you are currently reading.
        4. The Docker daemon streamed that output to the Docker client, which sent it
            to your terminal.

        To try something more ambitious, you can run an Ubuntu container with:
        $ docker run -it ubuntu bash

        Share images, automate workflows, and more with a free Docker ID:
        https://hub.docker.com/

        For more examples and ideas, visit:
        https://docs.docker.com/get-started/



2.docker run -it ubuntu bash :

=>      root@LAPTOP-B4123T32M0RT2:/home/doves/Desktop/TP-DOCKER-1# docker run -it ubuntu bash
        root@dbbaff8085ba:/#
        root@dbbaff8085ba:/# exit
        exit


3.docker images :

=>      REPOSITORY        TAG       IMAGE ID       CREATED        SIZE



4.docker ps -a :

=>      CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES



5.docker run -p 80:80 nginx et docker run -p -d 80:80 nginx :

=>      Unable to find image 'nginx:latest' locally
        latest: Pulling from library/nginx
        61320b01ae5e: Pull complete 
        670a101d432b: Pull complete 
        405bd2df85b6: Pull complete 
        cc80efff8457: Pull complete 
        2b9310b2ee4b: Pull complete 
        6c4aa022e8e1: Pull complete 
        abddc69cb49d: Pull complete 
        Digest: sha256:fb39280b7b9eba5727c884a3c7810002e69e8f961cc373b89c92f14961d903a0
        Status: Downloaded newer image for nginx:latest




=>      7e398331467ac0e8815204dd41beee73f610301117c0c8f1b3abc2a712de03de