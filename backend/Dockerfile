# backend/Dockerfile
FROM node:18-alpine
WORKDIR /usr/src/app

# copy package files and install production dependencies
COPY package*.json ./
# prefer npm ci for reproducible installs if package-lock exists:
RUN if [ -f package-lock.json ]; then npm ci --only=production; else npm install --production; fi

# copy source
COPY . .

# expose backend port (should match your app)
ENV PORT=8080
EXPOSE 8080

# start command (assuming server.js exists)
CMD ["node", "server.js"]
