# Deploying the Robot Shop Application to Google Kubernetes Engine (GKE)

This document provides a step-by-step guide to deploying the Robot Shop microservices application to a Google Kubernetes Engine (GKE) cluster.

## Prerequisites

Before you begin, ensure you have the following tools installed and configured:

*   **Google Cloud SDK (`gcloud`):** Authenticated to your GCP account (`gcloud auth login`) and with your project set (`gcloud config set project YOUR_PROJECT_ID`).
*   **`kubectl`:** The Kubernetes command-line tool.
*   **Helm:** The package manager for Kubernetes.
*   **Docker:** The containerization platform.

## Deployment Steps

### 1. Create a GKE Cluster

First, create a GKE cluster in your GCP project. The following command will create a 3-node cluster in the `asia-south1-a` (Mumbai) region.

```bash
gcloud container clusters create robot-shop-gke-cluster --num-nodes=3 --zone=asia-south1-a --project YOUR_PROJECT_ID --service-account=YOUR_SERVICE_ACCOUNT_EMAIL
```

Replace `YOUR_PROJECT_ID` and `YOUR_SERVICE_ACCOUNT_EMAIL` with your GCP project ID and the service account you want to use for the cluster nodes.

### 2. Get Cluster Credentials

After the cluster is created, configure `kubectl` to connect to it by running the following command:

```bash
gcloud container clusters get-credentials robot-shop-gke-cluster --zone=asia-south1-a --project YOUR_PROJECT_ID
```

### 3. Deploy the Application with Helm

Navigate to the `GKE/helm` directory and use Helm to deploy the application. This command will create a new namespace called `robot-shop` and deploy all the necessary services.

```bash
cd GKE/helm
helm install robot-shop . -n robot-shop --create-namespace
```

## Verification

To verify the deployment, you can check the status of the pods and services.

### 1. Check Pod Status

Run the following command to ensure all pods are in the `Running` state:

```bash
kubectl get pods -n robot-shop
```

### 2. Check Service Status and External IP

Run the following command to get the status of the services. It may take a few minutes for the `web` service to be assigned an external IP address.

```bash
kubectl get services -n robot-shop
```

## Accessing the Application

Once the `web` service has an external IP address, you can access the application in your web browser at `http://<EXTERNAL_IP>:8080`.

## Cleanup

To avoid incurring charges, you can delete the resources you created.

### 1. Delete the Helm Release

```bash
helm uninstall robot-shop -n robot-shop
```

### 2. Delete the GKE Cluster

```bash
gcloud container clusters delete robot-shop-gke-cluster --zone=asia-south1-a --project YOUR_PROJECT_ID
```
