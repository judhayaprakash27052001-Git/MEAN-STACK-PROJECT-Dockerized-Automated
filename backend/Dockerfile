FROM node:18-alpine

WORKDIR /usr/src/app

# copy package files and install
COPY package*.json ./
RUN npm install --production

# copy app source
COPY . .

EXPOSE 8080
CMD ["node", "server.js"]
