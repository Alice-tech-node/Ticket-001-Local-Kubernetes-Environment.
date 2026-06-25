## DevOps Ticket #001 – Project "Clean local Kubernetes environment"

### Project Overview

Ticket ID: #001
Priority: High

**Goal:**
Set up a clean local Kubernetes environment, deploy a multi-tier application, and automate deployments using GitOps with Argo CD.

**Problem Statement**

The development team was experiencing several issues with their local Kubernetes environments:

*Different team members were using different configurations and versions.*
*Cluster settings were becoming inconsistent over time.*
*Deployments were done manually using kubectl apply, making it difficult to track changes.*
*There was no clear deployment history or automated synchronization process.*

As a result, applications behaved differently across environments, making troubleshooting difficult.

**Root Cause Analysis**

After reviewing the environment, two major problems were identified:

**1. Incorrect Git Repository Configuration**

Argo CD could not synchronize the application because the repository URL and path configuration did not match the actual GitHub repository structure.

Solution:
- *Verified the repository URL.*
- *Corrected the repository path in the Argo CD Application manifest.*

**2. Invalid Container Images**

Application pods failed to start and entered an ImagePullBackOff state because the deployment files referenced placeholder images that did not exist.

Solution:
- *Replaced placeholder image names with valid container images.*
- *Updated deployment manifests and pushed changes to GitHub.*
- *Implementation Steps*

## **Step 1: Create the Kubernetes Environment**

A fresh Kubernetes cluster was created using Minikube to provide a clean and consistent development environment.
<img width="733" height="397" alt="minikube" src="https://github.com/user-attachments/assets/34455cd5-6808-45c3-a835-6a65cd1ea62d" />

## **Step 2: Install Argo CD**

Argo CD was deployed into the Kubernetes cluster to automate application deployment and synchronization.

The repository configuration was updated as shown below:
<img width="758" height="127" alt="Repoconfig" src="https://github.com/user-attachments/assets/bd9542b1-1f6a-47fb-9c60-992d82b82ff5" />

    
## **Step 3: Update Application Manifests**

Deployment files for the frontend, backend, and database were updated with valid container images.

Changes were committed and pushed to GitHub:

<img width="1246" height="342" alt="auto config" src="https://github.com/user-attachments/assets/070d5503-9e2a-4fd5-a953-15a4f95e0d9e" />

## **Step 4: Synchronize with Argo CD**

Argo CD was synchronized with the Git repository.

The synchronization process automatically:

- *Pulled the latest manifests from GitHub.*
- *Created Kubernetes resources.*
- *Configured networking between services.*
- *Exposed the frontend application through an Ingress resource.*
- *Kept the cluster state aligned with Git.*
  
  <img width="1061" height="165" alt="argosync" src="https://github.com/user-attachments/assets/aaa928fe-91d8-4d9d-b4f1-20e133ff3be6" />

## **Challenges Faced**

 - Repository URL Errors: A small formatting issue in the repository URL prevented Argo CD from locating the repository and synchronizing the application.
 - Image Pull Errors: Some deployment files referenced images that did not exist, causing pods to fail during startup until valid images were provided.

**Final Result**

The GitOps workflow was successfully implemented.

**Status**
 - Argo CD Sync Status: Synced ✅
 - Application Health: Healthy ✅
 - Cluster State: Fully aligned with GitHub repository ✅
<img width="1886" height="872" alt="Argo sync " src="https://github.com/user-attachments/assets/38c5e782-f517-409e-8024-13fe280033c1" />

**Solution Architecture**
<img width="1536" height="1024" alt="Soln Archt" src="https://github.com/user-attachments/assets/cb3388cf-b2f2-4872-b927-41e2c4c9fb7b" />


**Lessons Learned**
 - Git as the Single Source of Truth
 - Using GitOps ensures that all cluster configurations are stored in Git, making deployments consistent, traceable, and reproducible.
 - Validate Configurations Before Deployment

**Always verify:**
 - Repository URLs
 - Manifest paths
 - Container image names and tags

before deploying through an automated GitOps pipeline.

**Benefits of Argo CD**
 - Automated deployments
 - Continuous synchronization
 - Easy rollback capability
 - Reduced configuration drift
 - Improved visibility into application status

//**Technologies Used: Kubernetes, Minikube, Argo CD, GitHub, GitOps, PostgreSQL, Ingress Controller, YAML, Docker.**//
