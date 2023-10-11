# Run front end
export NODE_OPTIONS=--openssl-legacy-provider

aws eks create-cluster --region us-east-1 --name kubeprj3 --role-arn arn:aws:iam::670020652061:role/EKSClusterRole --resources-vpc-config subnetIds=subnet-0f4878051272ab1da,subnet-08c08b6f86bb59d50,subnet-0a620fca33bc682cb,subnet-0a02b1f23c300bb61,subnet-0f11be55bffb37187

aws eks describe-cluster --region us-east-1 --name kubeprj3 --query "cluster.status"

aws eks update-kubeconfig --region us-east-1 --name kubeprj3

# API deploy 

# Create or apply environment variable files 
kubectl apply -f ./configuarations/aws-secret.yaml
kubectl apply -f ./configuarations/env-secret.yaml
kubectl apply -f ./configuarations/env-configmap.yaml

# create or apply deployment files 
kubectl apply -f ./configurations/backend-feed-deployment.yaml
kubectl apply -f ./configurations/backend-user-deployment.yaml
kubectl apply -f ./configurations/frontend-deployment.yaml
kubectl apply -f ./configurations/reverseproxy-deployment.yaml

# create or apply deployment files 
kubectl apply -f ./configurations/backend-feed-service.yaml
kubectl apply -f ./configurations/backend-user-service.yaml
kubectl apply -f ./configurations/frontend-service.yaml
kubectl apply -f ./configurations/reverseproxy-service.yaml

## Check the deployment names and their pod status
kubectl get deployments
## Create a Service object that exposes the frontend deployment
## The command below will ceates an external load balancer and assigns a fixed, external IP to the Service.
kubectl expose deployment frontend --type=LoadBalancer --name=publicfrontend
kubectl expose deployment reverseproxy --type=LoadBalancer --name=reverseproxy
## Repeat the process for the *reverseproxy* deployment. 
## Check name, ClusterIP, and External IP of all deployments
kubectl get services 
kubectl get pods # It should show the STATUS as Running
