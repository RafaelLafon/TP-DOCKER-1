████████╗██████╗      ██╗  ██╗  ██╗  
╚══██╔══╝██╔══██╗     ██║  ██║  ██║
   ██║   ██████╔╝     ██║  ██║  ██║
   ██║   ██╔═══╝      ██║  ██║  ██║
   ██║   ██║          ██║  ██║  ██║
   ╚═╝   ╚═╝          ╚═╝  ╚═╝  ╚═╝
LAFON Rafael - B3 Cyber

-------------------------------------------------------------------------------------------------------------

Ci-dessous, toutes les commandes réalisées par ordre chronologique ainsi que leur retour dans le terminal,
le tout accompagné d'une note personnel sur l'utilité de chaque commande :

-------------------------------------------------------------------------------------------------------------

1.Nouveau repository

=> Je vais crée un dossier TP3 dans le répository déjà existant



2.npx create-react-app .

=> Need to install the following packages:
   create-react-app@5.1.0
   Ok to proceed? (y) y


   Creating a new React app in /home/doves/Desktop/TP-DOCKER-1/tp3.

   Installing packages. This might take a couple of minutes.
   Installing react, react-dom, and react-scripts with cra-template...


   added 1324 packages in 23s

   269 packages are looking for funding
   run `npm fund` for details

   Installing template dependencies using npm...

   added 18 packages, and changed 1 package in 4s

   269 packages are looking for funding
   run `npm fund` for details
   Removing template package using npm...


   removed 1 package, and audited 1342 packages in 7s

   269 packages are looking for funding
   run `npm fund` for details

   9 vulnerabilities (3 moderate, 6 high)

   To address all issues (including breaking changes), run:
   npm audit fix --force

   Run `npm audit` for details.

   Success! Created tp3 at /home/doves/Desktop/TP-DOCKER-1/tp3
   Inside that directory, you can run several commands:

   npm start
      Starts the development server.

   npm run build
      Bundles the app into static files for production.

   npm test
      Starts the test runner.

   npm run eject
      Removes this tool and copies build dependencies, configuration files
      and scripts into the app directory. If you do this, you can’t go back!

   We suggest that you begin by typing:

   cd /home/doves/Desktop/TP-DOCKER-1/tp3
   npm start

   You had a `README.md` file, we renamed it to `README.old.md`

   Happy hacking!

Note: Création du projet React



3.npm start

=> Compiled successfully!

   You can now view tp3 in the browser.

   Local:            http://localhost:3000
   On Your Network:  http://10.32.4.230:3000

   Note that the development build is not optimized.
   To create a production build, use npm run build.

   webpack compiled successfully


Note: Lancement du serveur, et le serveur fonctione sur le local host au port 3000



4.Création d'un Dockerefile

=> FROM node:18-alpine as build

   WORKDIR /app
   COPY package*.json ./
   RUN npm install
   COPY . .
   RUN npm run build

Note: Ce docker file me permet de build l'app react, copier les fichiers json pour optimiser le cache Docker, j'initalise les dépendances, je copie le reste des fichier sources, puis je lance le build.



5.Serveur Web pour l'app

=> FROM nginx:alpine

   COPY --from=build /app/build /usr/share/nginx/html
   EXPOSE 80
   CMD ["nginx", "-g", "daemon off;"]
   
Note: On rajoute ceci dans le dockerfile, ce qu nous permet de lancer un serveur nginx, de copier les fichiers buildés de l'étape précédente vers le dossier de nginx, et d'exposer le serveur sur le port 80. Puis lance Nginx en mode foreground, pour que le processus principal du conteneur reste actif et ne se termine pas desuite.

6.docker build -t mareactapp .

