gcloud compute networks create privatenet --project=qwiklabs-gcp-02-a19dc001c4ab --subnet-mode=custom --bgp-routing-mode=regional

gcloud compute networks subnets create privatenet-us --project=qwiklabs-gcp-02-a19dc001c4ab --range=10.130.0.0/20 --network=privatenet --region=us-central1

gcloud compute --project=qwiklabs-gcp-02-a19dc001c4ab firewall-rules create privatenet-allow-ssh --direction=INGRESS --priority=1000 --network=privatenet --action=ALLOW --rules=tcp:22 --source-ranges=35.235.240.0/20

gcloud beta compute --project=qwiklabs-gcp-02-a19dc001c4ab instances create vm-internal --zone=us-central1-c --machine-type=n1-standard-1 --subnet=privatenet-us --no-address --maintenance-policy=MIGRATE --service-account=61294483124-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-9-stretch-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=vm-internal --reservation-affinity=any

gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap

gsutil cp gs://cloud-training/gcpnet/private/access.svg gs://adamclass

sudo apt-get update


