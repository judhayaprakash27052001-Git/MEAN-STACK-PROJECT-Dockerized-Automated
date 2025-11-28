In this DevOps task, you need to build and deploy a full-stack CRUD application using the MEAN stack (MongoDB, Express, Angular 15, and Node.js). The backend will be developed with Node.js and Express to provide REST APIs, connecting to a MongoDB database. The frontend will be an Angular application utilizing HTTPClient for communication.  

The application will manage a collection of tutorials, where each tutorial includes an ID, title, description, and published status. Users will be able to create, retrieve, update, and delete tutorials. Additionally, a search box will allow users to find tutorials by title.

## Project setup

### Node.js Server

cd backend

npm install

You can update the MongoDB credentials by modifying the `db.config.js` file located in `app/config/`.

Run `node server.js`

### Angular Client

cd frontend

npm install

Run `ng serve --port 8081`

You can modify the `src/app/services/tutorial.service.ts` file to adjust how the frontend interacts with the backend.

Navigate to `http://localhost:8081/`


## Added automation & Docker support (prepared by assistant)

Added files:
- backend/Dockerfile
- frontend/Dockerfile
- frontend/frontend.conf
- docker-compose.yml (root)
- .github/workflows/ci-cd.yml (GitHub Actions CI/CD workflow)

### Quick local run (development)
1. Build and run via Docker Compose:
   ```bash
   docker compose build
   docker compose up -d
   ```
2. Open http://localhost

### CI/CD (GitHub Actions)
1. Create repository on GitHub and push this project.
2. Add repository secrets:
   - DOCKERHUB_USERNAME
   - DOCKERHUB_TOKEN
   - SSH_HOST (public IP of your Ubuntu VM)
   - SSH_USER (e.g., ubuntu)
   - SSH_PRIVATE_KEY (private key content for SSH)
3. On push to main, workflow builds two images and pushes to Docker Hub, then SSHs to VM, pulls images and restarts the compose stack.

### VM deployment notes
- Clone the repo on the VM into `~/app` or let the workflow fetch docker-compose.yml.
- Ensure Docker & Docker Compose (v2) are installed.
- Ensure the VM user has permissions to run docker (add to docker group or use sudo).
