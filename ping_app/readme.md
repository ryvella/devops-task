# DevOps Task

## 1. Dockerize the Application

- Modified Python code to include dotenv for environment variable management.
- Created Dockerfile for the ping application to containerize it.
- Added requirements.txt and .env files for managing deps and environment variables.
- Created Docker Compose file to handle MongoDB database with the app.

### Build and Test

- Build - docker-compose -f compose.yml up
- Test Functionality - curl http://localhost:5000/ping


## 2. Deploy with Kubernetes

- All files related to the app are in folder kubernetes/ping_app
- The application uses mongodb. All files related to MongoDb are in folder kubernetes/mongodb. Since I don't have a k8 cluster in hand I followed this [guide.](https://antonputra.com/kubernetes/how-to-install-mongodb-on-kubernetes/)
- All commands that needs to be executed are in a text file 'commands.txt' in their respective folder. 
- In the deployment file, the image is considering an incremental build defined by date.buildoftheday
- Default deployment is set on 2 replicas with an HPA scaling up to 5 based on the CPU percentage. 

### Requirements for production

- Private Container registry (Azure / Docker Hub ect.)
- Kubernetes Cluster with recomended on prem requirements (if to be hosted on prem) [Specifications.](https://docs.kublr.com/installation/hardware-recommendation/#kublr-platform-deployment-example/)
- Key Vault or any other type of secret managment to remove sensitive keys from source control.

## 3. Setup Prometheus Monitoring

- All files related to monitoring are in kubernetes/prometheus

### Prometheus Configuration

- This is defined in prometheus.yml file. Using job ping-app for the application monitoring and node-exporter for system metrics.

### Deploy Prometheus within Kubernetes Cluster

- This is done via prometheusdeployment.yaml using the image prom/prometheus:latest. This also includes the service. 

### Visualize Metrics

- Only for the purpose of this task, deployed Grafana and exposed it. After logging in, datasource for prom should be added. 

## 4. Integrate with ELK Stack

<i> This is something I don't have much experience in as currently we use Azure App Insights and Datadog for monitoring. Also since I am not applying changes to an actual cluster I can't debug my configs to make sure it works properly. 

### Filebeat Configuration

- Added filebeat to ping_app/deployment.yaml

### Elasticsearch

- Deployed in kubernetes/elk elasticsearchDeployment.yaml

### Kibana

- Deployed in kubernetes/elk kibanaDeployment.yaml
- To visualize logs, I understood that from Kibana's managment section we would need to create a new index pattern and configure the pattern for filebeat. From there we can create dashboards ect. 

