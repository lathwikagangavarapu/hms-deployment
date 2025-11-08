# Hospital Management System - Fullstack Application

This is a cloud-native Hospital Management System deployed on Kubernetes, enabling hospitals to manage patient records, doctor schedules, billing, and reports effectively.

## Architecture

- **Frontend**: React application served by Nginx
- **Backend**: Spring Boot REST API
- **Database**: MySQL with persistent storage
- **Orchestration**: Kubernetes with Ingress for external access

## Prerequisites

- Docker and Docker Compose (for local testing)
- Kubernetes cluster (Minikube, EKS, GKE, etc.)
- kubectl configured to access your cluster

## Local Development with Docker Compose

1. Clone the repository
2. Run `docker-compose up --build -d`
3. Access the application at:
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:8089

## Kubernetes Deployment

### Build and Push Docker Images

```bash
# Build backend image
cd backend
docker build -t bookmydoctor-backend:latest .
docker tag bookmydoctor-backend:latest your-registry/bookmydoctor-backend:latest
docker push your-registry/bookmydoctor-backend:latest

# Build frontend image
cd ../frontend
docker build -t bookmydoctor-frontend:latest .
docker tag bookmydoctor-frontend:latest your-registry/bookmydoctor-frontend:latest
docker push your-registry/bookmydoctor-frontend:latest
```

### Deploy to Kubernetes

```bash
# Apply all Kubernetes manifests
kubectl apply -f k8s/

# Check deployment status
kubectl get pods
kubectl get services
kubectl get ingress
```

### Access the Application

- Update the Ingress host in `k8s/ingress.yaml` to match your domain
- Add the host entry to your DNS or local hosts file
- Access the application at the configured domain

## Checking Application Status

### Docker Compose (Local Development)

```bash
# Check container status
docker-compose ps

# View logs
docker-compose logs

# Check specific service logs
docker-compose logs backend
docker-compose logs frontend
docker-compose logs mysql

# Test backend API
curl http://localhost:8089/actuator/health

# Test frontend
curl http://localhost:3000
```

### Kubernetes Deployment

```bash
# Check pod status
kubectl get pods

# Check pod logs
kubectl logs -l app=backend
kubectl logs -l app=frontend
kubectl logs -l app=mysql

# Check services
kubectl get services

# Check ingress
kubectl get ingress

# Port forward for testing (if needed)
kubectl port-forward svc/backend-service 8089:8089
kubectl port-forward svc/frontend-service 3000:80
```

## Testing Login Functionality

1. **Access the application** at the configured URL
2. **Check browser console** for any JavaScript errors
3. **Test API endpoints**:
   ```bash
   # Health check
   curl http://localhost:8089/actuator/health

   # Test authentication endpoint (if available)
   curl -X POST http://localhost:8089/api/auth/login \
     -H "Content-Type: application/json" \
     -d '{"username":"test","password":"test"}'
   ```
4. **Database connectivity**:
   ```bash
   # Connect to MySQL container
   docker-compose exec mysql mysql -u root -p bookmydoctordb
   # Password: admin
   ```

## Features

- Patient record management
- Doctor schedule management
- Billing system
- Reporting capabilities
- User authentication

## Scalability

- Backend and frontend deployments are configured with 2 replicas
- Resource limits and requests are set for efficient resource utilization
- Horizontal Pod Autoscaling can be added for dynamic scaling

## High Availability

- Persistent volume for MySQL data
- Health checks (liveness and readiness probes)
- Service discovery through Kubernetes services