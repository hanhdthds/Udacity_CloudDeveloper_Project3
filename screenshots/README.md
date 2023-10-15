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
![Alt text](image.png)
* To verify Kubernetes services are properly set up
```bash
kubectl describe services
```
![Alt text](image-1.png)
![Alt text](image-2.png)
* To verify that you have horizontal scaling set against CPU usage
```bash
kubectl describe hpa
```
![Alt text](image-4.png)
* To verify that you have set up logging with a backend application
```bash
kubectl logs {pod_name}
```
![Alt text](image-3.png)
