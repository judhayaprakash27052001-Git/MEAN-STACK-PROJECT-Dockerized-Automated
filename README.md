# MEAN Stack  Application (Dockerized & Automated)

This project is a full-stack CRUD (Create, Read, Update, Delete) application built using the **MEAN stack** (MongoDB, Express, Angular 15, Node.js). It manages a collection of tutorials with search functionality.

Originally designed for local development, this project has been enhanced with **Docker** and **GitHub Actions** for scalable cloud deployment, container orchestration, and continuous integration/continuous deployment (CI/CD).

## 🏗 Architecture & Technologies

  * **Database:** MongoDB (Stores tutorial data: title, description, published status).
  * **Backend:** Node.js & Express.js (REST API, Mongoose for ODM).
  * **Frontend:** Angular 15 (SPA User Interface).
  * **Web Server:** Nginx (Serves the Angular static build).
  * **Orchestration:** Docker Compose.
  * **Automation:** GitHub Actions.

-----

## 🚀 Docker & Automation Strategy

To make this project suitable for cloud deployment, 5 key files were added to the original source code. Here is how the containerization logic works:

### 1\. The Container Hierarchy (`docker-compose.yml`)

We use `docker-compose` to build and run multiple containers simultaneously. When the stack starts:

1.  **MongoDB Container:** Starts first on internal port `27017`.
2.  **Backend Container:** Builds using `backend/Dockerfile`. It waits for MongoDB to be ready (`depends_on`). It connects to the database via the internal Docker network and exposes the API on port **8080**.
3.  **Frontend Container:** Builds using `frontend/Dockerfile`. It compiles the Angular app into static files and serves them via an **Nginx** server, exposed on port **80**.

### 2\. File Explanations

  * **`backend/Dockerfile`**: Installs Node.js dependencies (`npm install`), copies the API source code, and sets up the execution command.
  * **`frontend/Dockerfile`**: A multi-stage build.
      * *Stage 1:* Installs dependencies and builds the Angular application.
      * *Stage 2:* Copies the build output to an Nginx image for high-performance serving.
  * **`frontend/frontend.conf`**: A custom Nginx configuration file. Since Angular is a Single Page Application (SPA), this config ensures that all routing requests are redirected to `index.html`, preventing 404 errors on refresh.
  * **`.github/workflows/ci-cd.yml`**: The automation workflow. If changes are pushed to GitHub, this script:
    1.  Builds new Docker images.
    2.  Pushes them to Docker Hub.
    3.  SSHs into the deployment VM.
    4.  Pulls the latest images and recreates the containers without downtime.

-----

## 🛠 Prerequisites

  * **Docker Desktop** (or Docker Engine + Compose plugin).
  * **Node.js & NPM** (Only if running locally without Docker).
  * **Git**.

-----

## 💻 How to Run (Docker Method - Recommended)

This is the fastest way to run the application without installing Node or Mongo locally.

1.  **Clone the Repository**

    ```bash
    git clone <repository-url>
    cd <project-folder>
    ```

2.  **Build and Run the Stack**

    ```bash
    docker compose up --build -d
    ```

      * `--build`: Forces a rebuild of the images.
      * `-d`: Detached mode (runs in the background).

3.  **Access the Application**

      * **Frontend:** Open `http://localhost` (Running on Port 80).
      * **Backend API:** Accessible at `http://localhost:8080/api/tutorials`.

4.  **Stop the Containers**

    ```bash
    docker compose down
    ```

-----

## 🖥 How to Run (Local Legacy Method)

If you need to develop without Docker:

### 1\. Setup Backend

```bash
cd backend
npm install
# Ensure you have a local MongoDB instance running or update app/config/db.config.js
node server.js
```

  * Server runs on: `http://localhost:8080`

### 2\. Setup Frontend

```bash
cd frontend
npm install
# Update src/app/services/tutorial.service.ts if backend URL changes
ng serve --port 8081
```

  * Client runs on: `http://localhost:8081`

-----

## ⚙️ CI/CD Pipeline Configuration

To enable the automated deployment via GitHub Actions:

### 1\. GitHub Secrets

Go to your GitHub Repository -\> **Settings** -\> **Secrets and variables** -\> **Actions** and add the following repository secrets:

| Secret Name | Description |
| :--- | :--- |
| `DOCKERHUB_USERNAME` | Your Docker Hub username. |
| `DOCKERHUB_TOKEN` | Your Docker Hub Access Token. |
| `SSH_HOST` | The public IP address of your Ubuntu VM. |
| `SSH_USER` | The username for the VM (e.g., `ubuntu` or `root`). |
| `SSH_PRIVATE_KEY` | The private SSH key content to access the VM. |

### 2\. VM Setup (Deployment Target)

On your cloud server (AWS EC2 / DigitalOcean / Azure):

1.  Install Docker and Docker Compose (v2).
2.  Ensure the `docker-compose.yml` file is present in the deployment folder (e.g., `~/app`).
3.  Add the user to the Docker group to run commands without sudo:
    ```bash
    sudo usermod -aG docker $USER
    ```

### 3\. Trigger Deployment

1.  Push code to the `main` branch.
2.  Watch the **Actions** tab in GitHub to see the build and deploy process.

-----

## 📂 Project Structure

```text
├── backend/
│   ├── app/                # API Controllers, Models, Routes
│   ├── Dockerfile          # Backend Docker configuration
│   ├── server.js           # Entry point
│   └── package.json
├── frontend/
│   ├── src/                # Angular Source Code
│   ├── Dockerfile          # Frontend Docker configuration
│   ├── frontend.conf       # Nginx SPA configuration
│   └── package.json
├── .github/workflows/
│   └── ci-cd.yml           # GitHub Actions Pipeline
├── docker-compose.yml      # Orchestration of Database, Backend, and Frontend
└── README.md
```

### Next Steps for you

Would you like me to write the content for the specific `frontend.conf` or the `ci-cd.yml` file to ensure the syntax matches exactly what is described in this README?
