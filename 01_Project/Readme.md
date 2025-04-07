
### Jenkins #####
docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
Jenkins Login username - jenkinsadmin 
Jenkins Login password - jenkinsadmin
#default plugin install with git
Install plungin - Docker Pipeline ,Artifactory
Find the Artifactory section and click Add Artifactory Server.
Provide the necessary Artifactory URL, credentials, and repository details.

Jenkins login URL - http://localhost:8100/

Jenkins server need to installation

    2  apt-get update
    3  apt-get install -y docker.io
    4  docker --version

apt-get install -y curl
curl https://baltocdn.com/helm/signing.asc | apt-key add -
apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | tee /etc/apt/sources.list.d/helm-stable-debian.list
apt-get update
apt-get install -y helm
helm version --short

apt-get update && apt-get install -y iputils-ping
curl -I http://nexus:8081

apt-get update && apt-get install -y ca-certificates

Regenerate the GitHub token with correct scopes:

Go to https://github.com/settings/tokens

apt-get update && apt-get install -y dnsutils

create the Personal access tokens (classic) to push the code to repo




### Please Read the Jenkins files ######
#### artifactory #####
artifactory username - jenkins  
artifactory password - Jenkins@123#
Create the Artifactory repo - docker-repo


# jenkins github id  for credentials - github-credentials
# jenkins artifactory id  for credentials - jfrog-credentials
# nexus artifactory id  for credentials - nexus-credentials


### Nexus ####
Nexus Login URL - http://localhost:8082/
docker exec nexus cat /nexus-data/admin.password
default username - admin
new password -nexusadmin
# Repository Typ - Hosted / Store your own Docker images (Push & Pull)
 Nexus artifactory username - jenkins  
nexus artifactory password - Jenkins@123#

Create the Artifactory repo - docker-repo
Create the Artifactory repo - helm-repo

docker login -u admin -p <your-password> http://localhost:8083
docker login -u admin -p nexusadmin http://localhost:8083
docker login -u jenkins -p Jenkins@123# http://localhost:8083

http://localhost:8083/repository/docker-repo/
http://localhost:8082/repository/helm-repo/



### Helm chart 

helm create python-app
#helm install nginx-ingress ingress-nginx/ingress-nginx



helm install python-app ./python-app
helm upgrade python-app ./python-app
helm uninstall python-app
helm list --all

kubectl get ingress
kubectl describe ingress python-app-ingress
kubectl get svc
kubectl port-forward svc/python-app 5000:5000
curl http://localhost:5000/api



### NGINX Ingress Controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml

kubectl get pods -n ingress-nginx

kubectl logs -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx
kubectl describe pod -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx
kubectl get svc -n ingress-nginx


## Convert Ingress NGINX to NodePort
Since LoadBalancer doesn’t work properly in Docker Desktop, convert it to NodePort:
kubectl patch svc ingress-nginx-controller -n ingress-nginx -p '{"spec":{"type":"NodePort"}}'

kubectl get svc -n ingress-nginx
curl -v http://localhost:32266/api


## Helm Packing and Pushing to Nexus
cd python-app-hc/
helm package .
helm push python-app-0.1.0.tgz oci://localhost:8083/repository/helm-repo
curl -u admin:nexusadmin --upload-file python-app-0.1.1.tgz http://localhost:8082/repository/helm-repo


working (helm chart format only working and compose need bridge network)
curl -u admin:nexusadmin --upload-file mychart-0.1.0.tgz http://nexus:8081/repository/helm-repo/


## ------------ this for helm push plugin ######
helm plugin install https://github.com/chartmuseum/helm-push.git
helm repo add helm-repo http://localhost:8082/repository/helm-repo
helm push python-app-0.1.1.tgz helm-repo
---------------------------------------

#### argocd ###
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
