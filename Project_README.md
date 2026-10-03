# Project architecture

```
                                      ┌─────────────────┐
                                      │    Frontend     │
                                      │     React       │
                                      └────────┬────────┘
                                               │
                                   ┌───────────┴───────────┐
                                   │                       │
                                   ▼                       ▼
                          ┌─────────────────┐    ┌─────────────────┐
                          │  helloService   │    │ profileService  │
                          │     :3001       │    │     :3002       │
                          └─────────────────┘    └────────┬────────┘
                                                          │
                                                          ▼
                                                    ┌─────────────┐
                                                    │   MongoDB   │
                                                    └─────────────┘

```
# Step 1
## Clone Git repo

```
$ git clone https://github.com/learndevopsabhash/Orchestration_Scaling_MERN_with_Microservices.git
```
<img width="1563" height="483" alt="image" src="https://github.com/user-attachments/assets/ffdb5e2d-cecd-4403-a489-53cfb2d3e142" />

# Local Project Run

# Step 1

## Inspect the application configuration

Before we run the application, I want you to understand the dependencies and start commands.

## Inspect Project Services
1. helloService/package.json
2. profileService/package.json
3. frontend/package.json
In main git/ project folder
```
cat backend/helloService/package.json
cat backend/profileService/package.json
cat frontend/package.json
```

# Step 2

## Read the actual backend code
Why we read - we're looking for 
1. PORT
2. Express
3. Routes
4. Health check
5. Response
6. MongoDB connection
7. MONGO_URL
8. User operations

Especially look for things like: **process.env.PORT & process.env.MONGO_URL**

```
cat backend/helloService/index.js
cat backend/profileService/index.js
```

# Step 3

## Now installing Dependency

Go to -> cd backend/helloService
```
npm install
```
<img width="1611" height="618" alt="image" src="https://github.com/user-attachments/assets/270a2028-3eac-4764-b49a-f7ed67526c09" />

Go to -> cd backend/profileService
```
npm install
```
<img width="1631" height="448" alt="image" src="https://github.com/user-attachments/assets/6dee933c-6828-485a-a33d-f0d016e2c4a9" />

Go to -> cd frontend/
```
npm install
```
<img width="1911" height="934" alt="image" src="https://github.com/user-attachments/assets/9c2b3392-1f5c-4a59-bacb-128aa82b6d5e" />


# Step 4

## Create the .env file for Backend

1. For HelloService
```
echo "PORT=3001" > .env
cat .env
```
<img width="380" height="78" alt="image" src="https://github.com/user-attachments/assets/d805b003-18f0-4044-8d1f-fe0f7f75f4bb" />
_______________________________________________________________________________________________________________________________________________________

2. For profileService
```
nano .env

PORT=3002
MONGO_URL=mongodb+srv://abhash_db_user:<passoword>@mydb.lyrtqvv.mongodb.net/?appName=mydb"
```
<img width="1577" height="103" alt="image" src="https://github.com/user-attachments/assets/c74c280e-b89b-462c-9ae1-f9124c648212" />

# Start node service

<img width="1911" height="934" alt="image" src="https://github.com/user-attachments/assets/72d612a2-686a-4c62-ab94-b1be4af46bd4" />


<img width="1908" height="588" alt="image" src="https://github.com/user-attachments/assets/e00bbd97-a75c-429b-b00c-2d358f6671e9" />


Application is now fully working locally
* helloService → port 3001
* profileService → port 3002 + MongoDB
* React frontend → port 3000


# Prepere for Docker File for all Services - Frontend, Hello-Service & profile-Service

# For Hello-Service
## Step 1 — Create Dockerfile for helloService
Go to helloService and create Dockerfile
```
nano Dockerfile
```
```
# ---------- Stage 1: Dependencies ----------
FROM node:24-alpine AS dependencies
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev

# ---------- Stage 2: Production ----------
FROM node:24-alpine AS production
WORKDIR /app
COPY --from=dependencies /app/node_modules ./node_modules
COPY index.js ./
COPY package*.json ./
EXPOSE 3001
CMD ["node", "index.js"]
```

## Step 2 — Build the Docker image

```
docker build -t hello-service:1.0 .
```
<img width="1912" height="175" alt="image" src="https://github.com/user-attachments/assets/32c59e4f-ac63-4e6b-b15f-2b107d734bdb" />

## Step 3 — Run the container

```
docker run -d --name hello-service-container -p 3001:3001 -e PORT=3001 hello-service:1.0
```

<img width="1646" height="195" alt="image" src="https://github.com/user-attachments/assets/9cdc4c87-5308-4ac3-87f1-7dfae5995da8" />

## Step 4 — Verify the running container
```
docker  ps
```

