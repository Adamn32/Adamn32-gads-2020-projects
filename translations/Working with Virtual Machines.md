# Create mc-server VM
gcloud beta compute --project=qwiklabs-gcp-00-a34cb8026eb2 instances create mc-server --zone=us-central1-a --machine-type=n1-standard-1 --subnet=default --address=34.72.233.185 --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=945847787389-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/trace.append,https://www.googleapis.com/auth/devstorage.read_write --tags=minecraft-server --image=debian-9-stretch-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=mc-server --create-disk=mode=rw,size=50,type=projects/qwiklabs-gcp-00-a34cb8026eb2/zones/us-central1-a/diskTypes/pd-ssd,name=minecraft-disk,device-name=minecraft-disk --reservation-affinity=any

# Prepare the data disk and mount it using SSH
sudo mkdir -p /home/minecraft

sudo mkfs.ext4 -F -E lazy_itable_init=0,\
lazy_journal_init=0,discard \
/dev/disk/by-id/google-minecraft-disk

sudo mount -o discard,defaults /dev/disk/by-id/google-minecraft-disk /home/minecraft

# Install and run the application java Virtual Machine (JVM)
sudo apt-get update

sudo apt-get install -y default-jre-headless

cd /home/minecraft

sudo apt-get install wget

sudo wget https://launcher.mojang.com/v1/objects/d0d0fe2b1dc6ab4c65554cb734270872b72dadd6/server.jar

sudo java -Xmx1024M -Xms1024M -jar server.jar nogui

# Enable eula=false to eula=true
sudo nano eula.txt

# Create a virtual terminal screen to start the Minecraft server
sudo apt-get install -y screen

sudo screen -S mcs java -Xmx1024M -Xms1024M -jar server.jar nogui

sudo screen -r mcs

# Create firewall minecraft-rule
gcloud compute --project=qwiklabs-gcp-00-a34cb8026eb2 firewall-rules create minecraft-rule --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:25565 --source-ranges=0.0.0.0/0 --target-tags=minecraft-server


