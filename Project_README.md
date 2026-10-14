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
# Clone Git repo
## Step 1

```
$ git clone https://github.com/learndevopsabhash/Orchestration_Scaling_MERN_with_Microservices.git
```
<img width="1563" height="483" alt="image" src="https://github.com/user-attachments/assets/ffdb5e2d-cecd-4403-a489-53cfb2d3e142" />

# Local Project Run

## Step 1

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

## Step 2

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

## Step 3

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


## Step 4

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

________________________________________________________________________________________________________________________________
# Creating  DockerFiles for - Frontend, Hello-Service & profile-Service

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

## For profile-Service
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

## For Frontend-Service
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

# Setup Amazon Elastic Kubernetes Service (EKS)

## Step 1 - Go to EKS page and select cluster then click on **"Create Cluster"**
Select - Custom configuration, Disable EKS auto mode, Cluster name - **mern-eks-cluster**, select cluster IAM role or create if you do not have, and do other setting accordingly.

<img width="1919" height="838" alt="image" src="https://github.com/user-attachments/assets/7e5b0eb9-8697-4e62-8b79-4388cf8a51ed" />

<img width="1390" height="199" alt="image" src="https://github.com/user-attachments/assets/cab2e3e9-7e32-44b9-bc8c-74648e58e8b2" />

Once Cluster is active - Create Node group
Go to Compute and select Node Group the **"Add"**  
<img width="1915" height="864" alt="image" src="https://github.com/user-attachments/assets/046ea791-2209-4e54-b50f-bfe8b9577e1c" />
<img width="1919" height="766" alt="image" src="https://github.com/user-attachments/assets/b4f89749-91b9-4463-98f4-ba8e9592819b" />


## Check the frontend API URLs
Before creating Kubernetes Deployments, we need to address one important issue: your React frontend previously called http://localhost:3001/ and http://localhost:3002/fetchUser.
We need to configure routing between the frontend and backend services.

frontend/src/components/Home.js
The file currently calls:
```
- http://localhost:3001/ — Hello Service
- http://localhost:3002/fetchUser — Profile Service

Change to 
- .get("http://localhost:3001/") to .get("/hello/")
- .get("http://localhost:3002/fetchUser") to .get("/profile/fetchUser")
```
Home.js is updated, and both API requests now use relative URLs instead of localhost.

## Update frontend Dockerfile

Before Update frontend Dockerfile 
Create the Nginx reverse-proxy configuration

frontend/nginx.conf
```
server {
    listen 80;
    server_name _;

    root /usr/share/nginx/html;
    index index.html;

    location = /hello {
        proxy_pass http://hello-service:3001/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location /profile/ {
        proxy_pass http://profile-service:3002/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

Now Update the frontend Dockerfile
Add code on stage 2 
```
# ---------- Stage 2: Serve with Nginx ----------
FROM nginx:alpine AS production

COPY --from=build /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

Rebuild Docker

```
docker build -t frontend:1.2 ./frontend
```

Tag the image for Amazon ECR
```
docker tag frontend:1.2 057079472578.dkr.ecr.ap-south-1.amazonaws.com/frontend:1.2
```

Push the updated image to ECR
```
docker push 057079472578.dkr.ecr.ap-south-1.amazonaws.com/frontend:1.2
```

Verify the image in ECR

Verify the backend image tags
<img width="1331" height="398" alt="image" src="https://github.com/user-attachments/assets/435f19f1-bb0d-4dde-97c8-f450422d85c2" />

Verify the backend image tags
<img width="1350" height="979" alt="image" src="https://github.com/user-attachments/assets/607a183f-b6cd-4c94-a0a0-65239559368c" />

## Kubernetes Deployment

### Create the Hello Service Kubernetes Deployment

Create k8s folder if not in the project root folder

Create - hello-deployment.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-service
  namespace: mern-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-service
  template:
    metadata:
      labels:
        app: hello-service
    spec:
      containers:
        - name: hello-service
          image: 057079472578.dkr.ecr.ap-south-1.amazonaws.com/hello-service:1.0
          env: 
            - name: PORT
              value: "3001"
          ports:
            - containerPort: 3001
          resources:
            requests:
              cpu: "100m"
              memory: "128Mi"
            limits:
              cpu: "500m"
              memory: "512Mi"
          readinessProbe:
            httpGet:
              path: /health
              port: 3001
            initialDelaySeconds: 5
            periodSeconds: 10
