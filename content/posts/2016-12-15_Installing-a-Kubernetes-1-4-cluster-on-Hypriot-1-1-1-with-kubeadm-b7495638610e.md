---
title: Installing a Kubernetes 1.4 cluster on Hypriot 1.1.1 with kubeadm
description: >-
  It’s been a while since our last blog post. However, we are back and again
  working with Kubernetes on a daily basis. This blog post will…
date: '2016-12-15T20:32:00.000Z'
categories: []
keywords: []
slug: >-
  /@phennex/installing-a-kubernetes-1-4-cluster-on-hypriot-1-1-1-with-kubeadm-b7495638610e
---

![](img/1__ipFQkJrHHyzT2i82LaVLyQ.jpeg)

It’s been a while since our last blog post. However, we are back and again working with Kubernetes on a daily basis. This blog post will show you how to set up a Kubernetes 1.4 cluster with HypriotOS 1.1.1 and the new `kubeadm` tool.

### Prerequisites

*   A couple of Raspberry Pis (minimum two)
*   A switch to connect the Raspberry Pis
*   Cables for power and network

For this tutorial we used 4 Raspberry Pi 3 and a Macbook Pro.

### Flashing the SD-cards

First, flash the SD cards with [HypriotOS 1.1.1](https://github.com/hypriot/image-builder-rpi/releases/download/v1.1.1/hypriotos-rpi-v1.1.1.img.zip) using the Hypriot [flash tool](https://github.com/hypriot/flash).

```
$ flash --hostname master hypriotos-rpi-v1.1.1.img.zip $ flash --hostname slave01 hypriotos-rpi-v1.1.1.img.zip $ flash --hostname slave02 hypriotos-rpi-v1.1.1.img.zip $ flash --hostname slave03 hypriotos-rpi-v1.1.1.img.zip
```

When the flash of the 4 sd-cards has completed, insert them into the 4 Raspberry Pis and power them up. Make sure you are able to contact the Pis by SSH’ing into them one by one. The default password for the pirate-user is: _hypriot_

```
$ ssh pirate@master.local $ ssh pirate@slave01.local $ ssh pirate@slave02.local $ ssh pirate@slave03.local
```

After logging into the Pis, `exit` back to your machine.

Next, we need to set up our SSH-keys:

```
ssh-add ssh-keygen -R master.local ssh-copy-id pirate@master.local
```

Repeat the above for all the Pis.

### Configuring the IPs

To make things easier to handle, I want to change the IPs to static IPs. I chose the IP scheme to be:

```
192.168.1.100 (master) 192.168.1.101 (slave01) 192.168.1.102 (slave02) 192.168.1.103 (slave03)
```

**Repeat for all Pis**

First SSH into the Pi

```
$ ssh pirate@master.local $ sudo nano /etc/network/interfaces
```

Replace the content of this file with

```
auto lo iface lo inet loopback 
```

```
allow-hotplug eth0ifaceeth0 inet static address **<YOUR IP GOES HERE>** netmask 255.255.255.0 network 192.168.1.0 broadcast 192.168.1.255 gateway 192.168.1.1 dns-nameservers 192.168.1.1 8.8.8.8 8.8.4.4 
```

```
iface eth0 inet6 auto 
```

```
allow-hotplug wlan0 iface wlan0 inet dhcp pre-up /usr/bin/occi wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf iface default inet dhcp
```

Then reboot each device by running

```
sudo reboot
```

### Installing Kubernetes

Switch to the root user

```
sudo -s
```

And execute the following, which will add the Kubernetes apt-get respository to the resources.

```
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - $ cat <<EOF > /etc/apt/sources.list.d/kubernetes.list deb http://apt.kubernetes.io/ kubernetes-xenial main EOF
```

**Notice: Make sure there isn’t a space after _EOF_.**

Update everything

```
$ apt-get update
```

Then install the `kubelet`, `kubeadm`, `kubectl`, and `kubernetes-cni`

```
$ apt-get install -y kubelet kubeadm kubectl kubernetes-cni
```

### Setting up the master

```
kubeadm init --pod-network-cidr=10.244.0.0/16
```

The `--pod-network-cidr=10.244.0.0/16` is needed for flannel to be configured correctly. Flannel is at the moment the only overlay network that works with Raspberry Pis. When the command has finished, (which will take a couple of minues) the output will be similar to the following:

```
Public: /etc/kubernetes/pki/ca-pub.pem Private: /etc/kubernetes/pki/ca-key.pem Cert: /etc/kubernetes/pki/ca.pem <master/pki> generated API Server key and certificate: Issuer: CN=kubernetes | Subject: CN=kube-apiserver | CA: false Not before: 2016-11-24 20:25:58 +0000 UTC Not After: 2017-11-24 20:26:02 +0000 UTC Alternate Names: [192.168.1.100 10.96.0.1 kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] Public: /etc/kubernetes/pki/apiserver-pub.pem Private: /etc/kubernetes/pki/apiserver-key.pem Cert: /etc/kubernetes/pki/apiserver.pem <master/pki> generated Service Account Signing keys: Public: /etc/kubernetes/pki/sa-pub.pem Private: /etc/kubernetes/pki/sa-key.pem <master/pki> created keys and certificates in "/etc/kubernetes/pki" <util/kubeconfig> created "/etc/kubernetes/kubelet.conf" <util/kubeconfig> created "/etc/kubernetes/admin.conf" <master/apiclient> created API client configuration <master/apiclient> created API client, waiting for the control plane to become ready <master/apiclient> all control plane components are healthy after 188.931005 seconds <master/apiclient> waiting for at least one node to register and become ready <master/apiclient> first node is ready after 5.533117 seconds <master/apiclient> attempting a test deployment <master/apiclient> test deployment succeeded <master/discovery> created essential addon: kube-discovery, waiting for it to become ready <master/discovery> kube-discovery is ready after 185.528275 seconds <master/addons> created essential addon: kube-proxy <master/addons> created essential addon: kube-dns Kubernetes master initialised successfully! You can now join any number of machines by running the following on each node: kubeadm join --token=c3625d6ebda8defc 192.168.1.100
```

Should anything fail during the setup, run `kubeadm reset`, then `systemctl start kubelet.service`, and try again.

Next, we need to install the cluster network, which as mentioned earlier will be flannel.

```
$ export ARCH=arm $ curl -sSL "https://github.com/coreos/flannel/blob/master/Documentation/kube-flannel.yml?raw=true" | sed "s/amd64/${ARCH}/g" | kubectl create -f -
```

Verify that the kube-dns pod is running: `kubectl get pods --all-namespaces`

### Setting up the slaves/workers

Copy the command from the output of the master setup and run it on all your workers.

```
kubeadm join --token <token> <master-ip>
```

After a couple of minutes, your Raspberry Pi Kubernetes cluster is ready. Verify with `kubectl get nodes` on the master pi. The output should be similar to this:

```
NAME STATUS AGE master Ready 13m slave01 Ready 1m slave02 Ready 51s slave03 Ready 30s
```

### Getting access to your cluster from you own machine

Make sure you have `kubectl` installed. Then SSH into the master and open the following file:

```
$ ssh pirate@master.local $ sudo cat /etc/kubernetes/admin.conf
```

Copy the content of this file to a place on your own machine. I just copy/pasted it in to a file called `raspberrypi.conf`.

Now, from your local machine you can access the cluster as follows:

```
$ kubectl --kubeconfig raspberrypi.conf get nodes
```

Now your Kubernetes cluster is ready to be used! Awesome! :)

_Originally published at_ [_kubecloud.io_](http://kubecloud.io/kubernetes-cluster-with-kubeadm/) _on December 15, 2016._