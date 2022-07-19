# devopscheatsheet
## AWS CLI
command|description
---|---
`aws s3 cp <S3:URI>`| Copying an S3 object to a local file

Reference: https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html

## Docker
command|description
---|---
`docker build -t <image_name> .` | build an image
`docker run -dp 5000:5000 --name <container_name> <image_name` | run detached `-d` specifying container name
`docker ps` | list running processes
`docker ps -a` | list all (running and stopped) processes
`docker stop <container_name>` | stop container
`docker rm -f <container_name>` | stop and remove container
`docker images` | list images
`docker image rm` | remove one or more images
`docker image prune` | remove unused images
`docker system df` | 
`docker exec -ti <container_name> /bin/bash` | open a shell in the container
`docker exec -ti --env-file .env -p 5000:5000 --name <container_name> <image_name>` | use `.env` file to specify environmental variables
`docker logs <container_name>` | show log output from container
`sudo usermod -a -G docker <user>` | remove sudo requirement for user

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
`helm history <NAME>` | history of deployment
`helm rollback <NAME> <REVISION>` | rollback to specified revision (defaults to previous revision)
`helm dependency update` | ask Griffin
`helm get values <NAME>` | get the values files for the named release (extends to `all` or `hooks` or `manifest` or `notes`)

## Kubernetes Redeployment
- update docker container registry manually with  
`docker build -t code.maymobility.com:5050/devops/artifactory-stage/dashboard-api:<your-tag> .`
- update `Chart.yaml` (Helm chart for Kubernetes) with updated chart version and app version
- update `*.values.yaml` with latest image `tag` and required environmental variables
- upgrade (redeploy) Helm chart with  
`helm upgrade <NAME> ci/chart/dashboard-api --values ci/chart/dashboard-api/*.values.yaml`
- upgrade or install (deploy) Helm chart with  
`helm upgrade --install <NAME> ci/chart/dashboard-api --values ci/chart/dashboard-api/*.values.yaml`
