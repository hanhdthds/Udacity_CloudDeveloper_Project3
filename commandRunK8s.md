# Run front end
export NODE_OPTIONS=--openssl-legacy-provider

aws eks create-cluster --region us-east-1 --name kubeprj3 --role-arn arn:aws:iam::298760192985:role/EKSClusterRole --resources-vpc-config subnetIds=subnet-0e54f2ef99de44af3,subnet-076698d2ade266820,subnet-0a4e2ab677183195d,subnet-01fe8a8a56510a0e3

aws eks describe-cluster --region us-east-1 --name kubeprj3 --query "cluster.status"

aws eks update-kubeconfig --region us-east-1 --name kubeprj3

# API deploy 

# Create or apply environment variable files 
kubectl apply -f ./configurations/aws-secret.yaml
kubectl apply -f ./configurations/env-secret.yaml
kubectl apply -f ./configurations/env-configmap.yaml

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

## add env
kubectl set env deployment backend-feed POSTGRES_USERNAME="honghanh" POSTGRES_PASSWORD="honghanh" 
kubectl set env deployment backend-user POSTGRES_USERNAME="honghanh" POSTGRES_PASSWORD="honghanh" 

## Check the deployment names and their pod status
kubectl get deployments
## Create a Service object that exposes the frontend deployment
## The command below will ceates an external load balancer and assigns a fixed, external IP to the Service.
kubectl expose deployment frontend --type=LoadBalancer --name=publicfrontend
kubectl expose deployment reverseproxy --type=LoadBalancer --name=publicreverseproxy
## Repeat the process for the *reverseproxy* deployment. 
## Check name, ClusterIP, and External IP of all deployments
kubectl get services 
kubectl get pods # It should show the STATUS as Running


Kubernetes kubectl get pods output
Kubernetes kubectl describe services output
Kubernetes kubectl describe hpa output
Kubernetes kubectl logs <your pod name> output

kubectl get pod backend-user-6d5f7f994-txb86 --output=yaml
kubectl get pod backend-feed-64c979f764-ccpzf --output=yaml

kubectl logs backend-feed-64c979f764-ccpzf
kubectl logs backend-user-6d5f7f994-txb86

kubectl describe pod backend-feed-64c979f764-ccpzf
kubectl describe pod backend-user-6d5f7f994-txb86

kubectl exec --stdin --tty backend-feed-64c979f764-ccpzf -- /bin/bash
kubectl exec --stdin --tty backend-user-6d5f7f994-txb86 -- /bin/bash

printenv | grep POST


# remove 
kubectl delete deployment reverseproxy
kubectl delete service reverseproxy
kubectl delete deployment backend-user

 kubectl delete svc backend-user 
 kubectl delete svc backend-feed 

## check end point
kubectl get ep -o wide


<!-- AWS_BUCKET="prj3-udacity" AWS_PROFILE="honghanh" AWS_REGION="us-east-1" POSTGRES_DB="postgres" POSTGRES_HOST="cdr.cggmn5gd5quo.us-east-1.rds.amazonaws.com" -->

kubectl port-forward backend-user-76dcc5944f-mjgbn 8080:8080

curl http://backend-feed:8080/api/v0/feed
curl http://reverseproxy:8080/api/v0/feed

curl http://reverseproxy:8080/api/v0/users/

kubectl rollout restart deployment reverseproxy
