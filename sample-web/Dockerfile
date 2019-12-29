# Dockerfile
### STAGE 1: Build ###

# We label our stage as 'builder'
FROM node:13.5.0-alpine as builder

COPY package*.json ./

RUN npm set progress=false && npm config set depth 0 && npm cache clean --force

## Storing node modules on a separate layer will prevent unnecessary npm installs at each build
RUN npm i && mkdir /ng-app && cp -R ./node_modules ./ng-app

WORKDIR /ng-app

COPY . .

## Build the angular app in production mode and store the artifacts in dist folder
RUN npm run build:ssr


### STAGE 2: Setup ###

FROM node:13.5.0-alpine

COPY --from=builder /ng-app/dist /dist

## Install http-server
RUN npm install http-server -g

CMD ["node", "/dist/server"]