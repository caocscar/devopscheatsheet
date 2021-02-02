# devopscheatsheet

## Docker
- `sudo docker build -t api-test .`
- `sudo docker run -dp 5000:5000 --name ac api-test`
- `sudo docker ps`
- `sudo docker stop ac`
- `sudo docker rm -f ac`
- `sudo docker images`
- `sudo docker system df`

## Kubectl (controls the Kubernetes cluster manager)
command|description
---|---
kubectl get po | get description of pods
kubectl describe po sandbox-dashboard-api | describe pod

## Helm (Kubernetes package manager)
command|description
---|---
helm list |list releases of charts with chart and app version

## Kubernetes Redeployment
- update docker container with  
`docker build -t code.maymobility.com:5050/devops/artifactory-stage/dashboard-api:<your-tag> .`
- update `Chart.yaml` (Helm chart for Kubernetes) with updated chart version and app version
- update `sandbox.values.yaml` with latest image `tag`
- upgrade (redeploy) Helm chart with  
`helm upgrade sandbox-dashboard-api ci/chart/dashboard-api --values ci/chart/dashboard-api/sandbox.values.yaml`
