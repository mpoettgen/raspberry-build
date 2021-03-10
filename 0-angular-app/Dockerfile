FROM nginx:1.19.7 AS base

FROM node:14.16.0-buster-slim AS node
ENV NODE_ENV=development
RUN npm i -g npm@7.6.1
ENV NPM_CONFIG_PREFIX=/home/node/.npm-global \
    PATH=$PATH:/home/node/.npm-global/bin
RUN npm i -g @angular/cli@11.2.3
WORKDIR /src
COPY rasbuild/package.json rasbuild/package-lock.json ./
RUN npm ci
COPY rasbuild/ ./
RUN ng build --prod

FROM base AS final
COPY --from=node /src/dist/rasbuild /usr/share/nginx/html
