# Next.js DevOps Assessment

This repository contains a **Next.js application** deployed using **Docker** and **Kubernetes (Minikube)**. The project demonstrates containerization, deployment to Kubernetes, and integration with GitHub Container Registry (GHCR).

---

## Table of Contents

- [Project Setup](#project-setup)
- [Dockerization](#dockerization)
- [GitHub Container Registry (GHCR)](#github-container-registry-ghcr)
- [Kubernetes Deployment](#kubernetes-deployment)
- [Accessing the App](#accessing-the-app)
- [Git Workflow](#git-workflow)
- [Project Structure](#project-structure)
- [Notes](#notes)
- [References](#references)

---

## Project Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Prabhas81/wexa-ai.git
   cd wexa-ai
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Run the development server locally:**
   ```bash
   npm run dev
   ```
   The application runs at [http://localhost:3000](http://localhost:3000).

---

## Dockerization

1. **Build the Docker image:**
   ```bash
   docker build -t ghcr.io/prabhas81/wexa-ai:latest .
   ```

2. **Run the container locally:**
   ```bash
   docker run --rm -p 3000:3000 ghcr.io/prabhas81/wexa-ai:latest
   ```
   The application will be available at [http://localhost:3000](http://localhost:3000).

**Terminal Output Example:**
```
> nextjs-devops-assessment@0.1.0 start
> next start

   ▲ Next.js 15.5.4
   - Local:        http://localhost:3000
   - Network:      http://172.17.0.2:3000

 ✓ Starting...
 ✓ Ready in 1475ms
```

---

## GitHub Container Registry (GHCR)

1. **Create a Personal Access Token (PAT) in GitHub** with the following scopes:
   - `write:packages`
   - `read:packages`
   - `repo`

2. **Log in to GHCR:**
   ```bash
   echo <PAT> | docker login ghcr.io -u prabhas81 --password-stdin
   ```

3. **Push the Docker image:**
   ```bash
   docker push ghcr.io/prabhas81/wexa-ai:latest
   ```

**Push Output Example:**
```
latest: digest: sha256:b8df60aabac20e6c73f7196554ac4ab17afde9022ece2585d47bdcd27b4002e0 size: 856
```

---

## Kubernetes Deployment

1. **Create Docker Registry Secret:**
   ```bash
   kubectl create secret docker-registry ghcr-secret \
     --docker-server=ghcr.io \
     --docker-username=prabhas81 \
     --docker-password=<PAT> \
     --docker-email=prabhasthamatam@gmail.com
   ```

2. **Apply Kubernetes Manifests:**
   ```bash
   kubectl apply -f k8s/deployment.yaml
   kubectl apply -f k8s/service.yml
   ```

3. **Verify Pods and Services:**
   ```bash
   kubectl get pods
   kubectl get svc
   ```

**Pods Example Output:**
```
NAME                                READY   STATUS    RESTARTS   AGE
nextjs-deployment-f949ff888-6qktv   1/1     Running   0          31s
```

**Service Example Output:**
```
NAME                  TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
nextjs-service        NodePort    10.96.0.100     <none>        3000:30000/TCP   30s
```

---

## Accessing the App

- Open your browser at `http://<Node-IP>:<NodePort>`.
- With Minikube, you can run:
  ```bash
  minikube service nextjs-service
  ```

---

## Git Workflow

1. **Stage and commit changes:**
   ```bash
   git add .
   git commit -m "Initial commit: Next.js app with Minikube Kubernetes deployment"
   ```

2. **Push to remote:**
   ```bash
   git push -u origin master
   ```

3. **If non-fast-forward error occurs, rebase and resolve conflicts:**
   ```bash
   git pull origin master --rebase
   # resolve conflicts if any
   git add <resolved_files>
   git rebase --continue
   git push -u origin master
   ```

---

## Project Structure

```
nextjs-devops-assessment/
├── k8s/
│   ├── deployment.yaml      # Kubernetes Deployment
│   └── service.yml         # Kubernetes Service
├── app/                    # Next.js application
├── Dockerfile              # Docker multi-stage build
├── package.json
└── README.md
```

---

## Notes

- Deployment uses `replicas: 2` for high availability.
- `imagePullSecrets` allows Kubernetes to pull images from private GHCR.
- Readiness and liveness probes ensure pod health.
- This setup is fully reproducible for local Minikube clusters and cloud-based Kubernetes environments.

---

screenshots:
<img width="1345" height="638" alt="image" src="https://github.com/user-attachments/assets/2eba30b3-0682-494c-8741-091a1d268af5" />
