# Start a Kubernetes Engine cluster

export MY_ZONE=us-central1-a

gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2

kubectl version

# Run and deploy a container

kubectl create deploy nginx --image=nginx:1.17.10

kubectl get pods

kubectl expose deployment nginx --port 80 --type LoadBalancer

kubectl get services

# Scale up the number of pods running on your service

kubectl scale deployment nginx --replicas 3

kubectl get pods

# Confirm that your external IP address has not changed

kubectl get services

