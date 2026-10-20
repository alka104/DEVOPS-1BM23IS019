# DEVOPS-1BM23IS019
# DevOps Hands-On Exercise Series

This repository contains hands-on DevOps exercises covering Docker, Kubernetes, container networking, monitoring, Jenkins CI/CD pipelines, and container security.

## Technologies Used

- **Languages:** Python, Shell Scripting
- **Application Framework:** Flask
- **Containerization:** Docker, Docker Compose
- **Orchestration:** Kubernetes, Minikube, kubectl
- **CI/CD:** Jenkins, Git, GitHub
- **Monitoring:** Prometheus, Grafana
- **Networking and Security:** Docker Bridge Networks, MySQL, Redis, AppArmor
- **Configuration:** YAML, Dockerfile, Jenkinsfile

---

## Exercise 1: Kubernetes Fundamentals

**Objective:** Understand Kubernetes basics and set up a local Kubernetes environment using Minikube.

### Implementation
- Started a local Kubernetes cluster.
- Explored nodes, Pods, Services, and Kubernetes resources.
- Practiced basic cluster management and troubleshooting.

### Important Commands

```bash
minikube start
minikube status
kubectl cluster-info
kubectl get nodes
kubectl get pods -A
kubectl get services
minikube dashboard
minikube stop
```

### Key Learning
Understood how Minikube runs a local Kubernetes cluster and how kubectl is used to inspect and manage Kubernetes resources.

---

## Exercise 2: Deploy a Flask Application on Minikube

**Objective:** Containerize and deploy a Python Flask application using Kubernetes Deployment and Service resources.

### Implementation
- Created a Flask application returning `Hello from Flask on Kubernetes!` on port `15000`.
- Created a Dockerfile and built the application image.
- Configured a Kubernetes Deployment with one replica.
- Used `imagePullPolicy: Never` for the locally available image.
- Created a NodePort Service to expose the application.
- Verified Pods, deployment status, logs, and service accessibility.

### Important Commands

```bash
minikube start
minikube status
kubectl get nodes
minikube docker-env
docker build -t flask-app .
kubectl apply -f flask-deployment.yaml
kubectl get deployments
kubectl get pods -l app=flask-app
kubectl describe deployment flask-app
kubectl logs <pod-name>
kubectl get services
minikube service flask-app-service --url
```

Test the application using the URL returned by Minikube:

```bash
curl http://127.0.0.1:<service-port>
```

### Key Learning
Learned how to deploy a containerized application, inspect Kubernetes resources, and expose a Flask application through a Service.

---

## Exercise 3: Scaling a Flask Application Using ReplicaSets

**Objective:** Understand ReplicaSets, scaling, Pod distribution, and self-healing in Kubernetes.

### Implementation
- Developed a Flask-based e-commerce flash-sale application with `/`, `/buy`, and `/health` endpoints.
- Built a Docker image for the application.
- Created a ReplicaSet with three replicas and a ClusterIP Service.
- Scaled the ReplicaSet from three to five Pods.
- Deleted a Pod and observed Kubernetes automatically create a replacement.
- Inspected Pod IP addresses and node placement.

### Important Commands

```bash
minikube start --nodes=1
kubectl get nodes
kubectl apply -f flashsale-replicaset.yaml
kubectl get rs
kubectl get pods
kubectl get pods -o wide
```

Scale the ReplicaSet:

```bash
kubectl scale rs <replicaset-name> --replicas=5
kubectl get rs
kubectl get pods
```

Test self-healing:

```bash
kubectl delete pod <pod-name>
kubectl get pods
```

Inspect the ReplicaSet and application logs:

```bash
kubectl describe rs <replicaset-name>
kubectl logs <pod-name>
```

### Key Learning
Learned how ReplicaSets maintain the desired number of Pods, how applications can be scaled, and how Kubernetes replaces deleted Pods automatically.

---

## Exercise 4: Docker Networking

**Objective:** Understand container networking and communication between application services.

### Implementation
- Worked with a Flask application, MySQL, and Redis.
- Created and inspected a custom Docker bridge network.
- Explored communication between containers and connectivity to supporting services.
- Practiced inspecting containers and network configuration.

### Important Commands

```bash
docker network ls
docker network create <network-name>
docker network inspect <network-name>
docker ps
docker ps -a
docker network connect <network-name> <container-name>
docker logs <container-name>
docker exec -it <container-name> sh
```

For a Docker Compose setup:

```bash
docker compose up -d --build
docker compose ps
docker compose logs
docker compose down
```

### Key Learning
Learned how Docker bridge networks enable container-to-container communication and how applications connect to databases and caches.

---

## Exercise 5: Docker Security with AppArmor

**Objective:** Explore AppArmor profiles and Docker security options to restrict container processes.

### Implementation
- Prepared a Flask application for containerized execution.
- Worked on an AppArmor profile defining access restrictions.
- Explored applying a security profile to a Docker container.
- Prepared tests to investigate restricted actions and container security.

### Important Commands

Build and run the Flask container:

```bash
docker build -t flask-apparmor .
docker run -d --name flask-test -p 5000:5000 flask-apparmor
docker ps
docker logs flask-test
```

Check AppArmor status on a supported Linux host:

```bash
sudo aa-status
sudo apparmor_parser -r /etc/apparmor.d/my-apparmor-profile
```

Run a container with the custom profile after confirming it is loaded:

```bash
docker run --security-opt="apparmor=my-apparmor-profile" flask-apparmor
```

### Key Learning
Explored Linux AppArmor profiles, container security restrictions, and Docker security options.

**Note:** Verify that the custom profile was loaded and enforced before marking the security tests as completed. Docker Desktop on WSL2 may require a different Linux environment for custom AppArmor enforcement.

