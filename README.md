# Next.js DevOps Assessment

This repository contains a **Next.js application** deployed using **Docker** and **Kubernetes (Minikube)**. It demonstrates **containerization, deployment, and integration with GitHub Container Registry (GHCR)**.

---

## Table of Contents

- [Project Setup](#project-setup)
- [Dockerization](#dockerization)
- [GitHub Container Registry](#github-container-registry)
- [Kubernetes Deployment](#kubernetes-deployment)
- [Accessing the App](#accessing-the-app)
- [Git Workflow](#git-workflow)
- [Project Structure](#project-structure)
- [Screenshots](#screenshots)
- [References](#references)

---

## Project Setup

1. Clone this repository:

git clone https://github.com/Prabhas81/wexa-ai.git
cd wexa-ai
Install dependencies:

npm install
Run the development server locally:

npm run dev
App runs at http://localhost:3000

Dockerization
Build the Docker image:

docker build -t ghcr.io/prabhas81/wexa-ai:latest .
Run the container locally:

docker run --rm -p 3000:3000 ghcr.io/prabhas81/wexa-ai:latest
App available at: http://localhost:3000

Terminal Output Example:

> nextjs-devops-assessment@0.1.0 start
> next start

   ▲ Next.js 15.5.4
   - Local:        http://localhost:3000
   - Network:      http://172.17.0.2:3000

 ✓ Starting...
 ✓ Ready in 1475ms
GitHub Container Registry (GHCR)
Create a Personal Access Token (PAT) in GitHub with:

write:packages

read:packages

repo

Log in to GHCR:

echo <PAT> | docker login ghcr.io -u prabhas81 --password-stdin
Push the Docker image:

docker push ghcr.io/prabhas81/wexa-ai:latest
Push Output Example:

arduino
latest: digest: sha256:b8df60aabac20e6c73f7196554ac4ab17afde9022ece2585d47bdcd27b4002e0 size: 856
Kubernetes Deployment
1. Create Docker Registry Secret
kubectl create secret docker-registry ghcr-secret \
  --docker-server=ghcr.io \
  --docker-username=prabhas81 \
  --docker-password=<PAT> \
  --docker-email=prabhasthamatam@gmail.com
3. Apply Kubernetes Manifests
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yml
4. Verify Pods and Service
kubectl get pods
kubectl get svc
Pods Example Output:

sql
NAME                                READY   STATUS    RESTARTS   AGE
nextjs-deployment-f949ff888-6qktv   1/1     Running   0          31s
Service Example Output:

pgsql
NAME                  TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
nextjs-service        NodePort    10.96.0.100     <none>        3000:30000/TCP   30s
Accessing the App
Open your browser at http://<Node-IP>:<NodePort>

With Minikube, you can run:

minikube service nextjs-service
Git Workflow
Initialize and commit:

git add .
git commit -m "Initial commit: Next.js app with Minikube Kubernetes deployment"
Push to remote: git push -u origin master

Project Structure

nextjs-devops-assessment/
├── k8s/
│   ├── deployment.yaml      # Kubernetes Deployment
│   └── service.yml          # Kubernetes Service
├── app/                     # Next.js application
├── Dockerfile               # Docker multi-stage build
├── package.json
└── README.md


Next.js App in Browser:
<img width="1348" height="641" alt="image" src="https://github.com/user-attachments/assets/fbc4a8b5-3546-459e-8509-087a04402970" />

