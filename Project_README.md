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

