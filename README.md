# MERN Docker Compose Deployment

Deploying a simple MERN (MongoDB, Express, React, Node.js) application using Docker and Docker Compose.

This project demonstrates:

- Dockerizing frontend and backend applications
- Running MongoDB as a separate container
- Multi-network container communication
- Docker Compose orchestration
- Container isolation using Docker networks

---

# Simple Architecture

- Frontend → React Application
- Backend → Node.js + Express API
- Database → MongoDB Container

Network Flow:

- Frontend container → `frontend-network`
- Backend container → `frontend-network` + `backend-network`
- MongoDB container → `backend-network`

---

# Architecture Diagram

```text
                 ┌────────────────────┐
                 │     Frontend       │
                 │      React         │
                 │  frontend-network  │
                 └─────────┬──────────┘
                           │
                           │
                 ┌─────────▼──────────┐
                 │      Backend       │
                 │   Node + Express   │
                 │ frontend-network   │
                 │ backend-network    │
                 └─────────┬──────────┘
                           │
                           │
                 ┌─────────▼──────────┐
                 │      MongoDB       │
                 │  backend-network   │
                 └────────────────────┘
```

---

# Project Structure

```bash
MERN-docker-compose/
│
├── docker-compose.yml
├── README.md
├── .gitignore
│
├── frontend/
│   ├── Dockerfile
│   ├── cypress
│   ├── public
│   ├── src
│   ├── .eslintrc.cjs
│   ├── index.html
│   ├── cypress.json
│   ├── postcss.config.js
│   ├── tailwind.config.js
│   ├── vite.config.js
│   ├── package.json
│   └── package-lock.json
│
├── backend/
│   ├── Dockerfile
│   ├── db
│   ├── routes
│   ├── server.js
│   ├── package.json
│   └── package-lock.json

```

---

# How Container Communication Works

## Frontend → Backend

Frontend communicates with backend through `frontend-network`.

---

## Backend → MongoDB

Backend communicates with MongoDB through `backend-network`.

---

## Why Backend Uses Both Networks

Backend acts as a bridge between:

- Frontend
- MongoDB

Therefore backend needs connectivity to both networks.

This setup improves:

- Security
- Isolation
- Scalability
- Production-style architecture

---

# Run Containers Separately

This method builds and runs each container individually.

---

# Step 1: Create Docker Networks

```bash
docker network create frontend-network

docker network create backend-network
```

---

# Step 2: Pull MongoDB Image

```bash
docker pull mongo:7.0
```

---

# Step 3: Run MongoDB Container

```bash
docker run -d \
--network backend-network \
--name mongodb \
-p 27017:27017 \
mongo:7.0
```

---

# Step 4: Build Backend Image

```bash
cd backend

docker build -t mern-backend .
```

---

# Step 5: Run Backend Container

```bash
docker run -d \
--name mern-backend \
--network frontend-network \
-p 5050:5050 \
mern-backend
```

---

# Step 6: Connect Backend Container to backend-network

```bash
docker network connect backend-network mern-backend
```

Backend is now connected to:

- frontend-network
- backend-network

---

# Step 7: Build Frontend Image

```bash
cd frontend

docker build -t mern-frontend .
```

---

# Step 8: Run Frontend Container

```bash
docker run -d \
--name mern-frontend \
--network frontend-network \
-p 8081:80 \
mern-frontend
```

---

# Run Using Docker Compose

Docker Compose automatically:

- Builds frontend and backend images
- Pulls MongoDB image from Docker Hub
- Creates networks
- Starts all containers together
- Handles container communication

---

# Start Entire Application

```bash
docker compose up -d
```

---

# Build and Start Containers

```bash
docker compose up --build -d
```

---

# Stop Containers

```bash
docker compose down
```

---

# View Running Containers

```bash
docker ps
```

---

# View Logs

```bash
docker compose logs -f
```

---

# Access Application

| Service | URL |
|---|---|
| Frontend | http://localhost:8081 |
| Backend API | http://localhost:5050 |
| MongoDB | mongodb://localhost:27017 |

---

# Benefits of Docker Compose

- Single command deployment
- Easy container orchestration
- Automatic networking
- Easier troubleshooting
- Simplified environment management
- Faster setup process

---

# Useful Docker Commands

## List Running Containers

```bash
docker ps
```

---

## List Docker Networks

```bash
docker network ls
```

---

## Inspect Network

```bash
docker network inspect backend-network
```

---

## View Container Logs

```bash
docker logs mern-backend
```

---

# Tech Stack

- React
- Node.js
- Express.js
- MongoDB
- Docker
- Docker Compose
- NGINX

---

# Clone Repository

```bash
git clone https://github.com/Moorthy-M/MERN-docker-compose.git

cd MERN-docker-compose
```

---

# Run Project

```bash
docker compose up --build
```

---

# License

This project is created for learning and demonstration purposes.