my-app/
├── Chart.yaml
├── values.yaml
├── templates/
│   ├── server-deployment.yaml
│   ├── server-service.yaml
│   ├── client-deployment.yaml
│   ├── api-token-secret.yaml


helm install py-server-client ./my-app

curl -H "Authorization: Bearer super-secret-token" http://ping.local/ping
