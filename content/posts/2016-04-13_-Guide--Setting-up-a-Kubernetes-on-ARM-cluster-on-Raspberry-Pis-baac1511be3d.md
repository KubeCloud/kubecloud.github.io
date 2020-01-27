---
title: '[Guide] Setting up a Kubernetes on ARM cluster on Raspberry Pis'
description: >-
  This blog post will walk you through the steps of setting up kubernetes-on-arm
  that Luxas has created. Thanks to Luxas’ project it is…
date: '2016-04-13T13:08:34.838Z'
categories: []
keywords: []
slug: >-
  /@phennex/guide-setting-up-a-kubernetes-on-arm-cluster-on-raspberry-pis-baac1511be3d
---

This blog post will walk you through the steps of setting up kubernetes-on-arm that [Luxas](https://github.com/luxas/) has created. Thanks to [Luxas’ project](https://github.com/luxas/kubernetes-on-arm) it is pretty straightforward to get Kubernetes up and running on a Raspberry Pi. This guide will walk you through a fork of Luxas’ project, that contains minor adjustments to the [DNS](https://github.com/rpicloud/kubernetes-on-arm/commit/d7635f8e6e45f9657f53f71b5f0b54403da0c7af) and a couple of [extra scripts](https://github.com/rpicloud/kubernetes-on-arm/commit/51d5edaa548f70680e43cdaa7f0a79d9edb371a6).

#### Overall setup

The goal of this guide to get a cluster of four nodes up and running. One master and three workers. In this guide ArchLinux will be installed on all of the nodes and static IPs assigned. We have tried ArchLinux out and found it most stable in our setup. Afterwards Kubernetes-on-arm will be installed, and each node configured as either a master or a worker. Let’s get started!

![](img/0__319jSZ4qdgJgHaW4.jpg)

#### SD cards

First the SD cards must be flashed. Since we are using OSX, we used the build-in Disk Utility to erase each SD card with the format “MS-DOS (FAT)” and Scheme “GUID Partition Map”. Remember to unmount each card before you unplug it.

![](img/0__VylGV3YROrxRl__FT.png)

Afterwards we continued on a Linux machine inserting one SD card at a time.

```
sudo fdisk -l
```

![](img/0__S8Vq0N3NiOwm61tg.png)

As seen above the SD card is found as /dev/sdb, your device might get a different name. To be able to write to the SD card /dev/sdb2 is unmounted. Afterwards pull kubernetes-on-arm and start writing with the arguments for the Raspberry Pi 2, ArchLinux and kubesystemd. There are more options on Luxas’ kubernetes-on-arm.

```
$ sudo umount /dev/sdb2 $ git pull https://github.com/rpicloud/kubernetes-on-arm.git $ cd kubernetes-on-arm $ sudo sdcard/write.sh /dev/sdb rpi-2 archlinux kube-systemd
```

You will be prompted that you will loose all data on the device, so make sure that you select the right device from the fdisk step.

The write script will download and copy the files to your SD card, and in the end you should see an output similar to the one below.

![](img/0__1gqy2EiUdWpRXCE1.png)

Repeat this step for each SD card and you should be ready for the next step.

#### Static IP

In order to know where the different nodes are both for yourself and Kubernetes, static IPs can be a help. Since we have eight clusters and a router with gateway IP `192.168.1.1`, we use a convention that concatenates (notice || syntax later) the cluster number with the node number in the last section of the IP: `192.168.1.(cluster || node)`. Cluster 1 node 1 will get the IP: `192.168.1.11`, while node 2 in the same cluster will get: 192.168.1.12.

Now, how do we connect a Raspberry Pi? You can either plug in an HDMI-cable and a keyboard or use SSH. We plugged in one Raspberry Pi at a time, found the newly assigned IP under ‘logs’ section and changed it one at a time.

An example could be that your router assigned the first Raspberry Pi the IP `192.168.1.189` you can log in and change your IP in the following way.

```
$ ssh root@192.168.1.189 # You will be asked if you trust the key fingerprint - type 'yes' # Afterwards type the password 'root' # Edit the following file e.g. with nano $ nano /etc/systemd/network/dns.network
```

When you have opened the file, delete everything and paste in the lines below. Notice that `192.168.1.11` is the static IP address here. The DNS entry sets up `10.0.0.10` which is default for kube-dns, the local `192.168.1.1` gateway and Google's DNS.

```
[Match] Name=eth0 [Network] Address=192.168.1.11/24 Gateway=192.168.1.1 Domains=default.svc.cluster.local svc.cluster.local cluster.local DNS=10.0.0.10 192.168.1.1 8.8.8.8
```

Hit ctrl+x and then ‘y’ to save the file and reboot to make the change go through.

```
reboot
```

Now connect as earlier with the new IP.

```
$ ssh root@192.168.1.11
```

#### Installing Kubernetes (and Docker)

From Luxas’ image comes a command line tool called `kube-config` that can control much of the configuration around Kubernetes. To install Docker and Kubernetes run:

```
kube-config install
```

It will take some time to download everything, and in the end you will be prompted for a name. We have used the same naming convention as the last section of the IP, giving cluster 1’s node 1 the name node11.   
 Afterwards you will be asked for timezone, swapfile and if you want to reboot. The default values seems fine, so just press enter.

Since one of your nodes must be a master, you have to branch out differently from here.

Master

On the master simply run the command:

```
kube-config enable-master
```

From the command has finished until Kubernetes is up can take some minutes. You can follow the creation of Docker containers by running `docker ps` and, at some point, `kubectl get nodes`. Get nodes will show the nodes connected to this master (including the master itself).

Slave

For each slave (e.g. ending IP with 12, 13, 14) you need to wait for the master to be ready and then specify the ip of the master in the following way.

```
kube-config enable-worker 192.168.1.11
```

When all of the nodes have downloaded and started a worker, check from the master that the workers are connected to it by running `kubectl get nodes`.

#### Startup and shutdown

The extra scripts, mentioned in the introduction, can help you start up and shut down your cluster from the master node without bringing the nodes in an inconsistent state.   
 In order, for the master, to be able to run commands on the workers you need to set up ssh-keys and copy them to the workers. You can do so in the following way (e.g. from node11):

```
ssh-keygen # Hit enter about three times ssh-copy-id root@192.168.1.12 ssh-copy-id root@192.168.1.13 ssh-copy-id root@192.168.1.14
```

The scripts are placed in the /root/ folder, and you run them from the master node in the following way.

```
sh startup.sh sh shutdown.sh
```

Now you should be ready to play with Kubernetes on your Raspberry Pi cluster. Have fun with it!

_Originally published at_ [_kubecloud.io_](http://kubecloud.io/kubernetes-on-arm-cluster/) _on April 13, 2016._