This project details the process of deploying a highly available and scalable web application (a simple game)
using Amazon EKS (Elastic Kubernetes Service) with the AWS Fargate serverless computing engine.

🛠️ Execution Steps & Configuration
Before starting, ensure the following tools are installed and configured:

AWS CLI: Configure with your credentials (aws configure).

kubectl: Kubernetes command-line tool.

eksctl: EKS command-line utility.

Step 1: Cluster Provisioning (EKS Fargate)
A Fargate based cluster is created, which automatically handles the networking (public/private subnets) and places application pods in the private subnets.
# WARNING: This command incurs AWS charges. Delete resources after experimentation.
```
eksctl create cluster --name demo-cluster --region us-east-1 --fargate
```
Verification: Check the AWS Console to ensure the demo cluster is successfully created.

Step 2: Fargate Profile Creation
A Fargate profile is created to ensure applications deployed in the game-2048 namespace are managed by Fargate. refer App deploy ingress file i attached


Step 3: Application Deployment
The application is deployed using a Kubernetes manifest (YAML) that defines the deployment, service, and ingress resources. refer App deploy ingress file i attached 
This manifest (app-delpy-ingress.yaml) contains the Deployment, Service and Ingress definitions for the game application.
kubectl apply -f app-delpy-ingress.yaml


Step 4: Configure ALB Ingress Controller (IAM Integration)
Create IAM Policy/Role: Use the ALB controller addon file to create the necessary IAM policy and service account. This attaches the IAM role to the Kubernetes service account, allowing the pod to interact with AWS services.


Step 5: Deploy the ALB Ingress Controller
The ALB Ingress Controller is deployed using a Helm chart, configured to use the service account created in the previous step. check albcontroller file u can find inside

Step 6: Final Verification
Once the ALB Ingress Controller is running, it will read the Ingress resource created in Step 3 and provision an AWS Application Load Balancer Check the AWS EC2 console for the new Load Balancer.

Once the Load Balancer DNS is active, paste the URL into your browser to confirm the successful deployment and accessibility of the game application and congrats you did.

⚠️ IMPORTANT CAUTION (Cost Management)
This EKS cluster and its associated resources (Load Balancers, VPC components) will incur significant AWS charges. Remember to delete the cluster immediately after you finish experimentation.

~~~
eksctl delete cluster --name demo-cluster --region us-east-1
~~~
