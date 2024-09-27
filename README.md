# AccuKnox_QA_Trainee_Role_Problem-1

# Docker login command

    Log in to Docker Hub or your container registry
    docker_login = [
        docker login
        -u your-docker-username
        -p, your-docker-*********]

# Write Dockerfile:

    # Use an official Python runtime as a parent image
      FROM python:3.9-slim

    # Set the working directory in the container
      WORKDIR /app

    # Copy the current directory contents into the container at /app
      COPY . /app

    # Install any needed packages specified in requirements.txt
      RUN pip install --no-cache-dir -r requirements.txt

    # Make port 80 available to the world outside this container
      EXPOSE 80

    # Define environment variable
      ENV NAME Wisecow

    # Run app.py when the container launches
     CMD [python app.py]        

# app.py

    from flask import Flask
    from werkzeug.utils import url_quote
    
    app = Flask(__name__)
    
    @app.route('/')
    def hello():
        return """
        <html>
            <head>
                <title>Welcome to Wisecow</title>
                <style>
                    body {
                        font-family: 'Verdana', sans-serif;
                        background-color: #f0f8ff;
                        color: #333;
                        margin: 0;
                        padding: 0;
                        display: flex;
                        justify-content: center;
                        align-items: center;
                        height: 100vh;
                    }
                    .container {
                        text-align: center;
                        background-color: #fff;
                        border-radius: 10px;
                        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
                        padding: 40px;
                        max-width: 600px;
                    }
                    h1 {
                        color: #4CAF50;
                        font-size: 3em;
                    }
                    p {
                        font-size: 1.2em;
                        color: #555;
                        margin-top: 10px;
                    }
                    .button {
                        display: inline-block;
                        padding: 10px 20px;
                        margin-top: 20px;
                        font-size: 1.1em;
                        color: white;
                        background-color: #4CAF50;
                        border-radius: 5px;
                        text-decoration: none;
                        transition: background-color 0.3s;
                    }
                    .button:hover {
                        background-color: #45a049;
                    }
                </style>
            </head>
            <body>
                <div class="container">
                    <h1>Hello, World!</h1>
                    <p>Welcome to the Wisecow application, a simple yet powerful Flask app.</p>
                    <a class="button" href="#">Learn More</a>
                </div>
            </body>
        </html>
        """
    
    if __name__ == '__main__':
        app.run(host='0.0.0.0', port=80)


# requirements.txt

    flask==2.2.2
    requests==2.25.1
    numpy==1.21.0
    Werkzeug==2.2.2
    
# Build the Docker image:

    docker build -t wisecow-app 
    
# Test the Docker image locally:

    docker run -p 8080:80 wisecow-app

    docker run image
    docker.png file upload

# Kubernetes Deployment

## Install kubectl (assuming Ubuntu)

    subprocess.run sudo pt-get update
    subprocess.run sudo apt-get install -y apt-transport-https gnupg2
    subprocess.run sudo curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg sudo apt-key add 
    subprocess.run echo deb https://apt.kubernetes.io/ kubernetes-xenial main sudo tee -a/etc/apt/sources.list.d/kubernetes.list
    subprocess.run sudo apt-get update
    subprocess.run sudo apt-get install y kubectl

# Create Deployment YAML (wisecow-deployment.yaml):

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: wisecow-deployment
      labels:
        app: wisecow
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: wisecow
      template:
        metadata:
          labels:
            app: wisecow
        spec:
          containers:
          - name: wisecow
            image: <your-container-registry>/wisecow-app:latest
            ports:
            - containerPort: 80
        
# Create Service YAML (wisecow-service.yaml):

    apiVersion: v1
    kind: Service
    metadata:
      name: wisecow-service
    spec:
      selector:
        app: wisecow
      ports:
      - protocol: TCP
        port: 80
        targetPort: 80
      type: LoadBalancer
    Apply the manifest files:
    kubectl apply -f wisecow-deployment.yaml
    kubectl apply -f wisecow-service.yaml

# Continuous Integration and Deployment (CI/CD)

 ## GitHub Actions Workflow:
 ## Create .github/workflows/ci-cd.yml:

    name: CI/CD Pipeline

    on:
      push:
        branches:
          - main
    
    jobs:
      build-and-deploy:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout code
          uses: actions/checkout@v2
        - name: Login to Container Registry
          uses: docker/login-action@v1
          with:
            username: ${{ secrets.REGISTRY_harshithac12 }}
            password: ${{ secrets.REGISTRY_********* }}
        - name: Build Docker image
          run: docker build -t <your-container-registry>/wisecow-app:latest .
        - name: Push Docker image
          run: docker push <your-container-registry>/wisecow-app:latest
        - name: Deploy to Kubernetes
          run: kubectl apply -f wisecow-deployment.yaml
          env:
            KUBECONFIG: ${{ secrets.KUBE_CONFIG_DATA }}

# Implementation.py

    from kubernetes import client, config

    def main():
        # Load Kubernetes configuration (adjust as needed, e.g., for local testing or in-cluster configuration)
        config.load_kube_config()

    # Define Kubernetes API objects
    v1 = client.CoreV1Api()
    apps_v1 = client.AppsV1Api()

    # Define deployment object
    deployment = client.V1Deployment(
        api_version="apps/v1",
        kind="Deployment",
        metadata=client.V1ObjectMeta(name="wisecow-deployment"),
        spec=client.V1DeploymentSpec(
            replicas=1,
            selector=client.V1LabelSelector(
                match_labels={"app": "wisecow"}
            ),
            template=client.V1PodTemplateSpec(
                metadata=client.V1ObjectMeta(labels={"app": "wisecow"}),
                spec=client.V1PodSpec(
                    containers=[
                        client.V1Container(
                            name="wisecow",
                            image="your-image:latest",
                            ports=[client.V1ContainerPort(container_port=5000)],
                            volume_mounts=[
                                client.V1VolumeMount(
                                    name="tls-certs",
                                    mount_path="/etc/ssl/certs",
                                    read_only=True
                                )
                            ]
                        )
                    ],
                    volumes=[
                        client.V1Volume(
                            name="tls-certs",
                            secret=client.V1SecretVolumeSource(secret_name="tls-secret")
                        )
                    ]
                )
            )
        )
    )

    # Create deployment
    apps_v1.create_namespaced_deployment(namespace="default", body=deployment)

    if __name__ == "__main__":
        main()



     













    
        
