# Devops-Project1DevOps Assignment: EKS + ArgoCD CI/CD Pipeline

This project demonstrates a complete CI/CD infrastructure pipeline using AWS EKS, Terraform, Kubernetes, and ArgoCD, with NGINX deployed via GitOps.

Infrastructure Provisioning (Terraform)
Navigate to the Terraform directory and apply the infrastructure:

cd terraform terraform init terraform apply This will:

Create a VPC with public/private subnets

Provision an EKS cluster with managed nodes

Update kubeconfig to access the cluster:

aws eks --region ap-south-1 update-kubeconfig --name demo-eks-cluster

Install ArgoCD on EKS
kubectl create namespace argocd kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

Expose ArgoCD server: kubectl port-forward svc/argocd-server -n argocd 8080:443

Login credentials: kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d URL: https://localhost:8080

Username: admin

Password: (from above)

Deploy NGINX via ArgoCD (GitOps) Apply the ArgoCD Application manifest:
kubectl apply -f argocd/nginx-app.yaml Ensure your GitHub repository:

Is public

Contains the /manifests directory with:

nginx-deployment.yaml

nginx-service.yaml

Access NGINX Web Server Check the NodePort assigned:
kubectl get svc nginx-service

Access NGINX via any EC2 node public IP and the NodePort: http://:30080

Project Folder Structure . ├── terraform/ │ ├── provider.tf │ ├── variables.tf │ ├── vpc.tf │ ├── eks.tf │ └── outputs.tf │ ├── manifests/ │ ├── nginx-deployment.yaml │ └── nginx-service.yaml │ ├── argocd/ │ └── nginx-app.yaml │ └── README.md
