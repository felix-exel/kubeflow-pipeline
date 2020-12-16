# Kubeflow Pipeline along with MLflow Tracking on a time series forecasting example
Example lightweight Kubeflow Pipeline along with MLflow Tracking to train a time series forecasting model with TensorFlow 2.<br>
Related Blog Post: [https://www.novatec-gmbh.de/blog/ml-pipeline-mit-kubeflow-und-mlflow/](https://www.novatec-gmbh.de/blog/ml-pipeline-mit-kubeflow-und-mlflow/)
## Requirements
- [Kubeflow Installation](https://www.kubeflow.org/docs/started/getting-started/)
- Deployment of MLflow inside Kubernetes Cluster (see next Chapter)
- Create an AWS S3 Bucket as MLflow Artifact-Store to save ML Models
## MLflow Deployment
#### Prerequisites
1. Build the Dockerimage for the MLflow Trackingserver:<br> ```docker build . -t mlflow/server```
2. Create the namespace mlflow:<br> ```kubectl create namespace mlflow```
3. Create 3 Secrets for the user data of MySQL backend and AWS Credentials in namespaces mlflow and the created one in kubeflow (default is anonymous):<br> 
- ```kubectl create secret generic mysql-secret --from-literal=MYSQL_DATABASE=mlflow --from-literal=MYSQL_USER=mlflow --from-literal=MYSQL_PASSWORD=mlflow --from-literal=MYSQL_ROOT_PASSWORD=mlflow -n mlflow```<br>
- ```kubectl create secret generic aws-secret --from-literal=AWS_ACCESS_KEY_ID=<your key id> --from-literal=AWS_SECRET_ACCESS_KEY=<your secret key> -n mlflow```<br>
- ```kubectl create secret generic aws-secret --from-literal=AWS_ACCESS_KEY_ID=<your key id> --from-literal=AWS_SECRET_ACCESS_KEY=<your secret key> -n anonymous```
#### Deploy yaml Files
1. ```kubectl apply -f mysql-pvc.yaml``` <br>Check if the pvc is bound to a pv. If not change the StorageClass to your default Class.
2. ```kubectl apply -f mysql-deployment.yaml```
3. Change the bucket name in the mlflow-deployment.yaml file and then use:<br> ```kubectl apply -f mlflow-deployment.yaml``` 
