<<<<<<< HEAD
# cs421_Assignment4


## Building and Running the Front-End Containers

1. Clone the Repository

```sh
git clone https://github.com/your-username/your-repo.git
cd your-repo
```
2.Build Docker Images
```sh
docker-compose build
```
3.Run the Containers
```sh
docker-compose up -d
```
4 Verify Deployment
Open your browser and go to http://<your-ec2-public-ip>. You should see the Vue.js front-end with the node identifier displayed

## Load Balancer Setup

 Round-Robin Algorithm
 
The NGINX load balancer is configured using a round-robin strategy to distribute traffic evenly across three Vue.js front-end containers:
```sh
upstream frontend_nodes {
    server frontend1:80;
    server frontend2:80;
    server frontend3:80;
}

```
 ## Health Checks
 
NGINX uses passive health checks based on response status codes. If a container is down (e.g., connection refused), it will be temporarily skipped until it becomes responsive again.

## AWS Deployment Guide
1. Launch EC2 Instance (Ubuntu)
   Use Free Tier-eligible Ubuntu 22.04 AMI.

   Open ports 80 (HTTP), 22 (SSH), and any others needed for your app.
2.Install Docker and Docker Compose
```sh
sudo apt update && sudo apt install -y docker.io docker-compose
sudo usermod -aG docker $USER
newgrp docker
```
3.Upload and Run the App
SCP or clone the repo:
```sh
git clone https://github.com/your-username/your-repo.git
cd your-repo
docker-compose up -d
```
4.Access the App

```sh
Visit http://<your-ec2-public-ip> in your browser.
```


## Troubleshooting Tips
1.Front-end not loading
Check the container logs using docker-compose logs frontend1 (or frontend2/frontend3). Also ensure that the NGINX load balancer is up and running properly.

2.Load balancer errors
Verify that the nginx.conf file is correctly configured and mapped. Make sure the frontend_nodes listed in the config are reachable and active.

3.Header not appearing
Ensure your Vue components are rendering properly and that all necessary environment variables are correctly set within the containers.

4.Connection refused errors
This may indicate one or more containers are down. Restart them using docker-compose restart.

5.Changes not reflected
If updates to the code are not showing up, try rebuilding the images with:

docker-compose build --no-cache
docker-compose up -d
