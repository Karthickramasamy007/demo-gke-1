# INTRODUCTION
* This project contains: 
    - gcloud configuration, 
    - gcp project creation, 
    - link billing to project, 
    - enabeling api for gke, 
    - creating gke cluster with one node,
    - deploying demo flask app in gke from loacal system
    - creating load balancer type service for demo flask app
    - accessing flask app via external ip address


# GCLOUD CONFIGURATION
* Make sure you have gcloud cli is installed in your local machine.
* Setup gcloud account and gcloud cli configuration
* gcloud init
* using the above command we have created gcloud config called  'demo-dev-gcloud-configuration'
* Now you can make sure your active gcloud account by (* is active): gcloud auth list
* You can also check properties of our created gcloud config 'demo-dev-gcloud-configuration' by : gcloud config list

# CREATE PROJECT
* create project : gcloud projects create dev-demo-proj-id --name="demo-dev-project" --labels=type=dev
* you can check it by : gcloud projects list --sort-by=projectId --limit=5

# BILLING
* we need to create and link billing account with our new project.
* created billing account via UI
* to list all available billing accoounts for current active user: gcloud beta billing accounts list
* to link project to billing : gcloud beta billing projects link <project-id> --billing-account <billing-account-id>

# ENABLE API'S
* enable api for gke cluster: gcloud services enable container.googleapis.com

# CREATE GKE CLUSTER
* gcloud container clusters create demo-dev-gke-cluster --region us-west1 --node-locations us-west1-c --num-nodes 1 --cluster-version latest
* describe cluster: gcloud container clusters describe demo-dev-gke-cluster --region us-west1
* your local kube config and current-context will automatically get updated with this latest gke cluster details. Now you can run kubectl commands from your local machine. You can check the current context by: kubectl config current-context 
* Or you can view full kube config by : kubectl config view

# DEPLOY DIRECTLY TO K8S DEV CLUSTER FROM LOCAL MACHINE TERMINAL
* we already have some flask app image in our docker hub.
* we are deployin that flask app image using deployment.yaml manifest in k8s-manifest folder.
* cd into k8s-manifest folder.
* kubectl apply -f deployment.yaml
* now creating 'Loadbalancer' type service using services.yaml file.
* kubectl apply -f services.yaml

# ACCESSING THE FLASK WEB APP FROM INTERNET VIA EXTERNAL LOADBALANCER IP
* first, get the load balancer ip and port by : kubectl get svc
* now get the ip address from 'External-ip' field and port number from 'port' field
* Now you can browser the ip or curl using this eg: http://89.83.111.68:5000/app2
* /app2 is path in the flask.

# CONFIGURE GIT REPO AND PUSH CODE:
* Get into the folder that you want to push into git. Here 'cicd-demo-1'
* Also, create a repo called 'cicd-demo-1' in git hub using UI.
* git init
* git add .
* git commit -m "first commit"
* git branch -M master
* git remote add origin https://github.com/Karthickramasamy007/cicd-demo-1.git
* git push -u origin master
