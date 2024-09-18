# Multi-Container-Application-Deployment with Docker Compose and Kubernetes

## 1. Overview

This document explains the architecture, deployment strategy, and step-by-step instructions for building, deploying, and managing a multi-container application consisting of three services:

* Frontend: React application

* Backend: Node.js API

* Database: PostgreSQL

The deployment is managed using Docker Compose for local development. The application demonstrates container orchestration, container networking, and dependency management across services.

## 2. Architecture

Application Components:

1. Frontend:
* React application, served via port 3000.

2. Backend:
* Backend with Node.js, exposed on port 5000.
* Serves the API to interact with the database.
* Uses PostgreSQL as its database.

3. Database:
* PostgreSQL database, running on port 5432.
* Stores application data and is accessible by the backend.

## Architecture Diagram:


## 3. Deployment Strategy
## Local Development: Docker Compose

For local development, we use Docker Compose to run all the containers on a single network, enabling communication between the frontend, backend, and database.
