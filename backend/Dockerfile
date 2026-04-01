# Multi-stage build:
# - Stage 1 (builder/test) installe toutes les dependances et execute les tests.
# - Stage 2 (runtime) ne conserve que le necessaire pour l'execution en production.
# Ce decoupage reduit la surface d'attaque et la taille de l'image finale.

FROM node:20-alpine AS builder
WORKDIR /app

# Installation des dependances (inclut les devDependencies pour les tests)
COPY package*.json ./
RUN npm install

# Copie du code puis execution des tests unitaires
COPY . .
RUN npm test

# ---- Image finale de production ----
FROM node:20-alpine AS runtime
WORKDIR /app
ENV NODE_ENV=production

# Installer uniquement les dependances de production
COPY package*.json ./
RUN npm install --omit=dev && npm cache clean --force

# Copier uniquement le code necessaire au runtime
COPY server.js ./server.js

EXPOSE 3000
CMD ["npm", "start"]