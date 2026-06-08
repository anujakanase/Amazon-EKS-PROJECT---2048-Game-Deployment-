# 🚀 Deploying 2048 Game on Amazon EKS

## Project Overview

This project demonstrates the deployment of the popular 2048 game application on Amazon Elastic Kubernetes Service (EKS) using Kubernetes and AWS cloud-native services.

The goal of this project was to gain hands-on experience with Kubernetes orchestration, EKS cluster management, AWS networking, and load balancing.

---

## Architecture

User → AWS Application Load Balancer (ALB) → Kubernetes Ingress → Service → Pods (2048 Game)

---

## Technologies Used

* AWS EKS
* Kubernetes
* EKS Fargate
* AWS Load Balancer Controller
* IAM
* Helm
* kubectl
* eksctl

---

## Key Features

* EKS Cluster Provisioning
* Serverless Pod Execution using Fargate
* Kubernetes Deployments and Services
* Ingress-based Routing
* AWS Application Load Balancer Integration
* IAM Roles for Service Accounts (IRSA)

---

## Deployment Steps

### Create EKS Cluster

```bash
eksctl create cluster \
--name demo-cluster \
--region ap-south-1 \
--fargate
```

### Create Fargate Profile

```bash
eksctl create fargateprofile \
--cluster demo-cluster \
--region ap-south-1 \
--name alb-sample-app \
--namespace game-2048
```

### Install AWS Load Balancer Controller

```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json (Download IAM policy)

aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json  (Create IAM Policy)

eksctl create iamserviceaccount \
  --cluster=<your-cluster-name> \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::<your-aws-account-id>:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve  (Create IAM Role)

helm repo add eks https://aws.github.io/eks-charts  (Add helm repo)

helm repo update  (Update the repo)

helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system \
  --set clusterName=<your-cluster-name> \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=<your-region> \
  --set vpcId=<your-vpc-id>  (Install)
```

### Deploy Application

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```

### Verify Resources

```bash
kubectl get pods -n game-2048
kubectl get svc -n game-2048
kubectl get ingress -n game-2048
kubectl get deployment -n kube-system aws-load-balancer-controller
```

---

## Learning Outcomes

* Understanding Kubernetes Deployments and Services
* Working with Amazon EKS
* Configuring AWS Load Balancer Controller
* Implementing Ingress for external access
* Managing Kubernetes workloads on Fargate
* Troubleshooting Kubernetes networking and permissions

---

## Project Screenshots

<img width="1346" height="691" alt="1" src="https://github.com/user-attachments/assets/701a0284-d495-4f9e-8bad-e733d5d7935c" />
---
<img width="1344" height="399" alt="2" src="https://github.com/user-attachments/assets/34098809-fc4e-4f2b-96e2-8cee827de6b4" />
---
<img width="1016" height="68" alt="3" src="https://github.com/user-attachments/assets/63f17c51-d38c-4de6-a084-a1efffdd8503" />
---
<img width="1021" height="173" alt="4" src="https://github.com/user-attachments/assets/71e56874-de3f-410b-b467-503655b056b9" />
---
<img width="882" height="323" alt="5" src="https://github.com/user-attachments/assets/e828d394-6ed8-4180-9143-774e607a40d2" />
---
<img width="1348" height="540" alt="6" src="https://github.com/user-attachments/assets/20fb42f5-1d82-4673-b348-fb1e741d665f" />
---
<img width="798" height="460" alt="7" src="https://github.com/user-attachments/assets/31ad8e30-209a-408e-8567-9a75c54314a8" />
---
<img width="1027" height="184" alt="8" src="https://github.com/user-attachments/assets/b215cab7-3f8a-45d8-8496-2df6492282bd" />
---
<img width="1351" height="626" alt="9" src="https://github.com/user-attachments/assets/1d730b82-3155-4d21-834e-0a05c7790b09" />







