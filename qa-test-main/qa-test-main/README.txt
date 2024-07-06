# QA-Test

This project demonstrates deploying frontend and backend services to a local Kubernetes cluster, verifying their communication, and running automated integration tests.

## Prerequisites

1. **Windows 11 Configuration**:
   - Ensure Hyper-V is enabled:
     1. Open PowerShell as Administrator.
     2. Run the command: `bcdedit /set hypervisorlaunchtype auto`.
     3. Restart your computer.

2. **Docker Desktop**:
   - Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop).
   - Ensure Kubernetes is enabled in Docker Desktop:
     1. Open Docker Desktop.
     2. Go to Settings > Kubernetes.
     3. Check the box "Enable Kubernetes" and click "Apply & Restart".

3. **Node.js and npm**:
   - Download and install [Node.js](https://nodejs.org/).
   - Verify installation by running:
     ```sh
     node -v
     npm -v
     ```

4. **Python**:
   - Download and install [Python](https://www.python.org/).
   - Ensure `pip` (Python package installer) is installed.

5. **kubectl**:
   - Download and install [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

6. **Git**:
   - Download and install [Git](https://git-scm.com/).

## Clone the Repository

```sh
git clone https://github.com/Vengatesh-m/qa-test.git
cd qa-test
```

## Setting Up the Environment

### 1. Install Node.js Dependencies

Navigate to the frontend directory and install dependencies:
```sh
cd C:\Users\hp\Downloads\qa-test-main\qa-test-main\frontend
npm install
```

### 2. Build Docker Images

Build Docker images for both frontend and backend:
```sh
cd C:\Users\hp\Downloads\qa-test-main\qa-test-main
docker build -t frontend:latest -f frontend/Dockerfile .
docker build -t backend:latest -f backend/Dockerfile .
```

### 3. Deploy to Kubernetes

Ensure Docker Desktop's Kubernetes is running:
```sh
kubectl cluster-info
```

Deploy the services:
```sh
cd deployment
kubectl apply -f backend-deployment.yaml
kubectl apply -f frontend-deployment.yaml
```

### 4. Verify Deployment

Check the status of the pods:
```sh
kubectl get pods
```

Forward ports to access the services locally:
```sh
kubectl port-forward svc/frontend-service 8080:80
```

Access the frontend service at [http://localhost:8080](http://localhost:8080) and verify it displays the greeting message fetched from the backend. To access the backend directly, visit [http://localhost:3000/greet](http://localhost:3000/greet).

### Ensure Port Availability

To check if a port (e.g., 8080) is free, run the following command in the command prompt:
```sh
netstat -ano | findstr :8080
```

If the output shows:
```
TCP    0.0.0.0:8080           0.0.0.0:0              LISTENING       3056
TCP    [::]:8080              [::]:0                 LISTENING       3056
```
It means the port is in use. To free the port, run:
```sh
taskkill /PID 3056 /F
```

### Starting and Stopping Servers

To start the backend server:
```sh
cd C:\Users\hp\Downloads\qa-test-main\qa-test-main\backend
npm start
```

To stop the backend server:
```sh
cd C:\Users\hp\Downloads\qa-test-main\qa-test-main\backend
npm run stop
```

To start the frontend server:
```sh
cd C:\Users\hp\Downloads\qa-test-main\qa-test-main\frontend
npm start
```

To stop the frontend server:
```sh
cd C:\Users\hp\Downloads\qa-test-main\qa-test-main\frontend
npm run stop
```

## Automated Testing

### Test Script (Python)

Create a Python script to verify the integration between frontend and backend:

```python
# test_integration.py
import requests

def test_frontend_backend_integration():
    response = requests.get("http://localhost:8080")
    assert response.status_code == 200
    assert "Nikky Parley!" in response.text

if __name__ == "__main__":
    test_frontend_backend_integration()
    print("Integration test passed.")
```

### Run the Test

Ensure the frontend service is running and accessible at `http://localhost:8080`, then execute the test script:

```sh
pip install requests
python test_integration.py
```

## Deliverables

- **Test Script**: The test script (`test_integration.py`) checks the frontend-backend integration.
- **README**: This file contains detailed setup and execution instructions.

## Notes

- Ensure Docker Desktop is running and Kubernetes is enabled before starting the deployment.
- Use the provided `Dockerfile` and Kubernetes deployment files located in the `deployment` folder.
- If any issues arise, consult the logs using `kubectl logs <pod-name>` to troubleshoot.
