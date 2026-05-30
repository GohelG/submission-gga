# Microservice Containerization and Orchestration
This project covers the containerization and orchestration of three Node.js microservices using Docker and Docker Compose.

## Project Architecture
The application consists of four main components communicating over an isolated internal Docker network:

* **User Service**: Handles user-related data on Port `3000`
* **Product Service**: Handles product-related data on Port `3001`
* **Order Service**: Handles order-related data on Port `3002`
* **Gateway Service**: Acts as the single entry point routing requests on Port `3003`
```text
┌─────────────────┐
│ API Gateway │ (Port 3003)
└────────┬────────┘
│
┌─────────────────────┼─────────────────────┐
▼ ▼ ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│ User Service │ │ Product Service │ │ Order Service │
│ (Port 3000) │ │ (Port 3001) │ │ (Port 3002) │
└─────────────────┘ └─────────────────┘ └─────────────────┘
```

## Directory Structure

```text
submission/
├── user-service/
│   └── Dockerfile
├── product-service/
│   └── Dockerfile
├── gateway-service/
│   └── Dockerfile
├── docker-compose.yml
└── README.md
```

## Setup and Deployment Instructions

### Prerequisites
* [Docker Desktop](https://docker.com) installed and running.
* Terminal or Command Prompt with administrative privileges.

### Steps to Run

1. **Navigate to the project root:**
   ```bash
   cd submission
   ```

2. **Build and start the containers:**
   Run the services in the background (detached mode):
   ```bash
   docker compose up --build -d
   ```

3. **Verify running containers:**
   Ensure all services show an active status:
   ```bash
   docker compose ps
   ```

4. **Stop the application:**
   To stop and remove the containers and network:
   ```bash
   docker compose down
   ```

## Service Testing and Validation

Validate service availability using a web browser, Postman, or `curl` commands in your terminal.

### Direct Service Verification

* **User Service:** [http://localhost:3000](http://localhost:3000)
* **Product Service:** [http://localhost:3001](http://localhost:3001)
* **Order Service:** [http://localhost:3002](http://localhost:3002)

### API Gateway Verification

* **Gateway Main Routing:** [http://localhost:3003](http://localhost:3003) (Checks general gateway health)
* **User Reverse Proxy Route:** [http://localhost:3003/users](http://localhost:3003/users) (Pulls data from downstream User Service)
* **Product Reverse Proxy Route:** [http://localhost:3003/products](http://localhost:3003/products) (Pulls data from downstream Product Service)

## Troubleshooting Guide

### Port Allocation Errors
* **Issue:** Errors occur when local processes or old containers utilize ports 3000, 3001, 3002, or 3003.
* **Fix:** Stop existing containers or kill the specific local processes running on those ports.
  ```bash
  # Stop existing docker-compose stacks
  docker compose down
  ```

### Gateway Connection Failures
* **Issue:** The gateway cannot connect to downstream services. This happens when using `localhost` inside container environment configurations instead of the Docker Compose service name aliases.
* **Fix:** Ensure your gateway configuration targets the exact service name hosts defined in your `docker-compose.yml` file (e.g., `http://user-service:3000`).

### Code Changes Not Reflecting
* **Issue:** Docker runs cached versions of your image layers.
* **Fix:** Force a clean build without using the cache:
  ```bash
  docker compose build --no-cache
  docker compose up -d
  ```