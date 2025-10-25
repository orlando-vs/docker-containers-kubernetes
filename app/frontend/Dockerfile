# Build Angular
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Serve con Nginx
FROM nginx:alpine
COPY --from=build /app/dist/ /usr/share/nginx/html/
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80



# Building Angular
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Serve  Nginx
FROM nginx:alpine

COPY --from=build /app/dist/ /tmp/dist/

RUN set -eux; \
    rm -f /etc/nginx/conf.d/default.conf; \
    if find /tmp/dist -maxdepth 2 -type d -name browser | grep -q browser; then \
      echo "Copiando Angular dist/*/browser -> /usr/share/nginx/html"; \
      cp -r /tmp/dist/*/browser/* /usr/share/nginx/html/; \
    else \
      echo "Copiando Angular dist directo -> /usr/share/nginx/html"; \
      cp -r /tmp/dist/* /usr/share/nginx/html/; \
    fi
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