```

dry-run to check the deployment

```
kubectl apply --dry-run=client -f K8s/hello-deployment.yaml
```
<img width="1308" height="87" alt="image" src="https://github.com/user-attachments/assets/219aeea6-1605-4083-a4c9-c13fc69b4b77" />

<img width="1324" height="298" alt="image" src="https://github.com/user-attachments/assets/53b62977-a6d5-435f-be4a-49b8b97e1bc1" />

Deploy Hello Service to EKS
```
kubectl apply -f K8s/hello-deployment.yaml
```
<img width="1308" height="95" alt="image" src="https://github.com/user-attachments/assets/7aa8dd40-19ae-4ec7-a91c-20a31fa5286e" />


Check the Hello Service pod
<img width="1325" height="153" alt="image" src="https://github.com/user-attachments/assets/30d6228e-3f2d-43c2-96fa-6eb45d99e33c" />

Create a Kubernetes Service
Create - K8s/hello-service.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: hello-service
  namespace: mern-app
spec:
  type: ClusterIP
  selector:
    app: hello-service
  ports:
    - port: 3001
      targetPort: 3001
      protocol: TCP
```
Apply and check hello Kubernetes Service
<img width="1341" height="191" alt="image" src="https://github.com/user-attachments/assets/939b2ede-f06b-4c58-b2ca-d03009ba9fee" />

<img width="1902" height="205" alt="image" src="https://github.com/user-attachments/assets/b2544dde-89c4-457c-a626-66d3b898159f" />

### create secret for Database MONGO_URI

<img width="1341" height="237" alt="image" src="https://github.com/user-attachments/assets/6d49c0d9-c132-4f70-95ef-5785179ca8d1" />

Create profile-deployment
Create  K8s/profile-deployment.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: profile-service
  namespace: mern-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: profile-service
  template:
    metadata:
      labels:
        app: profile-service
    spec:
      containers:
        - name: profile-service
          image: 057079472578.dkr.ecr.ap-south-1.amazonaws.com/profile-service:1.0
          env:
            - name: PORT
              value: "3002"
            - name: MONGO_URL
              valueFrom:
                secretKeyRef:
                  name: profile-service-secret
                  key: MONGO_URL
          ports:
            - containerPort: 3002
          readinessProbe:
            httpGet:
              path: /health
              port: 3002
            initialDelaySeconds: 10
            periodSeconds: 10
```
Now Apply and check

<img width="1895" height="407" alt="image" src="https://github.com/user-attachments/assets/6d0712e4-b425-4f07-8e91-0a9a66b2b9bb" />

Create profile-service
Create  K8s/profile-service.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: profile-service
  namespace: mern-app
spec:
  type: ClusterIP
  selector:
    app: profile-service
  ports:
    - port: 3002
      targetPort: 3002
      protocol: TCP
```
<img width="1900" height="366" alt="image" src="https://github.com/user-attachments/assets/70667810-917a-4f08-bcfe-8cf20575ee34" />


Create frontend-deployment
Create k8s/frontend-deployment.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  namespace: mern-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
        - name: frontend
          image: 057079472578.dkr.ecr.ap-south-1.amazonaws.com/frontend:1.2
          ports:
            - containerPort: 80
          readinessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 5
            periodSeconds: 10
```
Now Apply and check
<img width="1901" height="754" alt="image" src="https://github.com/user-attachments/assets/27bd6302-5f0a-4b09-8f62-f0e9ef8dba64" />

Create frontend-service
Create  K8s/frontend-service.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
  namespace: mern-app
spec:
  type: LoadBalancer
  selector:
    app: frontend
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
```
<img width="1904" height="452" alt="image" src="https://github.com/user-attachments/assets/41b0d305-f018-4ee4-ab57-f36c4c4901cd" />


Final check

<img width="1898" height="508" alt="image" src="https://github.com/user-attachments/assets/92d91b29-2a8c-4209-8c3c-e148680b0d65" />

<img width="1919" height="527" alt="image" src="https://github.com/user-attachments/assets/d20e9c17-2e33-4975-a7ad-d41cbc2804d0" />


### Deploying Helm

Check helm and helm version, list helm, and create helm chart
<img width="1505" height="794" alt="image" src="https://github.com/user-attachments/assets/e33336f2-b8c0-43c2-bf40-02866faf378c" />

