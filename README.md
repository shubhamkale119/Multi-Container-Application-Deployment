# Multi-Container-Application-Deployment with Docker Compose and Kubernetes

## 1. Overview

This document explains the architecture, deployment strategy, and step-by-step instructions for building, deploying, and managing a multi-container application consisting of three services:

* Frontend: React application

* Backend: Node.js API

* Database: PostgreSQL

The deployment is managed using Docker Compose for local development. The application demonstrates container orchestration, container networking, and dependency management across services.

## 2. Architecture

Application Components:

1. Frontend: Github Repo  ``` https://github.com/Anand-1432/Techdome-frontend ```
* React application, served via port 3000.

2. Backend: Githib Repo ```https://github.com/Anand-1432/Techdome-backend```
* Backend with Node.js, exposed on port 5000.
* Serves the API to interact with the database.
* Uses PostgreSQL as its database.

3. Database:
* PostgreSQL database, running on port 5432.
* Stores application data and is accessible by the backend.

## Architecture Diagram:


## 3. Deployment Strategy
### Local Development: Docker Compose

For local development, we use Docker Compose to run all the containers on a single network, enabling communication between the frontend, backend, and database.

* Frontend connects to the backend API.
* Backend interacts with the database.

## 4. Docker Compose Configuration
In the root directory of the project, create a docker-compose.yml file. This file will define the services for Frontend, Backend, and Database, and will ensure they are networked together.

### Directory Structure:
```bash
multi-container-app/
├── backend/
│   ├── Dockerfile
│   ├── index.js
│   └── ...
├── frontend/
│   ├── Dockerfile
│   └── ...
├── docker-compose.yml
└── README.md
```


Here's docker-compose.yml file

```version: '3'

services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend
    networks:
      - app-network
  
  backend:
    build: ./backend
    ports:
      - "5000:5000"
    environment:
      DATABASE_URL: postgres://postgres:password@db:5432/mydatabase
    depends_on:
      - db
    networks:
      - app-network
  
  db:
    image: postgres:16
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydatabase
    ports:
      - "5432:5432"
    networks:
      - app-network
  
networks:
  app-network:
    driver: bridge
```

## 5. Building and Deploying

### Step-by-Step Instructions

### 1. Clone the Repositories:

Clone both the frontend and backend repositories into the appropriate directories.

``` git clone https://github.com/Anand-1432/Techdome-frontend ```

``` git clone https://github.com/Anand-1432/Techdome-backend ```

### 2. Create a Dockerfile for each frontend as well as for backend here`s dockerfiles for each frontend and backend

Dockerfile for Frontend:
```
# Official node image 
FROM node:16-alpine

# working directory in the container
WORKDIR /app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install


# Copy the rest of the application code
COPY . .

# Build the application
RUN yarn build

# Serve the application using a simple web server
RUN npm install -g serve
CMD ["serve", "-s", "build"]


# Expose the frontend port
EXPOSE 3000
```

Dockerfile for Backend:

```
#Official Node.js image
FROM node:16-alpine


# Working directory  
WORKDIR /app


# Copy the package.json and install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the backend application
COPY . .


# Start the application
CMD ["npm", "start"]

# Expose the backend port
EXPOSE 5000
```

