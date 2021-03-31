# Kubernetes Cluster on Raspberry Pi's

This project is to experiment with 3d printing and developing a low powered Kubernetes cluster on raspberry Pi’s. As I am creating/printing the cluster case itself, I want to include a switch, power, and space for the raspberry Pi’s in a compact and portable manor.

## Requirements:
1 Ender 3 pro (or similar 3 printer)

3x Raspberry Pi 3

1x TP link 8 port switch unmanaged switch https://www.amazon.com/Ethernet-Splitter-Optimization-Unmanaged-TL-SG108/dp/B00A121WN6

1x Anker 60w 6 port USB charging station amazon.com/Anker-Charger-PowerPort-iPhone-Galaxy/dp/B00P936188/

3x Right angle Micro USB Cable https://www.amazon.com/SUNGUY-3-Pack-Degree-Charging-Samsung/dp/B07H172CHD/

3x .5 FT Ethernet cables https://www.monoprice.com/product?p_id=3426

1x Ubiquiti USG (Or any router will work) https://www.ui.com/unifi-routing/usg/

## Printing

I started to made a very basic mockup on Autodesk fusion 360:

<img src="https://user-images.githubusercontent.com/47187712/109887461-b96b2700-7c4f-11eb-8bdd-c58f1cb02fa9.jpg" width="500" height="400">

I then started to print individual pieces to make sure they would fit properly.

<img src="https://user-images.githubusercontent.com/47187712/109887688-2c749d80-7c50-11eb-879d-cb370bec29d7.jpg" width="300" height="300">
This is the first prototype, not enough space for the whole pi to fit sadly...

Here is the first design to fit (had to cut the top off for the PI's):

<img src="https://user-images.githubusercontent.com/47187712/110850077-627fd600-827d-11eb-94c7-ff53f5788cf3.jpg" width="400" height="400">

Now all we need is the software!!


### Network:
My network is split up in the 10.10.10.0 subnet and split the names by network cable for easier diagnosing

<img src="https://user-images.githubusercontent.com/47187712/110851715-657bc600-827f-11eb-8bfa-5d5ef6eed73d.png">

### RPI SETUP:

After the Pi's have booted up for the first time, I needed to configure the Hostname and change the default passwords.

Run: <code> sudo raspi-config </code>

Set the new password, enable SSH, Change the GPU memory from 64=>16 and change the hostname to your preference. Mine will be ROY, GEE, and BIV to easily distungush the RPI's based on Ethernet Cable.

I have also configured each PI with a static IP:

In /etc/dhcpcd.conf, ROY example:
<code>      

static ip_address=10.10.10.6/24

static routers=10.10.10.1

static domain_name_servers=10.10.10.1 8.8.8.8

</code>

I also added in the /boot/cmdline.txt so we can run the k3s containers:

<code> cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory </code>

### Installing K3s Software:

I have decided to work with k3s, which is a lightweight implementation of kubernetes. This allows for more resources on these already limited RPI 3's.

On the master node (ROY) I ran:

<code> curl -sfL https://get.k3s.io | sh -</code>

Then you can find the node token by entering:

<code> sudo cat /var/lib/rancher/k3s/server/node-token </code>


On each of the worker nodes (GEE,BIV) I have ran:

<code> curl -sfL https://get.k3s.io | K3S_URL=https://10.10.10.6:6443 K3S_TOKEN=nodetoken sh - </code>

Running <code> kubectl get nodes </code> You should see all 3 nodes connected!

![image](https://user-images.githubusercontent.com/47187712/112377362-05901100-8cbc-11eb-8123-fd77a95a38cc.png)

### Package Manager

I installed Helm (https://helm.sh/) a package manager for Kubernetes containers.
Following their documentation:

<code>
        
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3

$ chmod 700 get_helm.sh

$ ./get_helm.sh

</code>

This allows us to easily install premade containers on all of our nodes. 

### Monitoring the cluster

To get better idea of how I use my resources, I am going to install prometheus, A time series database and metrics collector. then I will report this information via Grafana a dashboarding tool.

Thankfully with helm there is a pre-configured promethus and grafana container that we can deploy.

<code>
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
        
helm repo add stable https://kubernetes-charts.storage.googleapis.com
        
helm repo update

kubectl create ns monitoring

sudo -E helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring
</code>

Afterwards when running <code> sudo kubectl get pods -n monitoring </code>

![image](https://user-images.githubusercontent.com/47187712/113207968-46999f80-923f-11eb-83a1-03c7b39114bf.png)

We can now see the pods are up and running and we are collecting metrics on our server!

in order to view these pods outside of the pods network we have to portfoward to the main node and then we can view it. 
<code> sudo kubectl port-forward -n monitoring my-kube-prometheus-stack-grafana-5b9645d744-l5cfs --address 0.0.0.0 3000 </code>

I can go to my web browser to 10.10.10.6:3000 and the login page will be displayed

Credentials are by default admin/admin

<img src="https://user-images.githubusercontent.com/47187712/113209394-03d8c700-9241-11eb-8c32-afac895f8464.png" width="300" height="300">


It also comes with a few different premade dashboard to see activity on each pod.

### Changelog:
1.0.0
        Created Readme, Licenese, progresslog, and TODO

1.0.1
        Added to Requirments
        
1.1.0
        Added Printing information
        
1.1.1
        Added network diagram
        
        Added More printing information
        
1.1.2
        Added RPI SETUP
        
        Added Installing K3s Software
        
1.1.3
        Added Kubectl photos
        
        Added special RPI instruction so I could install k3s.
        
1.2
        Added Package Manager
        
        Added Monitoring the cluster
1.2.1
        Added to monitoring the cluster with pod info and Grafana
