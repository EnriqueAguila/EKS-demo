![eks](https://github.com/user-attachments/assets/9520e3a8-c969-4a4a-a3b8-e6c0979b1b4b)

# EKS-demo

## Deploying a Containerized Application on EKS

This time I will show you how to deploy a containerized application on eks.

Adding AWS credentials to my local machine and also giving admin access to my user.

Running Docker Desktop with Kubernetes configured on my instance.

First I built a simple node.js server that is going to use the Express framework which when the contacts route is reached on port 3000 will return a hardcoded list of contacts.

Also a Dockerfile is going to be created which has a base of the Alpine node. Copying the package.json file from the local source into the worker and then running npm install to pull the dependencies and then copying the rest of the source code into the worker indicates that it is exposing port 3000 and then running the server, going to the terminal, to build the image using the dockerfile, then starting an instance of the application to test it locally.

In the terminal, the image will be pushed to Docker Hub with a tagged version of 1.0.

In the template, the metadata name for the application server is set, an application server tag is given for the application, and the container specification has an application server name, since the image comes from the Docker Hub.

A load balancer type service is added for deployment in eks and I also specified a port of 3000.

Remember that all of this is done through the eks CTL CLI from weaveworks, which will allow us to interact with eks through a command line on our local machine.

In this part, you will see how the stack is created and completed. Ok, with the creation of the full stack (cloudformation), the cube configuration will be updated by passing the name of the cluster that was created to a region and this will enable Cube CTL, and we will see how a stack was created for the demo workers on the node.

## Comands CLI

Install your stay updates first:
```
sudo apt update
```
```
sudo apt upgrade -y
```

### Install the necessary packages: Install the necessary packages for Docker:

```
sudo apt install -y ca-certificates curl gnupg lsb-release
```

### Update the package list: Update the package list to include the Docker repository:

```
sudo apt update
```


### Install Docker:
```
sudo apt update
```
```
sudo apt install podman-docker
```
```
sudo systemctl start docker
```
```
sudo systemctl enable docker
```
```
docker --version
```

### Create a file called Dockerfile.
On your local machine, create a file called Dockerfile with the content you provided:

```
FROM node:alpine
WORKDIR /src
COPY ./src/package.json .
RUN npm install
COPY ./src/ .
EXPOSE 3000
```
```
CMD ["node", "server.js"]
```

### Create a package.json and server.js file
Make sure you have a package.json and server.js file in a directory called src in the same location as the Dockerfile.
Copy the files to the Ubuntu instance
You can copy the files to the Ubuntu instance using SCP (Secure Copy) or SFTP:

```
scp -r /ruta/local/src usuario@ip- instancia-ubuntu:/ruta/destino/
scp /ruta/local/Dockerfile usuario@ip-instancia-ubuntu:/ruta/destino/
```

### Change to the directory where the files are located
Change to the directory where the copied files are located:

```
cd /ruta/destino
```
Create the Docker image from the Dockerfile:
```
docker build -t mi-aplicacion .
```

### Run the container:

```
docker run -p 3000:3000 mi-aplicacion
```

### Configure eksctl

```
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl
eksctl version
```

```
Crete a Cluster
 eksctl create cluster \
 --name demo-cluster \
 --version 1.23 \
 --region us-east-1 \
 --nodegroup-name demo-workers \
 --node-type t3.micro \
 --nodes 3 \
 --nodes-min 1 \
 --nodes-max 4 \
 --managed
```

### Obtain a Cluster:
```
eksctl get cluster
```

### Create a Deployment on the EKS Cluster:
```
aws eks update-kubeconfig --name demo-cluster --region us-east-1
```
```
kubectl apply -f app-server-deployment.yaml
```
```
kubectl get all
```
```
kubectl get pods
```
```
kubectl get nodes
```
```
eksctl delete cluster --name demo-cluster
```

### Test Deployment:
1. kubectl get all
2. Copy LoadBalacner Endpoint

http://YOUR_LOAD_BALANCER_ENDPOINT:3000/contacts


![uno](https://github.com/user-attachments/assets/32f1ea45-ca40-40f7-a516-1419081124dd)

![dos](https://github.com/user-attachments/assets/060fa915-68cc-412c-9b99-70d468c9e53a)


![tres](https://github.com/user-attachments/assets/09fb640e-bff4-4f3a-955c-77db96c96f6b)

![cinco](https://github.com/user-attachments/assets/2d2bd4b4-cc3b-4cdd-a110-f488ba8bd954)