=> [+] Building 27.3s (14/14) FINISHED                                                                                                                                              docker:default
 => [internal] load build definition from Dockerfile                                                                                                                                       0.0s
 => => transferring dockerfile: 266B                                                                                                                                                       0.0s
 => WARN: FromAsCasing: 'as' and 'FROM' keywords' casing do not match (line 1)                                                                                                             0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                                                                                                            1.5s
 => [internal] load metadata for docker.io/library/node:18-alpine                                                                                                                          1.5s
 => [internal] load .dockerignore                                                                                                                                                          0.0s
 => => transferring context: 2B                                                                                                                                                            0.0s
 => [build 1/6] FROM docker.io/library/node:18-alpine@sha256:8d6421d663b4c28fd3ebc498332f249011d118945588d0a35cb9bc4b8ca09d9e                                                              2.6s
 => => resolve docker.io/library/node:18-alpine@sha256:8d6421d663b4c28fd3ebc498332f249011d118945588d0a35cb9bc4b8ca09d9e                                                                    0.0s
 => => sha256:ee77c6cd7c1886ecc802ad6cedef3a8ec1ea27d1fb96162bf03dd3710839b8da 6.18kB / 6.18kB                                                                                             0.0s
 => => sha256:f18232174bc91741fdf3da96d85011092101a032a93a388b79e99e69c2d5c870 3.64MB / 3.64MB                                                                                             0.4s
 => => sha256:dd71dde834b5c203d162902e6b8994cb2309ae049a0eabc4efea161b2b5a3d0e 40.01MB / 40.01MB                                                                                           1.5s
 => => sha256:1e5a4c89cee5c0826c540ab06d4b6b491c96eda01837f430bd47f0d26702d6e3 1.26MB / 1.26MB                                                                                             0.2s
 => => sha256:8d6421d663b4c28fd3ebc498332f249011d118945588d0a35cb9bc4b8ca09d9e 7.67kB / 7.67kB                                                                                             0.0s
 => => sha256:929b04d7c782f04f615cf785488fed452b6569f87c73ff666ad553a7554f0006 1.72kB / 1.72kB                                                                                             0.0s
 => => sha256:25ff2da83641908f65c3a74d80409d6b1b62ccfaab220b9ea70b80df5a2e0549 446B / 446B                                                                                                 0.5s
 => => extracting sha256:f18232174bc91741fdf3da96d85011092101a032a93a388b79e99e69c2d5c870                                                                                                  0.1s
 => => extracting sha256:dd71dde834b5c203d162902e6b8994cb2309ae049a0eabc4efea161b2b5a3d0e                                                                                                  0.9s
 => => extracting sha256:1e5a4c89cee5c0826c540ab06d4b6b491c96eda01837f430bd47f0d26702d6e3                                                                                                  0.0s
 => => extracting sha256:25ff2da83641908f65c3a74d80409d6b1b62ccfaab220b9ea70b80df5a2e0549                                                                                                  0.0s
 => [stage-1 1/2] FROM docker.io/library/nginx:alpine@sha256:65645c7bb6a0661892a8b03b89d0743208a18dd2f3f17a54ef4b76fb8e2f2a10                                                              2.3s
 => => resolve docker.io/library/nginx:alpine@sha256:65645c7bb6a0661892a8b03b89d0743208a18dd2f3f17a54ef4b76fb8e2f2a10                                                                      0.0s
 => => sha256:62223d644fa234c3a1cc785ee14242ec47a77364226f1c811d2f669f96dc2ac8 2.50kB / 2.50kB                                                                                             0.0s
 => => sha256:6769dc3a703c719c1d2756bda113659be28ae16cf0da58dd5fd823d6b9a050ea 10.79kB / 10.79kB                                                                                           0.0s
 => => sha256:f18232174bc91741fdf3da96d85011092101a032a93a388b79e99e69c2d5c870 3.64MB / 3.64MB                                                                                             0.4s
 => => sha256:65645c7bb6a0661892a8b03b89d0743208a18dd2f3f17a54ef4b76fb8e2f2a10 10.33kB / 10.33kB                                                                                           0.0s
 => => sha256:61ca4f733c802afd9e05a32f0de0361b6d713b8b53292dc15fb093229f648674 1.79MB / 1.79MB                                                                                             1.0s
 => => sha256:b464cfdf2a6319875aeb27359ec549790ce14d8214fcb16ef915e4530e5ed235 629B / 629B                                                                                                 1.0s
 => => extracting sha256:61ca4f733c802afd9e05a32f0de0361b6d713b8b53292dc15fb093229f648674                                                                                                  0.0s
 => => sha256:81bd8ed7ec6789b0cb7f1b47ee731c522f6dba83201ec73cd6bca1350f582948 402B / 402B                                                                                                 1.3s
 => => sha256:d7e5070240863957ebb0b5a44a5729963c3462666baa2947d00628cb5f2d5773 955B / 955B                                                                                                 1.1s
 => => extracting sha256:b464cfdf2a6319875aeb27359ec549790ce14d8214fcb16ef915e4530e5ed235                                                                                                  0.0s
 => => extracting sha256:d7e5070240863957ebb0b5a44a5729963c3462666baa2947d00628cb5f2d5773                                                                                                  0.0s
 => => sha256:197eb75867ef4fcecd4724f17b0972ab0489436860a594a9445f8eaff8155053 1.21kB / 1.21kB                                                                                             1.4s
 => => extracting sha256:81bd8ed7ec6789b0cb7f1b47ee731c522f6dba83201ec73cd6bca1350f582948                                                                                                  0.0s
 => => sha256:34a64644b756511a2e217f0508e11d1a572085d66cd6dc9a555a082ad49a3102 1.40kB / 1.40kB                                                                                             1.5s
 => => extracting sha256:197eb75867ef4fcecd4724f17b0972ab0489436860a594a9445f8eaff8155053                                                                                                  0.0s
 => => sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d 15.52MB / 15.52MB                                                                                           1.9s
 => => extracting sha256:34a64644b756511a2e217f0508e11d1a572085d66cd6dc9a555a082ad49a3102                                                                                                  0.0s
 => => extracting sha256:39c2ddfd6010082a4a646e7ca44e95aca9bf3eaebc00f17f7ccc2954004f2a7d                                                                                                  0.3s
 => [internal] load build context                                                                                                                                                          3.2s
 => => transferring context: 277.60MB                                                                                                                                                      3.1s
 => [build 2/6] WORKDIR /app                                                                                                                                                               0.4s
 => [build 3/6] COPY package*.json ./                                                                                                                                                      0.2s
 => [build 4/6] RUN npm install                                                                                                                                                            9.5s
 => [build 5/6] COPY . .                                                                                                                                                                   5.5s
 => [build 6/6] RUN npm run build                                                                                                                                                          6.7s
 => [stage-1 2/2] COPY --from=build /app/build /usr/share/nginx/html                                                                                                                       0.0s
 => exporting to image                                                                                                                                                                     0.0s
 => => exporting layers                                                                                                                                                                    0.0s
 => => writing image sha256:e2ff6ac6f0dc6753ec5f4ffbdb6b7f114c91e8bbf9ac30b1f3b870a8b4ffc2e5                                                                                               0.0s
 => => naming to docker.io/library/mareactapp                                                                                                                                              0.0s

 1 warning found (use docker --debug to expand):
 - FromAsCasing: 'as' and 'FROM' keywords' casing do not match (line 1)

Note: Build de l'image docker