FROM node:18-alpine AS builder

ARG NODE_ENV=production
ARG NODE_OPTIONS="--openssl-legacy-provider"

ENV NODE_ENV=$NODE_ENV
ENV NODE_OPTIONS=$NODE_OPTIONS

WORKDIR /app

COPY ./package.json ./package-lock.json .

RUN npm install

COPY . .

RUN npm run build

