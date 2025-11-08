# Deployment on Kubernetes Example (backend + frontend + Mongo)

 Visual overview : https://excalidraw.com/#json=LmUZaGPrfEtqP6fgNM5Lr,_QmZmJ1fXIGiNRSRsCRG0A
 
Minimal instructions to run this project locally (Docker) or on Kubernetes (minikube).

Prerequisites
- Docker
- kubectl
- (optional) minikube or another Kubernetes cluster

Project layout
- backend/: Flask backend (container port 8000)
- frontend/: Flask frontend (container port 9000)
- k8s/: Kubernetes manifests (mongo, mongo-express, backend, frontend)

Run with minikube (recommended for k8s)
1. Start minikube:
   minikube start

2. Build images inside minikube (avoid pushing):
   eval $(minikube -p minikube docker-env)
   docker build -t kube_backend ./backend
   docker build -t kube_frontend ./frontend
   eval $(minikube -p minikube docker-env -u)

3. Deploy manifests:
   kubectl apply -f k8s/mongo.yaml
   kubectl apply -f k8s/mongo-express.yaml
   kubectl apply -f k8s/backend.yaml
   kubectl apply -f k8s/frontend.yaml

4. Verify pods and services:
   kubectl get pods
   kubectl get svc

5. Port-forward for local access:
   # Frontend: access at http://localhost:9000
   kubectl port-forward service/kube-frontend 9000:80

   # Backend: access at http://localhost:8000
   kubectl port-forward service/kube-backend 8000:80


   # Mongo Express (web UI): http://localhost:8081
   kubectl port-forward service/mongo-express 8081:80

Run locally with Docker (no Kubernetes)
1. Create a Docker network:
   docker network create kube-net

2. Start Mongo:
   docker run -d --name mongo --network kube-net mongo

3. Build and run backend:
   docker build -t kube_backend ./backend
   docker run -d --name backend --network kube-net -e MONGO_HOST=mongo -p 8000:8000 kube_backend

4. Build and run frontend:
   docker build -t kube_frontend ./frontend
   docker run -d --name frontend --network kube-net -e BACKEND_URL=http://backend:8000 -p 9000:9000 kube_frontend

5. Open frontend:
   http://localhost:9000

Basic backend API
- GET /api/get           — list names
- GET /api/add/<name>    — add a name
- GET /api/delete/<name> — delete a name

Notes
- If you build images for k8s but don't use minikube's docker, push images to a registry and update k8s manifests.
- Use kubectl logs <pod-name> to inspect logs and kubectl describe pod <pod-name> for events.

That's it — start with Docker for quick local testing or minikube to run the full k8s manifests.
