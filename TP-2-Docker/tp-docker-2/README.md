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

1.sudo adduser $(whoami) docker

Note : Permet d’ajouter mon utilisateur au groupe Docker pour éviter d’utiliser `sudo` à chaque commande Docker. Pratique pour ne pas perdre de temps pendant le TP. Une reconnexion est nécessaire.

---

2.git init

Note : Initialise le dépôt Git local pour suivre les modifications et versionner proprement tout le travail fait durant le TP.

---

3.git add . && git commit -m "Initial commit : TP Docker Express"

Note : Premier commit avec les fichiers initiaux décompressés depuis le zip fourni sur Moodle (`TP-2-Docker.zip`).

---

4.npm install --only=production

Note : Cette option installe uniquement les dépendances nécessaires à l’exécution de l’app (pas celles pour le développement), ce qui permet d’alléger l’image Docker et de respecter les bonnes pratiques

---

5.docker build -t ma_super_app ./src

Retour :
```bash
[+] Building 7.4s (12/12) FINISHED
 => => naming to docker.io/library/ma_super_app
