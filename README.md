
---

# Fullstack CI/CD Project (React + Node.js + Docker + NGINX + GitHub Actions)

This repository demonstrates a full-stack web application with an automated CI/CD pipeline using:

* React (Frontend)
* Node.js / Express (Backend)
* Docker (Multi-stage builds)
* docker-compose
* NGINX reverse proxy
* GitHub Actions for deployment via SSH

The setup focuses on production-grade containerization and seamless deployments.

---

## 1. Project Overview

* **Frontend (React):** Built and served by NGINX
* **Backend (Node.js):** API endpoints
* **NGINX:** Serves React app and proxies `/api` to backend
* **docker-compose:** Orchestrates services locally
* **GitHub Actions:** Automates deployment on `main` branch push via SSH to remote server

---

## 2. Architecture Diagram

```
           Client
             ↓
          ┌───────┐
          │ NGINX │
          └──┬────┘
       ┌─────┴─────┐
       │           │
┌───────────────┐ ┌──────────────┐
│ React Frontend│ │Node.js Backend│
└───────────────┘ └──────────────┘
```

NGINX handles static serving and API proxying.

---

## 3. Tech Stack Summary

| Component        | Technology                      |
| ---------------- | ------------------------------- |
| Frontend         | React                           |
| Backend          | Node.js / Express               |
| Reverse Proxy    | NGINX                           |
| Containerization | Docker (multi-stage)            |
| Orchestration    | docker-compose                  |
| CI/CD            | GitHub Actions (SSH deployment) |

---

## 4. Docker Setup

* **Frontend:** Multi-stage Docker build producing optimized React static files served via `nginx:alpine`
* **Backend:** Multi-stage build with minimal image size
* **Benefits:** Small image sizes, faster builds, production-ready containers

---

## 5. Local Development

Start all services:

```bash
docker-compose up -d --build
```

Access:

* Frontend: [http://localhost](http://localhost)
* Backend API: [http://localhost/api](http://localhost/api)

---

## 6. GitHub Actions Deployment

* Triggered on pushes to `main` branch
* Uses SSH key stored in **Repository Secrets** (`SSH_PRIVATE_KEY`, `SERVER_IP`, `SERVER_USER`)
* SSH into remote server, pull latest code, rebuild containers, restart services

Example deploy command on server:

```bash
cd /mnt/c/Users/hp/Desktop/fullstack-ci-cd
git pull
docker-compose down
docker-compose up -d --build
```

---

## 7. Secrets Configuration

Add the following secrets in GitHub repo settings → **Settings > Secrets and variables > Actions**:

* `SSH_PRIVATE_KEY`: Your private SSH key for authentication
* `SERVER_IP`: IP address of the deployment server
* `SERVER_USER`: SSH username

---

## 8. Deployment Flow

```
Developer pushes → GitHub Actions triggered
      ↓
Runner uses SSH key
      ↓
SSH connects to deployment server
      ↓
Pull latest code, rebuild, and restart containers
      ↓
✅ Application updated automatically
```

---

## 9. Folder Structure

```
project/
│── frontend/
│   ├── Dockerfile
│   ├── src/
│
│── backend/
│   ├── Dockerfile
│   ├── src/
│
│── nginx/
│   ├── nginx.conf
│
│── docker-compose.yml
│── .github/workflows/deploy.yml
│── README.md
```

---

## 10. Best Practices

* Multi-stage Docker builds for optimized images
* Secrets managed securely via GitHub Actions
* SSH-based deployment avoids exposing sensitive credentials
* Reproducible local development with docker-compose
* Clear separation of frontend, backend, and proxy layers

---
