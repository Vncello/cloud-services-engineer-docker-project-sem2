FROM node:18-alpine AS builder

ARG NODE_ENV=dev
ARG NODE_OPTIONS="--openssl-legacy-provider"

ENV NODE_ENV=$NODE_ENV
ENV NODE_OPTIONS=$NODE_OPTIONS

WORKDIR /app

COPY ./package.json ./package-lock.json .

RUN npm install

COPY . .

RUN npm run build



FROM nginx:stable-alpine

WORKDIR /usr/share/nginx/html

RUN rm -rf ./* && addgroup -S "nginx_group" && adduser -SDH -g "nginx_group" -s /sbin/nologin nginx_user

COPY --chown=nginx_user:nginx_group ./nginx.conf /etc/nginx/conf.d/default.conf

COPY --chown=nginx_user:nginx_group --from=builder /app/dist/ .

RUN touch /var/run/nginx.pid && chown -R nginx_user:nginx_group /var/log/nginx /var/cache/nginx /var/run/nginx.pid

USER nginx_user

EXPOSE 80

ENTRYPOINT ["nginx", "-g", "daemon off;"]

HEALTHCHECK --interval=20s --start-period=15s --timeout=10s --retries=5 CMD curl -f http://localhost:80/catalog