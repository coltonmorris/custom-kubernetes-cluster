# custom-kubernetes-cluster
My notes on setting up a custom kubernetes cluster using old computers sitting around the house


## Installing OS
---

I installed the Ubuntu Server 16.04.3 on my two ~6 year old laptops. Might as well use them for something useful, rather than sit around and collect dust. If I decide to add raspberry pi's to the cluster, I'll look into using [the OS **Hypriot**](https://blog.hypriot.com/post/setup-kubernetes-raspberry-pi-cluster/) which has Kubernetes pre-installed.

Setting up networking wasn't trivial. My laptops do not have WiFi, and they are no where near the current router. To prevent stringing an ethernet across the house, I went and bought a Netgear WiFi Range Extender (AC750). A normal WiFi router will not do the trick, as you need an **Extender**, and **not an Access Point**.

Now I was able to connect a **Router** to the **Extender** and connect my multiple laptops with Ethernet.    
Wait!?     
What's this!?     
It doesn't work!?      
Must enable ethernet now...    
1. `ip link` shows the ethernet interface is `enp0s20` or something similar
2. Add an entry to `/etc/network/interfaces`
```
# The primary network interface
auto enp0s20
iface enp0s20 inet dhcp
```
Now restart the interface
3. `sudo ifdown enp0s20 && sudo ifup -v enp0s20`
Lastly, check if it works
4. `ping -c3 www.ubuntu.com` 

*Viola! Now we can install Docker.*

## Installing Docker
---

First install `apt-transport-https` - a package that allows using https as well as http in apt repository sources.
1. `sudo apt-get update && sudo apt-get install -y apt-transport-https`
Next install `docker`
2. `sudo apt install docker.io`
Now start and enable the Docker service
3. `sudo systemctl start docker && sudo systemctl enable docker`

*Viola! Now we can install Kubernetes.*

## Installing Kubernetes
---

Download and add the key
1. `sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add`
 
2. Create the file `/etc/apt/sources.list.d/kubernetes.list` with the following contents
```
deb http://apt.kubernetes.io/ kubernetes-xenial main 
```
3. `sudo apt-get update && sudo apt-get install -y kubelet kubeadm kubectl kubernetes-cni`

*Viola! Now you can setup your cluster.*
