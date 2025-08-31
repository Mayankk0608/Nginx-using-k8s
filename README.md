# Nginx-using-k8s

Nginx on Kubernetes Deployment
This repository contains a set of configuration files and a CI/CD pipeline for deploying an Nginx web server on a Kubernetes cluster. The project includes a Dockerfile to build a custom Nginx image, a docker-compose file for local testing, and various Kubernetes manifest files to deploy and manage the application.

**Project Structure**
1) .gitlab-ci.yml: A GitLab CI/CD pipeline definition with stages for Docker login, namespace creation, and deploying various Kubernetes resources.
2) Dockerfile: Defines how to build the custom Nginx Docker image. It's based on nginx:1.23.3-alpine and copies a custom configuration file.
3) docker-compose.yml: A simple Docker Compose file to build and run the Nginx container locally for development and testing.
4) index.htm: The static HTML file served by the Nginx web server. It contains a simple "Hello from Nginx" message.

**- Kubernetes Manifests:**

1) namespace.yml: Creates a dedicated nginx namespace for all the application resources.
2) Deployment.yml: Defines a Deployment to manage two replicas of the Nginx application, with resource requests and limits, and a persistent volume claim.
3) ReplicaSet.yml: A ReplicaSet to ensure a specified number of pods are always running. This is an alternative to the Deployment.
4) pod.yml: A basic Pod definition for a single Nginx instance.
5) Services.yml: Exposes the Nginx Deployment using a ClusterIP service.
6) Ingress.yml: Manages external access to the Nginx service, routing traffic based on path (/nginx).
7) PersistentVolume.yml: Defines a PersistentVolume with a hostPath for data storage.
8) PersistentVolumeClaim.yml: Claims the PersistentVolume for use by the Deployment.
9) Job.yml: A Kubernetes Job that runs a short-lived task (echo Hello world!!).
10) cronJob.yml: A Kubernetes CronJob that schedules a recurring backup task every minute.

**Getting Started**
**Prerequisites**

1) A Kubernetes cluster (e.g., Minikube, kind)
2) kubectl installed and configured to connect to your cluster
3)docker and docker-compose for local development

**Deployment Steps**

1) Build the Docker Image (Optional):
2) If you want to use your own image, first build it using the provided Dockerfile.
3) docker build -t mayank8765/nginx .
4) docker push mayank8765/nginx
5) Run the CI/CD Pipeline:
6) The .gitlab-ci.yml file is configured to automate the deployment process. Ensure your GitLab CI/CD variables $DockerHubUser and $DockerHubPass are set. The pipeline will:

7) Log into Docker Hub.
8) Create the nginx namespace.
9) Deploy the Nginx application using the Deployment.yml manifest.
10) Create the service, persistent volume, and persistent volume claim.

**Manual Deployment:**
You can also deploy the resources manually using kubectl.

# Create the namespace
kubectl apply -f namespace.yml

# Create the persistent volume and claim
kubectl apply -f PersistentVolume.yml
kubectl apply -f PersistentVolumeClaim.yml

# Deploy the Nginx application and service
kubectl apply -f Deployment.yml
kubectl apply -f Services.yml
kubectl apply -f Ingress.yml

# Run the Job and CronJob
kubectl apply -f Job.yml
kubectl apply -f cronJob.yml

Logs and Scan Results
The nginx.log file appears to contain security scan results, indicating a number of vulnerabilities in the Alpine-based Nginx image. The nginx_2.json and results.json files likely provide more detailed, machine-readable vulnerability data. It's recommended to review these reports and update the base image or patch the vulnerable packages to mitigate risks.
