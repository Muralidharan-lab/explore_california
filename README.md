 Containerizing and Deploying the Explore California Sample Web App
_________________________________________________________________________________
Task Details
ser Story for Containerizing and Deploying the Explore California Sample Web App:
 
As a DevOps engineer,
I want to fork, containerize, and deploy the Explore California web application using a suitable Kubernetes runtime (e.g., Rancher Desktop or Minikube),
So that I can showcase my expertise in Docker containerization and Kubernetes deployment.
 
Acceptance Criteria:
 
1. Sample web app:
  - Fork the [Explore California repository](https://github.com/mickeyjohn/explore_california) and clone it locally.
 
2. Kubernetes Runtime Setup:
  - Install and set up a Kubernetes runtime of your choice, such as Rancher Desktop or Minikube, avoiding Docker Desktop due to company restrictions.
 
3. Containerization:
  - Write a Dockerfile for the web app and build the image.
  - Test the containerized app to ensure it runs correctly.
 
4. Kubernetes Deployment:
  - Author Kubernetes manifests for a Deployment and Service.
  - Deploy the app to your Kubernetes runtime and ensure it's operational.
 
5. Exposing the App:
  - Expose the app using a suitable Kubernetes Service.
  - Verify external accessibility and correct functionality.
 
6. Documentation:
  - Document the process and decisions made with a README.md file in your git repo.
____________________________________________________________________________________________________________________________________________________________________________________________
Architecture Diagram:
![image](https://github.com/user-attachments/assets/dda6f4e4-b62b-444e-9403-d01a5e791060)


1. Sample web app:
  - Fork the [Explore California repository](https://github.com/mickeyjohn/explore_california) and clone it local

Use the fork button to create a git fork:
![image](https://github.com/user-attachments/assets/c6e9dccd-bca3-4583-afea-c06063b065ab)
 
To clone the repository, copy the URL and use git clone cmd in git bash: git clone <repository_url>

2. Kubernetes Runtime Setup: Rancher Desktop
  
URL for Ranger desktop installation: Rancher Desktop by SUSE 

Use WSL to install Linux on Windows:  Install WSL | Microsoft Learn
![image](https://github.com/user-attachments/assets/48fef023-1c40-4c22-85d2-2687167988f6)

Open Powershell and run : wsl --install
 ![image](https://github.com/user-attachments/assets/feab157d-5cf9-4162-8aaf-aabc722b1fec)
 ![image](https://github.com/user-attachments/assets/a7ee3d6b-5ede-4698-aba0-4f7db5a85e48)

3. Containerization:
  - Write a Dockerfile for the web app and build the image.
  - Test the containerized app to ensure it runs correctly.

* Install git, kubectl, and docker on WSL if they aren't already. 

  Cmds:
-	snap install docker
-	snap install kubectl
-	snap install git-ubuntu = for clone https://github.com/mickeyjohn/explore_california

* Sign in to pull and push images using Docker:

  Cmd : docker login

Docker file :
![image](https://github.com/user-attachments/assets/c8e473d6-bc49-419b-811a-a54dfbe28372)
 
Cmd to build :

-	Docker build -t test .

Cmds to Tag, push and Run to Docker Hub: 

-	docker tag test muralidharan1993/murali_demo
-	docker push muralidharan1993/murali_demo

-	Docker run -d muralidharan1993/murali_demo
![image](https://github.com/user-attachments/assets/a96cf923-8169-4221-a4f3-8a316f1ad129)
 
Working status check: 

-	Docker inspect <CONTAINERID> = run this cmd to get the ip 
-	curl -kiv 172.17.0.2:80  = to check the o/p
 ![image](https://github.com/user-attachments/assets/62078677-bb9d-41d4-91da-2ff8ac306dc9)

4. Kubernetes Deployment:

Create files for deployment and service.

Deployment.yaml  file:
![image](https://github.com/user-attachments/assets/fd19b8bf-47d5-4094-8ea2-229bbb77cf06)


 Service.yaml file:
![image](https://github.com/user-attachments/assets/181068de-4228-4c71-b9c3-58f143b844cc)

  - Deploy the app on Kubernetes

Cmds to apply both: kubectl apply -f deployment.yaml && kubectl apply -f service.yaml

Verify the status of the pod: kubectl get pods -n explore-california
![image](https://github.com/user-attachments/assets/750524cc-6a40-4f78-9d3b-67698d820d9c)

 
Check the service : kubectl get svc -n explore-california

![image](https://github.com/user-attachments/assets/809128cd-ad55-4a68-a3fb-9aedf2109bf8)

Login to pod: kubectl exec -it explore-california-deployment-6c8f7f6bfc-chqff – sh

Working inside the pod:
 ![image](https://github.com/user-attachments/assets/5e0da5af-89d5-4147-9fb3-6d62e79559a1)

5. Exposing the App:

NodePort service was configured to expose the application:
 ![image](https://github.com/user-attachments/assets/809128cd-ad55-4a68-a3fb-9aedf2109bf8)
 
In  Browser:  http://localhost:30422
![image](https://github.com/user-attachments/assets/dd1b2d58-4a08-4d4a-88a3-0d10a4096d6a)
 

 

Why NodePort service:
 •   exposes the Service on a specific port of each node in the cluster. It is useful for development and testing environments where you want to access the Service externally but don't need a full load balancer setup. Each node will listen on the specified NodePort, and traffic will be forwarded to the Service.
