# 🕹️ Break Games Vending Machine (BGVM)
 
> **Status:** Completed ✅  
> **Repository:** [sparki-json/break-games-vm](https://github.com/sparki-json/break-games-vm)  
> **Inspiration:** [DEF CON 31 S.O.D.A. Machine](https://www.youtube.com/watch?v=pmW6lMCEaJc)
 
## 📖 Overview
 
**Break Games Vending Machine** is a cloud-native application designed to dispense ephemeral game environments on demand. Inspired by the "Shell On Demand Appliance" from DEF CON 31, this project serves as a comprehensive playground for testing modern DevOps methodologies.
 
Instead of dispensing physical sodas, this vending machine provisions isolated **Kubernetes Pods** running specific game instances (e.g., *Rock, Paper, Scissors, Lizard, Spock*) accessible via a web interface.
 
## 🏗️ Architecture
 
The system follows a microservices architecture hosted on AWS, orchestrated by Kubernetes, and managed via Infrastructure as Code (IaC).
 
 
 
### Core Components
1.  **Frontend (Nginx):** A lightweight web interface serving the "Vending Machine" UI. It handles user requests and routes them to the backend.
2.  **Backend (Python Flask):** The control logic. It interacts with the Kubernetes API to dynamically spin up new pods for users and manages their lifecycle (Time-to-Live).
3.  **Game Pods:** Ephemeral containers that run the actual game logic. These are created on-the-fly and destroyed automatically after a set duration.
 
## 🛠️ Tech Stack
 
| Domain | Technology | Usage |
| :--- | :--- | :--- |
| **Infrastructure** | ![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=flat-square&logo=terraform&logoColor=white) | Provisioning AWS VPC, Subnets, Security Groups, and EC2 instances. |
| **Cloud Provider** | ![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazon-aws&logoColor=white) | Hosting the K3s cluster (EC2 T2.micro). |
| **Orchestration** | ![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white) | Managing container lifecycle, networking, and scaling. |
| **Containerization** | ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white) | Containerizing the Frontend (Nginx) and Backend (Python). |
| **CI/CD** | ![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=github-actions&logoColor=white) | Automated pipelines for building images, scanning security, and deploying to AWS. |
| **Backend** | ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) | Flask API for handling "vend" requests and K8s API interaction. |
 
## 🚀 Key Features
 
* **Infrastructure as Code:** Complete environment teardown and setup using Terraform.
* **Ephemeral Environments:** Game pods are temporary. The system automatically cleans up resources using Kubernetes TTL controllers or sidecar expiry logic.
* **Automated Deployment:** A "Cost Saver" CI/CD pipeline that starts the AWS instance, deploys the latest code, tests the endpoints, and stops the instance to stay within the AWS Free Tier.
* **Security Scanning:** Integrated **Trivy** image scanning in the CI pipeline to detect vulnerabilities before deployment.
 
## 📂 Project Structure
 
```text
├── .github/workflows   # CI/CD pipelines (Build, Push, Deploy)
├── terraform/          # IaC definitions (main.tf, variables.tf)
├── vending-machine/    # Source code for the main Vending Machine app
│   ├── backend/        # Flask API
│   └── frontend/       # Nginx + HTML/JS
├── rpslk-game/         # Source code for the "Rock Paper Scissors..." game
├── build-images        # Bash script to build images asynchronously and load them into minikube env
└── kube-vm.yaml        # Kubernetes manifests (Deployments, Services, Roles)
```

## 👣 Getting Started
Prerequisites
* AWS Account (Free Tier eligible)
* Terraform installed locally
* kubectl configured
### 1. Provision Infrastructure
```
cd terraform
terraform init
terraform apply
```
**Output:** Instance IP and Security Group details
 
### 2. Deploy Application
The application is deployed automatically via GitHub Actions upon push to main. To deploy manually:

**SSH into the node (if K3s)**

`ssh ubuntu@<EC2_PUBLIC_IP>`
 
### 3. Apply manifests

`kubectl apply -f kube-vm.yaml`
 
## 💡 Lessons Learned
* Cost Management: Automating the start/stop of EC2 instances via GitHub Actions is a viable strategy for essentially "free" dev environments.
* K8s RBAC: Giving pods permission to spawn other pods requires careful RoleBinding configuration.
* Image Optimization: Moving to distroless images significantly reduced the attack surface but required creative solutions (like Init Containers) for debugging or file injection.

*Maintained by [@sparki-json](https://github.com/sparki-json)*
