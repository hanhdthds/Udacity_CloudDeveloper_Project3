# Screenshots
To help review your infrastructure, please include the following screenshots in this directory::

## Deployment Pipeline
* DockerHub showing containers that you have pushed

    ![Alt text](DockerHub.png)
* Travis CI showing a successful build and deploy job
![Alt text](TravisCI_Pass_1.png)
![Alt text](TravisCI_Pass_2.png)
![Alt text](TravisCI_Pass_3.png)

## Kubernetes
* To verify Kubernetes pods are deployed properly
```bash
kubectl get pods
```
![Alt text](Get-deployment.png)
![Alt text](Get-pods.png)
* To verify Kubernetes services are properly set up
```bash
kubectl describe services
```
![Alt text](Descibe-service.png)
* To verify that you have horizontal scaling set against CPU usage
```bash
kubectl describe hpa
```
![Alt text](describe-hpa.png)
* To verify that you have set up logging with a backend application
```bash
kubectl logs {pod_name}
```
![Alt text](Log-frontend.png)
