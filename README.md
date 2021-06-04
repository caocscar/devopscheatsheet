# devopscheatsheet
## AWS CLI
command|description
---|---
`aws s3 cp <S3:URI>`| Copying an S3 object to a local file

Reference: https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html

## Docker
command|description
---|---
`sudo docker build -t <image_name> .` | build an image
`sudo docker run -dp 5000:5000 --name <container_name> <image_name` | run detached `-d` specifying container name
`sudo docker ps` | list running processes
`sudo docker ps -a` | list all (running and stopped) processes
`sudo docker stop <container_name>` | stop container
`sudo docker rm -f <container_name>` | stop and remove container
`sudo docker images`
`sudo docker system df`
`sudo docker exec -ti <container_name> /bin/bash` | open a shell in the container
`sudo docker exec -ti --env-file .env -p 5000:5000 --name <container_name> <image_name>` | use .env to specify environmental variables
`sudo docker logs <container_name>` | show log output from container

## Kubectl (controls the Kubernetes cluster manager)
command|description
---|---
`kubectl get po` | get description of pods
`kubectl describe po sandbox-dashboard-api` | describe pod
`kubectl config get-contexts` | list current and other contexts
`kubectl config use-context <namespace>` | switched to context \<namespace>
`kubectl get certificates` | list TLS certificate status and age
`aws eks update-kubeconfig --name=may-japan-root-cluster --profile=maymobility --region=ap-northeast-1` | update kubeconfig file

## Helm (Kubernetes package manager)
command|description
---|---
`helm list` | list releases of charts with chart and app version
`helm upgrade` | upgrade helm chart
`helm upgrade --install` | upgrade or install helm chart
`helm uninstall` | delete helm deployment
`helm status <NAME>` | display status of deployment

## Kubernetes Redeployment
- update docker container with  
`docker build -t code.maymobility.com:5050/devops/artifactory-stage/dashboard-api:<your-tag> .`
- update `Chart.yaml` (Helm chart for Kubernetes) with updated chart version and app version
- update `*.values.yaml` with latest image `tag` and required environmental variables
- upgrade (redeploy) Helm chart with  
`helm upgrade <NAME> ci/chart/dashboard-api --values ci/chart/dashboard-api/*.values.yaml`
- upgrade or install (deploy) Helm chart with  
`helm upgrade --install <NAME> ci/chart/dashboard-api --values ci/chart/dashboard-api/*.values.yaml`
