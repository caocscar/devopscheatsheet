# devopscheatsheet

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
`docker ps -qa \|xargs -I % sh -c 'docker stop % && docker rm -v %'` | delete all stopped containers

## Kubectl (controls the Kubernetes cluster manager)
command|description
---|---
`kubectl get po` | get description of pods
`kubectl describe po <namespace>` | describe pod
`kubectl config get-contexts` | list current and other contexts
`kubectl config use-context <namespace>` | switched to context \<namespace>
`kubectl get certificates` | list TLS certificate status and age
`kubectl apply -f <yaml-file>` | apply yaml file
`kubectl delete ingress <pod>` | delete existing ingress 
`aws eks update-kubeconfig --name=<cluster-name> --profile=<profile> --region=<region>` | update kubeconfig file
`kubectl rollout restart deployment <deployment_name> -n <namespace>` | rollout restart of pods
`kubectl get namespaces` | list namespaces

## Helm (Kubernetes package manager)
command|description
---|---
`helm list` | list releases of charts with chart and app version
`helm list --all-namespaces` | list releases of charts with chart and app version with namespaces
`helm upgrade` | upgrade helm chart
`helm upgrade --install` | upgrade or install helm chart
`helm upgrade --install --create-namespace --namespace <NAMESPACE>` | create a new namespace when installing the 1st service in that namespace
`helm uninstall` | delete helm deployment
`helm status <NAME>` | display status of deployment
`helm history <NAME>` | history of deployment
`helm history <NAME> --namespace ridemayte` | history of deployment with namespaces
`helm rollback <NAME> <REVISION>` | rollback to specified revision (defaults to previous revision)
`helm dependency update` | ask Griffin
`helm get values <NAME>` | get the values files for the named release (extends to `all` or `hooks` or `manifest` or `notes`)

**Note**: `default` is default namespace

## Kubernetes Redeployment
- update docker container registry manually with  
`docker build -t code.maymobility.com:5050/<container-registry>:<your-tag> .`
- update `Chart.yaml` (Helm chart for Kubernetes) with updated chart version and app version
- update `*.values.yaml` with latest image `tag` and required environmental variables
- upgrade (redeploy) Helm chart with  
`helm upgrade <NAME> ci/chart/<repo> --values ci/chart/<repo>/*.values.yaml`
- upgrade or install (deploy) Helm chart with  
`helm upgrade --install <NAME> ci/chart/<repo> --values ci/chart/<repo>/*.values.yaml`
