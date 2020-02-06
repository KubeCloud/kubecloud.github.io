---
title: Setting up a Kubernetes 1.11 Raspberry Pi Cluster using kubeadm
description: >-
  It’s been a while, and I thought I would revisit my previous blog post on
  setting up a Raspberry Pi Kubernetes Cluster and publish an…
date: '2018-09-14T11:29:08.732Z'
author: Kasper Nissen
categories:
- "Raspberry Pi"
image: /images/1__WCeZuH7st1kQhjHjTj9kuw.jpeg
type: "post"
---

![](/images/1__WCeZuH7st1kQhjHjTj9kuw.jpeg)

It’s been a while, and I thought I would revisit my previous blog post on setting up a Raspberry Pi Kubernetes Cluster and publish an updated version of the guide with the latest version of Raspbian and Kubernetes. This is it. Some parts of this post are copy/paste from my previous [post](https://kubecloud.io/setup-a-kubernetes-1-9-0-raspberry-pi-cluster-on-raspbian-using-kubeadm-f8b3b85bc2d1) on setting up Kubernetes 1.9.0 Raspberry Pi cluster.

### **My Raspberry Pi cluster setup:**

*   4pc Raspberry Pi (We used the Raspberry Pi 3 Model B)
*   4pc 16 GB MicroSDHC cards
*   1pc Small Switch (We used the d-link go-sw-5e)
*   4pc 0.3m Ethernet cables (we chose different colors for easy identification)
*   1pc USB Power Hub (We used Anker PowerPort 6 60W)
*   4pc Micro-USB cables 0.3m (approx 1ft)

The rack is custom made. We created an illustrator template and sent it to a laser cutting company. The template can be [downloaded here](https://s3-eu-west-1.amazonaws.com/kubecloud-public/kubecloud.ai).

![](/images/0__YxRzSe9BJ2JuSyMt.jpg)

![](/images/0__86XanFA59q7M5xBs.jpg)

![](/images/0__Qwn1aclNYfleiOs__.jpg)

![](/images/0__GPcn3MHgE5zbLhVa.jpg)

### **Flashing SD-cards**

Let’s start out flashing SD-cards. This process is boring and takes around 5 minutes per. SD-card. I prefer using etcher for flashing SD-cards, it’s super easy. You can find a copy [here](https://etcher.io/). We also need an operating system. Grab the latest version of Raspbian on either raspberrypi.org or directly from [here](https://downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2018-06-29/) (this is a link to the version I used).

If you haven’t already, go grab a cup of coffee, and repeat this process for all the nodes (SD-cards) you want in your cluster.

**Flash it!  
**Insert the SD-card, use the image you just downloaded and then press: \`flash\`.

Once the image is flashed, take it out, and insert into the machine again (or just mount it again).

**Enable SSH**  
Now we need to make SSH available on the machine. We do that by creating an empty file in the boot directory

```
$ touch /Volumes/boot/ssh
```

Unmount the SD-card and put it in the slot of a Raspberry Pi. Attach the network and power cables, and fire it up.

After a minute or so, you should be able to SSH into the Raspberry Pi as follows:

```
$ ssh pi@raspberrypi.local
```

The default password for Raspbian is: _raspberry_

**Setup hostname and attach static ip  
**Next, we want to make the Raspberry Pi easier to identify, and therefore provide it with a new hostname, and further assign it a static IP.

In order to make this as easy for you, I’ve create the following script, that you can copy to the Raspberry Pi and execute:

```
$ nano hostname\_and\_ip.sh
```

and insert the script:

Now run the script, an example of my naming and ip convention can be seen below. But adapt to your liking.

First argument: the new hostname  
Second argument: the new static IP  
Third argument: the IP of your Router

**_master: 192.168.1.100_**

```
$ sh hostname\_and\_ip.sh k8s-master 192.168.1.100 192.168.1.1
```

**_worker-01: 192.168.1.101_**

```
$ sh hostname\_and\_ip.sh k8s-worker-01 192.168.1.101 192.168.1.1
```

**_worker-02: 192.168.1.102_**

```
$ sh hostname\_and\_ip.sh k8s-worker-02 192.168.1.102 192.168.1.1
```

**_worker-03: 192.168.1.103_**

```
$ sh hostname\_and\_ip.sh k8s-worker-03 192.168.1.103 192.168.1.1
```

Now, reboot the Pi. You should be able to access the Pi over SSH as follows:

```
$ ssh pi@k8s-master.local (or k8s-worker-01.local etc.)
```

Verify that your Pi now also has a new static IP by running _ifconfig_.

#### Installing the prerequisites

Now, that the static networking and naming is in place, we need to install some software on the Raspberry Pi.

Therefore, create a new file on the Raspberry Pi:

```
$ nano install.sh
```

Copy and insert the following script.

Execute the script

```
$ sh install.sh
```

This will install and configure docker, disable swap and install kubeadm.

Reboot the machine, and repeat this process for all your Raspberry Pis.

### Initialize the Kubernetes master

So, we are now ready to set up Kubernetes. To do this, we are going to use the awesome tool called, kubeadm. This makes it pretty easy to spin up a Kubernetes cluster by, basically, running _kubeadm init_ on the master node and _kubeadm join_ on the worker nodes.

One of the purposes of this cluster is going to be demoing Kubernetes stuff. One example could be to pull out the network cable of one of the worker nodes and demoing how Kubernetes deals with this situation by rescheduling the pods from the lost node.

Therefore, we would like to change one of the arguments to the kube-controller-manager, namely, pod-eviction-timeout which defaults to 5 minutes. That’s a long time to wait in a presentation. Instead, we want to change this to 10s. You may also want to change the time that Kubernetes allows the node to be unresponsive. It defaults to 40 seconds. To change this, add the following argument to the master configuration : `node-monitor-grace-period: 10s.`

Changing arguments passed to the different Kubernetes core components by kubeadm is pretty simple. We just have to pass a YAML configuration file specifying the arguments we want to change. Let’s do that.

Create the configuration file:

```
$ nano kubeadm\_conf.yaml
```

Copy and insert the following

```
apiVersion: kubeadm.k8s.io/v1alpha1kind: MasterConfiguration
```

Save and run

```
$ sudo kubeadm init --config kubeadm\_conf.yaml
```

This takes a couple of minutes. Once the process finishes you should see something similar to:

```
...

Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

mkdir -p $HOME/.kube  
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config  
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.  
Run "kubectl apply -f \[podnetwork\].yaml" with one of the options listed at:  
  [https://kubernetes.io/docs/concepts/cluster-administration/addons/](https://kubernetes.io/docs/concepts/cluster-administration/addons/)

You can now join any number of machines by running the following on each node  
as root:

kubeadm join --token TOKEN 192.168.1.100:6443 --discovery-token-ca-cert-hash HASH
```

Follow the instructions in the output:

```
$ mkdir -p $HOME/.kube  
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config  
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

You can verify that your master node is up and running.

```
pi@master:~ $ kubectl get nodes  
NAME      STATUS     ROLES     AGE       VERSION  
master    NotReady   master    3m        v1.9.0
```

Don’t mind the status being NotReady. In order for the master node to become ready, we need to install a container network. But before we do that, let’s add some more nodes to the cluster first.

### Setting up the worker nodes

Alright, next up we need to spin up some workers for the cluster to be complete.

Assuming you have already set up the prerequisites mentioned above, we basically only need to run the kubeadm join on each of your worker nodes. As shown above, kubeadm outputs the command that you need to run on all your worker nodes.

```
$ sudo kubeadm join --token TOKEN 192.168.1.100:6443 --discovery-token-ca-cert-hash HASH
```

Repeat for every node.

### Set up weave as the container network

Nodes are not able to communicate without a container network, which is something you have to provide. Therefore, the final piece of the puzzle is to add one. We will be using weave-net for this. On the master node, run the following command:

```
$ kubectl apply -f “[https://cloud.weave.works/k8s/net?k8s-version=$(kubectl](https://cloud.weave.works/k8s/net?k8s-version=$%28kubectl) version | base64 | tr -d ‘\\n’)
```

### All set and done…

That was it. You should now have a fully functioning Raspberry Pi Kubernetes cluster. Verify your setup by running

```
pi@k8s-master:~ $ kubectl get nodes  
NAME            STATUS    ROLES     AGE       VERSION  
k8s-master      Ready     master    17m       v1.11.2  
k8s-worker-01   Ready     <none>    10m       v1.11.2  
k8s-worker-02   Ready     <none>    10m       v1.11.2  
k8s-worker-03   Ready     <none>    6m        v1.11.2
```
