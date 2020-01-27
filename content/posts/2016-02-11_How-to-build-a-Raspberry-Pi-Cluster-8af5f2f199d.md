---
title: How to build a Raspberry Pi Cluster
description: >-
  This guide will provide you with the stuff you need for putting a Raspberry Pi
  cluster together. The first thing you need is of course some…
date: '2016-02-11T08:54:32.517Z'
categories: []
keywords: []
slug: /@phennex/how-to-build-a-raspberry-pi-cluster-8af5f2f199d
---

This guide will provide you with the stuff you need for putting a Raspberry Pi cluster together. The first thing you need is of course some Raspberry Pis . We choose to buy the Raspberry Pi 2 Model B, which is a pretty powerful little machine compared to the older Raspberry Pi. This little beast will hook you up with a quadcore 900 MHz processor and 1 GB of memory, which is descent for the price and the size.

#### Our setup

*   4 Raspberry Pi 2 Model B
*   4 Raspberry Pi 2 power supplies (2A)
*   D-link GO-SW-5E
*   4 RJ45 Cat5e Patch cables
*   4 16Gb Kingston Class 10 micro SDHC

![](img/0__wpOuQx8fGqzheoaO.jpg)

The next thing we need are of course some power! There exist some pretty cool USB power supplys on the market, which offers the actual current needed for the Raspberry Pi ([link](http://www.amazon.co.uk/Anker-Family-Sized-Desktop-Technology-Motorola/dp/B00PK1MADE/ref=sr_1_2?ie=UTF8&qid=1455175598&sr=8-2&keywords=anker+60w)). Notice that there are a big different of the requirements of the Raspberry Pi 2 compared to the older model. The requirements for the Raspberry Pi 2 is 1.8A opposite 1.2A for the older model. However we didn’t have the funds to buy USB power supply’s, so we bought the official white 2A power supply’s.

Now we have some hardware and some power, but we are missing a vital piece. We need a memory card for storing our OS and all of our applications. It’s recommended that you buy a class 10 SDHC card or better. We decided to go with a Kingston 16GB micro SDHC. For installation of the distribution onto the micro SD card, please read our previous [post](http://rpi-cloud.com/guide-running-docker-on-your-raspberry-pi/).

![](img/0__nFFqX84UsH6cSWvT.jpg)

We decided to create our own rack, if you don’t want do that, have a look at something like [this](http://www.amazon.co.uk/Zactech-Dogbone-Clear-Case-Raspberry/dp/B019MY9DNE/ref=sr_1_23?ie=UTF8&qid=1455175783&sr=8-23&keywords=raspberry+rack). We designed the models in SolidWorks, and had them made a facility provided by Aarhus University. The design features holes for wiring, and room for the Switch. You can download the models [here](http://www.rpi-cloud.com/files/rpi-cloud.com_raspberrypi_solidworks.zip). The Zip-file contains 2 files, the top/bottom layer and the raspberry pi layers. We have 2 top/bottom and 4 raspberry pi layers in our cluster.

![](img/0__tQUolf92xbjZvsIZ.jpg)

Happy hacking! Over and out!

_Originally published at_ [_kubecloud.io_](http://kubecloud.io/guide-how-to-build-a-raspberry-pi-cluster/) _on February 11, 2016._