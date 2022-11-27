# WIK-DPS-TP02

---

### Créer une image Docker avec un seul stage qui permet d’exécuter votre API développée précédemment (WIK-DPS-TP01)

```
FROM node as builder

WORKDIR /home/node/app

COPY package*.json ./

RUN npm install

COPY . .

RUN npx tsc

CMD node build/index.js

USER node
```

---

### L'image doit être la plus optimisée possible concernant l'ordre des layers afin de limiter le temps de build lors des modifications sur le code

- Première image `dockerfile` :

![](https://i.imgur.com/oHU2GgU.png)

- Deuxieme image `dockerfile2` :

![Uploading file..._l1ms9fo15]()

---

### Scanner votre image avec docker scan, trivy ou clair pour obtenir la liste des vulnérabilités détectées

- Pour scanner l'image : `trivy image api_typescript_docker`

![](https://i.imgur.com/NCEffwg.png)

---

### L'image doit utiliser un utilisateur spécifique pour l'exécution de votre serveur web

- Dans le fichier dockerfile, on peut trouver une ligne ou je renseigne un USER nommé `node` :

- Effectué cette commande pour voir le user utilisé sur l'image :
  `docker inspect $(docker ps -q) --format '{{.Config.User}} {{.Name}}'`

![](https://i.imgur.com/KM8wSK0.png)

---

#### Créer une seconde image Docker pour votre API avec les mêmes contraintes en termes d'optimisations mais avec plusieurs stages : un pour l'étape de build et une autre pour l’exécution (qui ne contient pas les sources)

```
##STAGE 1 FOR BUILD

FROM node as builder
WORKDIR /usr/src/app
COPY ["package.json", "package-lock.json", "./"]
RUN ["npm", "install"]
COPY . .
RUN ["npx", "tsc"]

## STAGE 2 FOR EXECUTION

FROM builder as executer
WORKDIR /usr/src/app
ENTRYPOINT ["node", "./build/index"]
USER node
```

---

### Pour lancer le projet :

Cloner le dossier dans un repertoire de votre choix : https://github.com/gaetanroux/WIK-DPS-TP02.git

Lancer Vscode.
Tapez ces commandes :

- `docker build -t api_typescript_docker .`
- `docker run -d -p 8080:8080 --name api_headers_ping api_typescript_docker`

---
