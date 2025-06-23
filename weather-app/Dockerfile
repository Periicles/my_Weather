# Étape 1 : builder l'app VueJS
FROM node:22-alpine AS build

WORKDIR /app

# Copie des fichiers de dép Donc le navigateur tente de négocier du SSL... et Nginx lui balance du HTML clair sans cryptage → erreur.endances
COPY package*.json ./

# Installation des dépendances
RUN npm install

# Copie du reste du code
COPY . .

# Build Vue
RUN npm run build

# Étape 2 : serve statique avec Nginx
FROM nginx:alpine

# Copie du build dans nginx
COPY --from=build /app/dist /usr/share/nginx/html

# Exposition du port
EXPOSE 80

# Démarrage nginx (déjà par défaut mais on explicite)
CMD ["nginx", "-g", "daemon off;"]