<img width="1907" height="157" alt="image" src="https://github.com/user-attachments/assets/c9e44c29-3b2d-4850-88b2-57111b6c8b05" />

# For profile-Service
## Step 1 — Create Dockerfile for profile
Go to helloService and create Dockerfile
```
nano Dockerfile
```
```
# ---------- Stage 1: Dependencies ----------
FROM node:24-alpine AS dependencies
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev

# ---------- Stage 2: Production ----------
FROM node:24-alpine AS production
WORKDIR /app
COPY --from=dependencies /app/node_modules ./node_modules
COPY index.js ./
COPY package*.json ./
EXPOSE 3002
CMD ["node", "index.js"]
```

## Step 2 — Build the Docker image

```
docker build -t profile-service:1.0 .
```
## Step 3 — Run the container

```
docker run -d --name profile-service-container -p 3002:3002 --env-file .env profile-service:1.0
```
<img width="1585" height="92" alt="image" src="https://github.com/user-attachments/assets/7d619a1e-ef17-4be0-8ac5-5e0706aba88a" />


## Step 4 — Verify the running container
```
docker  ps
```
<img width="1898" height="126" alt="image" src="https://github.com/user-attachments/assets/156ba5b1-544b-42ba-98c7-5c01b3a723b6" />

<img width="1590" height="373" alt="image" src="https://github.com/user-attachments/assets/9c47ba0d-6f9d-4345-93fd-39742829a41a" />

# For Frontend-Service
## Step 1 — Create Dockerfile for Frontend
Go to helloService and create Dockerfile
```
nano Dockerfile
```
```
# ---------- Stage 1: Build React Application ----------
FROM node:24-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# ---------- Stage 2: Serve with Nginx ----------
FROM nginx:alpine AS production
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## Step 2 — Build the Docker image

```
docker build -t frontend:1.0 .
```
<img width="1904" height="134" alt="image" src="https://github.com/user-attachments/assets/a8dde1fc-36c2-43c7-8c01-c810b513b066" />

## Step 3 — Run the container

```
$ docker run -d --name frontend-container -p 8080:80 frontend:1.0
```
<img width="1437" height="98" alt="image" src="https://github.com/user-attachments/assets/8d8c37db-6c5a-412f-8cb3-8e03949c9edb" />


## Step 4 — Verify the running container
```
docker  ps
```
<img width="1911" height="217" alt="image" src="https://github.com/user-attachments/assets/046844ce-3a31-4c1a-b7d2-215cef582661" />

<img width="1902" height="171" alt="image" src="https://github.com/user-attachments/assets/f286217d-ce35-42fd-973c-760616d18380" />

# SetUp -ECR repository

## Step 1 Create the ECR repository

```
aws ecr create-repository --repository-name hello-service --region ap-south-1
aws ecr create-repository --repository-name profile-service --region ap-south-1
aws ecr create-repository --repository-name frontend --region ap-south-1
```
<img width="1301" height="935" alt="image" src="https://github.com/user-attachments/assets/cf47d21f-0f8d-45dc-9f04-843d0a5139d6" />
<img width="1305" height="463" alt="image" src="https://github.com/user-attachments/assets/d6fe004d-60b8-49c2-b545-6fbfb12e464c" />

## Step 2 Authenticate Docker with ECR to push Docker image into AWS and docker Tag to tag image as per ECR

```
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 057079472578.dkr.ecr.ap-south-1.amazonaws.com

docker tag hello-service:1.0 057079472578.dkr.ecr.ap-south-1.amazonaws.com/hello-service:1.0
docker push 057079472578.dkr.ecr.ap-south-1.amazonaws.com/hello-service:1.0

docker tag profile-service:1.0 057079472578.dkr.ecr.ap-south-1.amazonaws.com/profile-service:1.0
docker push 057079472578.dkr.ecr.ap-south-1.amazonaws.com/profile-service:1.0

docker tag frontend:1.1 057079472578.dkr.ecr.ap-south-1.amazonaws.com/frontend:1.1
docker push 057079472578.dkr.ecr.ap-south-1.amazonaws.com/frontend:1.1
```
<img width="1915" height="274" alt="image" src="https://github.com/user-attachments/assets/6ad465b5-ceac-4105-ab73-f365977ec693" />
<img width="1317" height="696" alt="image" src="https://github.com/user-attachments/assets/767b8b66-e5d8-4680-994f-81005d9f0105" />
<img width="1308" height="421" alt="image" src="https://github.com/user-attachments/assets/2636f063-bfb0-4e3b-8e6e-1bfa6131ffb6" />

## Step 3 Verify ECR
```
aws ecr describe-repositories --region ap-south-1
```
<img width="1303" height="989" alt="image" src="https://github.com/user-attachments/assets/ee564652-4c61-4470-8f9f-dffe17437520" />