Update values.yaml
```
namespace: mern-app

frontend:
  replicaCount: 1
  image:
    repository: 057079472578.dkr.ecr.ap-south-1.amazonaws.com/frontend
    tag: "1.2"
    pullPolicy: IfNotPresent
  service:
    type: LoadBalancer
    port: 80
    targetPort: 80

helloService:
  replicaCount: 1
  image:
    repository: 057079472578.dkr.ecr.ap-south-1.amazonaws.com/hello-service
    tag: "1.0"
    pullPolicy: IfNotPresent
  port: 3001
  env:
    PORT: "3001"

profileService:
  replicaCount: 1
  image:
    repository: 057079472578.dkr.ecr.ap-south-1.amazonaws.com/profile-service
    tag: "1.0"
    pullPolicy: IfNotPresent
  port: 3002
  env:
    PORT: "3002"
  existingSecret: profile-service-secret
  mongoUrlSecretKey: MONGO_URL
```

Create Helm file for helm deployments

1. mern-app-chart\templates\frontend-deployment.yaml
2. mern-app-chart\templates\frontend-service.yaml
3. mern-app-chart\templates\hello-deployment.yaml
4. mern-app-chart\templates\hello-service.yaml
5. mern-app-chart\templates\profile-deployment.yaml
6. mern-app-chart\templates\profile-service.yaml

<img width="472" height="689" alt="image" src="https://github.com/user-attachments/assets/13047a91-f436-498b-9b33-b971292cfc18" />

Remove the unused default Helm templates
```
rm -f mern-app-chart/templates/deployment.yaml \
      mern-app-chart/templates/service.yaml \
      mern-app-chart/templates/hpa.yaml \
      mern-app-chart/templates/ingress.yaml \
      mern-app-chart/templates/httproute.yaml \
      mern-app-chart/templates/serviceaccount.yaml
```

Validate the Helm chart
```
helm lint mern-app-chart
```
<img width="1297" height="147" alt="image" src="https://github.com/user-attachments/assets/c6a9b750-1145-48d5-a209-d6a182a3a8b7" />

Render the Helm chart
Before deploying anything to EKS, we need to check the Kubernetes YAML that Helm generates from your templates and values.yaml.
```
helm template mern-app mern-app-chart --namespace mern-app
```
<img width="1305" height="969" alt="image" src="https://github.com/user-attachments/assets/af323962-c181-4e1e-a2c8-0fa6bd97962c" />
<img width="1219" height="945" alt="image" src="https://github.com/user-attachments/assets/e1ede240-dd7e-420b-995f-319881ad5957" />
<img width="1237" height="963" alt="image" src="https://github.com/user-attachments/assets/b91cb3ce-aded-4998-8f74-2e00eeb4f2dc" />
<img width="1311" height="527" alt="image" src="https://github.com/user-attachments/assets/db932f33-10ca-44a2-931a-3da500d31d57" />

Check how Helm can take ownership of existing resources
```
helm upgrade --help | grep -A 2 -B 2 take-ownership
```
<img width="1896" height="194" alt="image" src="https://github.com/user-attachments/assets/0ff5eb0c-1a15-49de-a851-fccaeb2b9f38" />

Back up the current Kubernetes configuration
```
kubectl get deployments,services -n mern-app -o yaml > mern-app-resources-backup.yaml
```

Preview the Helm migration safely
```
helm upgrade --install mern-app mern-app-chart \
  --namespace mern-app \
  --take-ownership \
  --dry-run=server
```
<img width="1324" height="963" alt="image" src="https://github.com/user-attachments/assets/622fa527-8444-4435-8294-c3032d6a5411" />
<img width="1275" height="964" alt="image" src="https://github.com/user-attachments/assets/34e326ae-8894-46ef-b0e6-722432cd1789" />
<img width="1280" height="964" alt="image" src="https://github.com/user-attachments/assets/a2209d3c-81a9-4f31-8c32-03dd086080b8" />
<img width="1300" height="813" alt="image" src="https://github.com/user-attachments/assets/af105544-5c4c-4bb4-b5d4-1d56c0b62b17" />


Perform the Helm migration
```
helm upgrade --install mern-app mern-app-chart \
  --namespace mern-app \
  --take-ownership \
  --server-side=true \
  --force-conflicts \
  --wait \
  --timeout 5m
```
<img width="1306" height="377" alt="image" src="https://github.com/user-attachments/assets/159a962f-8d05-463b-ac1e-6c52106e1a71" />
Helm deployment succeeded.


Verify the application after migration
```
kubectl get deployments,pods,services -n mern-app
```
<img width="1889" height="481" alt="image" src="https://github.com/user-attachments/assets/4a0c76b1-9077-4b7e-af10-f9cec033267d" />
