FROM node:26-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
ENV NODE_OPTIONS=--openssl-legacy-provider
RUN npm run build

FROM nginxinc/nginx-unprivileged:1.29.8-alpine AS prod
USER root
RUN apk add --no-cache wget 
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
COPY --from=build --chown=appuser:appgroup /app/dist /usr/share/nginx/html/momo-store
COPY --chown=appuser:appgroup public/_redirects /usr/share/nginx/html/momo-store/_redirects
COPY --chown=appuser:appgroup nginx.conf /etc/nginx/conf.d/default.conf
RUN chmod -R 755 /usr/share/nginx/html/momo-store
USER nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]