---

## Exercise 6: Real-Time Monitoring with Prometheus and Grafana

**Objective:** Monitor application metrics and visualize them using Prometheus and Grafana.

### Implementation
- Ran Prometheus and Grafana using Docker.
- Installed the Python `prometheus-client` package.
- Worked with application metrics and Prometheus collection.
- Created and saved a Grafana dashboard.

### Important Commands

```bash
docker compose up -d
docker ps
docker ps -a
pip install prometheus-client
```

Inspect service logs:

```bash
docker logs <prometheus-container>
docker logs <grafana-container>
```

Access the local interfaces:

- Grafana: http://localhost:3000
- Prometheus: http://localhost:9090

### Key Learning
Learned how Prometheus collects application metrics and how Grafana dashboards help visualize monitoring data.

---

## Exercise 7: Jenkins Hello World Job

**Objective:** Understand basic CI automation by executing a shell script through Jenkins.

### Implementation
- Created a `hello-world.sh` script.
- Stored the project in GitHub.
- Configured a Jenkins job to retrieve the repository.
- Executed the job and inspected the console output.

### Important Commands

Initialize and push a repository for the first time:

```bash
git init
git add .
git commit -m "Add Hello World script"
git branch -M main
git remote add origin <repository-url>
git push -u origin main
```

Push subsequent changes:

```bash
git add .
git commit -m "Update Hello World exercise"
git push origin main
```

Inspect Jenkins:

```bash
docker ps
docker logs <jenkins-container>
```

### Key Learning
Learned how Jenkins integrates with GitHub and executes automated jobs using version-controlled scripts.

---

## Exercise 8: Kubernetes Hands-On Deployment

**Objective:** Practice deploying and managing containerized applications using Kubernetes.

### Implementation
- Worked with Minikube and Kubernetes resources.
- Practiced applying YAML configuration files.
- Inspected and troubleshot deployments and Pods.
- Managed exercise files using Git and GitHub.

### Important Commands

```bash
minikube start
kubectl get nodes
kubectl apply -f <configuration-file>.yaml
kubectl get deployments
kubectl get pods
kubectl get services
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl delete -f <configuration-file>.yaml
```

### Key Learning
Practiced declarative resource management, deployment verification, and troubleshooting using Kubernetes command-line tools.

---

## Exercise 9: Jenkins Multi-Stage CI/CD Pipeline

**Objective:** Automate build, testing, deployment, and application verification for a Python Flask application using Jenkins.

### Implementation
- Created a Flask application with unit tests.
- Added `requirements.txt` and a `Jenkinsfile`.
- Configured Jenkins to retrieve the pipeline from GitHub using Pipeline Script from SCM.
- Executed unit tests using Python's `unittest` framework.
- Copied the application into a deployment directory and launched it.
- Tested the application and inspected Jenkins console output.
- Verified the pipeline completed successfully.

### Pipeline Stages

1. **Build:** Prepare the application environment and dependencies.
2. **Test:** Execute unit tests.
3. **Deploy:** Copy application files to the deployment directory.
4. **Run Application:** Launch the Flask application.
5. **Test Application:** Verify the application using the test script.

### Important Commands

Run the unit tests:

```bash
python3 -m unittest discover -s .
```

Run the application test:

```bash
python3 test_app.py
```

Create a deployment directory and copy the application:

```bash
mkdir -p python-app-deploy
cp app.py python-app-deploy/
```

Push the Jenkinsfile:

```bash
git add Jenkinsfile
git commit -m "Add Jenkins multi-stage pipeline"
git push origin main
```

Troubleshoot Jenkins:

```bash
docker ps -a
docker logs <jenkins-container>
docker exec -it -u root <jenkins-container> bash
```

### Result
The pipeline output showed successful unit tests and ended with `Finished: SUCCESS`.

### Key Learning
Learned how Jenkins automates multiple CI/CD stages, integrates with GitHub, executes tests, and performs a basic deployment workflow.

---

## Exercise 10: Multi-Node Kubernetes Deployment

**Objective:** Deploy and test multiple application services in a multi-node Kubernetes environment.

### Implementation
- Created a three-node Minikube cluster named `devops-multinode`.
- Worked with Product Catalog and Shopping Cart applications.
- Tested application endpoints and inspected responses.
- Verified Kubernetes resources and pushed exercise files to GitHub.

### Important Commands

Start the named cluster:

```bash
minikube start --profile=devops-multinode --nodes=3
kubectl --context=devops-multinode get nodes
```

Inspect Kubernetes resources:

```bash
kubectl --context=devops-multinode get deployments
kubectl --context=devops-multinode get pods -o wide
kubectl --context=devops-multinode get services
```

Inspect Pods and logs:

```bash
kubectl --context=devops-multinode logs <pod-name>
kubectl --context=devops-multinode describe pod <pod-name>
```

### Key Learning
Practiced managing Kubernetes resources in a multi-node cluster, deploying separate application services, inspecting Pod placement, and testing application endpoints.

---

## General Git Workflow

Run these commands from the repository root when saving exercise updates:

```bash
git status
git add .
git commit -m "Update DevOps exercises"
git push origin main
```

Check the current branch:

```bash
git branch --show-current
```

## Overall Learning Outcomes

- Containerized Python applications using Docker.
- Configured Docker networking and application dependencies.
- Deployed and scaled Kubernetes workloads.
- Practiced ReplicaSets, Pod recovery, and Service exposure.
- Monitored application metrics using Prometheus and Grafana.
- Automated testing and deployment workflows using Jenkins.
- Explored container security using AppArmor.
- Managed code and project updates using Git and GitHub.

