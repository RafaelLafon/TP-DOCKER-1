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



5.