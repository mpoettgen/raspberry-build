# We use the node image to compile our angular app into static content
FROM node:14.16.0-buster-slim AS node
# This node environment is used to build the application, not to run it
ENV NODE_ENV=development
# Make sure we've got a proper version of npm
RUN npm i -g npm@7.6.2
# Set the working directory where we build our project
WORKDIR /src
# Packages change less frequently, so we restore them first
COPY rasbuild/package.json rasbuild/package-lock.json ./
# CI is faster than install and we want to install all packages
RUN npm ci
# Now add the rest of the application files ...
COPY rasbuild/ ./
# ... and build the actual application in prod mode.
RUN npm run build --prod

# The nginx image is the base for our target image.
FROM nginx:1.19.7 AS final
# Copy the static files to the nginx web root
COPY --from=node /src/dist/rasbuild /usr/share/nginx/html
