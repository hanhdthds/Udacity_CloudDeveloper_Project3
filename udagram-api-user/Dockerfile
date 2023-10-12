FROM node:12
## Create app directory
WORKDIR /usr/src/app
## Install app dependencies
## A wildcard is used to ensure both package.json AND package-lock.json are copied where available (npm@5+)
COPY package*.json ./
RUN npm ci 
ENV GENERATE_SOURCEMAP=false
ENV NODE_OPTIONS=--max-old-space-size=16384
## Bundle app source
COPY . .
EXPOSE 8080
RUN export NODE_OPTIONS=--openssl-legacy-provider
CMD [ "npm", "run", "prod" ]