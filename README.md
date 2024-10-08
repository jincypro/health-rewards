# health-rewards
Based on user's activity reward user with neighborhood store discounts

Activity-Based Rewards Application: Full Workflow Documentation
This document provides a comprehensive guide for setting up, running, and understanding the workflow of an Activity-Based Rewards Application. The application tracks user activities via Fitbit, awards points, and offers rewards, and it's built using a tech stack that includes Go (for backend services), Node.js (for the API layer), React (for the frontend), PostgreSQL (for database storage), and Redis (for caching). The entire application runs on Docker containers to simplify setup and deployment.



## Introduction
The Activity-Based Rewards application encourages physical activity by rewarding users with points for specific activities. These points can later be redeemed for rewards. The backend processes user data (currently integrated with Fitbit), while the frontend displays activity details and reward points.

## System Requirements
Before starting, ensure you have the following:

Docker: Version 20+ (to run containers).
Docker Compose: Version 1.27+ (for orchestration).
Node.js: Version 18+ (for the frontend and API).
Go: Version 1.20+ (for backend services).
PostgreSQL: Version 13+ (for the database).
Redis: Latest version (for caching rewards).

## Architecture Overview
The application is built using a microservices architecture:

Backend (Go): Manages services like UserService, ActivityService, and RewardsService.
API (Node.js): Provides an API interface to communicate between the frontend and backend services.
Frontend (React): Presents the user interface where users can view their activity and rewards.
Database (PostgreSQL): Stores user, activity, and reward data.
Cache (Redis): Stores rewards for quicker access.

## Application Setup

### Step 1: Cloning the Repository
Start by cloning the repository:

`
git clone https://github.com/your-username/reward-app.git
cd reward-app
`
### Step 2: Docker Installation
Ensure Docker is installed on your system:

For Linux/Mac: Install Docker from official Docker docs.
For Windows: Download Docker Desktop.
Run the following to check Docker:

`
docker --version
docker-compose --version
`
### Step 3: Database Setup
Initialize PostgreSQL with Docker:

`docker run --name postgres-container -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin_pass -e POSTGRES_DB=reward_db -p 5432:5432 -d postgres`

#### Initialize Redis with Docker:

`docker run --name redis-container -p 6379:6379 -d redis`

#### Create Database Schema: 
Use the provided init.sql file to create the necessary tables:


`psql -U admin -d reward_db -f ./scripts/init.sql`
### Step 4: Building and Running the Application
Use Docker Compose to build and run the entire stack.

Build and Run Services:

`docker-compose up --build`
This command:
Builds Go backend services.
Builds Node.js API.
Builds React frontend.
Starts PostgreSQL and Redis containers.
## Frontend: React Setup
The React frontend provides a user-friendly interface where users can view their activity history and rewards.

### React Setup: 
The frontend is set up using Create React App and communicates with the backend via API.

#### Frontend Directory Structure:

`
/frontend
  /src
    - App.js
    - components/
    - services/
  - package.json
`
#### Building the React App:

`
cd frontend
npm install
npm run build
The production build of React will be served from the /build directory.
`
## Accessing the Frontend:

Once Docker is up, access the frontend at http://localhost:3000.
## Backend: Go Services Setup
The backend services (User, Activity, Rewards) are written in Go and handle core business logic.

### Service Overview:

UserService: Handles user registration and profile management.
ActivityService: Handles activity logging and retrieval.
RewardsService: Calculates and distributes rewards.
Running the Backend: The backend services are built as Go binaries during the Docker build. These services run inside the container and communicate with the API via HTTP.

### Access Backend Services: 
Services run on ports 8081, 8082, and 8083.

## API: Node.js Setup
The Node.js API serves as a gateway between the frontend and the backend services.

### API Directory Structure:

`
/api
  - app.js
  - routes/
  - controllers/
  - package.json
`
### Installing Dependencies: If running locally:

`
cd api
npm install
`
Starting the API: The API is exposed at http://localhost:8080.

## Testing the Application
### Access the Frontend: 
Go to http://localhost:3000 to view the user dashboard.

### Test API Endpoints: 
Use a tool like Postman or curl to test API routes.

Example:

`curl http://localhost:8080/api/activity`
### Database Verification:

Use PostgreSQL CLI to verify data:

`psql -U admin -d reward_db
SELECT * FROM users;`

## Deployment
For production deployment:

Push Docker Images to Docker Hub:


`docker tag reward-app:latest <dockerhub-username>/reward-app
docker push <dockerhub-username>/reward-app`

Deploy to a Cloud Platform (e.g., AWS, GCP):

Use Kubernetes or Docker Swarm to orchestrate containers.
## Troubleshooting
Container Not Starting:

Check logs:

`docker logs <container-id>`
Database Connection Error:

Verify POSTGRES_HOST, POSTGRES_USER, and POSTGRES_PASSWORD in environment variables.

## Conclusion
This guide walks you through setting up, running, and testing the Activity-Based Rewards application. From cloning the repository, building the services, and setting up Docker, to accessing the frontend and running API tests, this document serves as a complete workflow for developers and users alike.

For any issues or additional information, please refer to the troubleshooting section or reach out to the team .
