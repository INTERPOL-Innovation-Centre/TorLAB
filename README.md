# TorLAB
This project is a fork of the original [TorLAB repository](https://github.com/dws-pm/TorLAB).

## Authors and contributors

**People**<br>
 * [SebLKMa - Sebastian Lik-Keung Ma](https://github.com/SebLKMa)<br>
 * [Syknde - Shun INAGAKI](https://github.com/skynde)<br>
 * The quick reference guide is credited to Prof. Pieter Hartel, Erwin Middlesch, Christian Karam.<br>

**Organisations**<br>
 * INTERPOL Cyber Space and New Technologies Laboratory<br>
 * Dark Web Solutions - [github](https://github.com/dws-pm/) | [web](https://dws.pm/)<br>
 * [TNO Innovation for life](https://www.tno.nl/en/)<br>

# About
This repository contains the scripts and disk image files required to create and run a private network of TOR nodes. 
The disk images correspond to the SD cards required to boot a network of Raspberry Pis.
This private TOR network is used by the cyberspace and new technologies Lab to simulate TOR and deliver trainings on dark web and virtual assets.
![Alt text](TorLAB%20-%20AFP%20GETTY%20images.png?raw=true "Title")
Illustration image of the TorLAB. Source and copyright: [GettyImages](https://www.gettyimages.com/detail/news-photo/devices-to-simulate-cyber-crimes-are-displayed-at-interpol-news-photo/469556866)

# Intent
With this repository we hope to emulate cooperation in updating and maintaining this simulation environment and transition to virtual machines.

## Private network

The network of 21 Raspberry PIs (RPI) and a Linux Laptop are connected to the same local network via one or more routers.

Each node's /etc/hosts are hardcoded to know these fixed IPs as:
```
192.168.1.130   a
192.168.1.131   b
192.168.1.132   c
192.168.1.133   d
192.168.1.134   e
192.168.1.135   f
192.168.1.136   g  
192.168.1.137   h
192.168.1.138   i
192.168.1.139   j
192.168.1.140   k
192.168.1.141   l
192.168.1.142   m
192.168.1.143   n
192.168.1.144   o
192.168.1.145   p
192.168.1.146   q
192.168.1.147   r
192.168.1.148   s
192.168.1.149   t
192.168.1.150   u
```

Therefore, the respective /etc/hostname in each node has to set accordingly, e.g. "a" for node a, "b" for node b and so on. 

## The Nodes
There are 21 TOR nodes and a Linux machine.<br/>
This Linux machine is connected to the same network router and is used to start and stop all the TOR nodes' services via a shell script.<br/>
The TOR nodes run on RPI.<br/>

### Nodes and their Roles
The Roles played by the nodes are:<br/>


| Roles        | Nodes                     |
| ------ |:-----:|
| Directories  | a, b, c                   |
| Exit         | d, e, f, o, p, h          |
| Proxy        | g, n, u                   |
| Relay        | i, j, k, l, m, q, r, s, t |


## Image Files
Based on the Roles played by the nodes mentioned above, you may recreate a set of 21 SD cards from the provided image files. <br/>
These image files are direct image copies of the RPU SD cards from the nodes a, d, g, n, u, and i. <br/>
These image files were created using [win32diskimager](https://sourceforge.net/projects/win32diskimager/). <br/>
You can refer to the [instructions](https://raspberry-projects.com/pi/pi-operating-systems/win32diskimager) provided by win32diskimager.

To manage large files in GitHub, please use [GitHub LFS](https://git-lfs.github.com/)<br/> 

Each image is compressed in the rar format.

## Linux Machine Shell Script
The script `torlab-large` is used to start and stop the TOR nodes.

![Alt text](torlab-large.JPG?raw=true "Title")

For details of the various arguments this script expects, please refer to its source code.

## Power-up
(a)	The laptop should know about the IP addresses of the 21 RPI (labelled a .. u). See for details the file *20150721_Torlab_IP_MAC_SD_config.txt*<br/>
(b)	The router should know the IP and Mac addresses of the RPI. A backup of the configuration file for the router is in *20150531_DSR.cfg*<br/>
(c)	Your laptop should also know that RPI a maps to 192.168.1.130, RPI b maps to 192.168.1.131 etc.<br/>
(d)	Make sure that the RPI will not ask for a password if you ssh to them, as this will require you to enter the pi password zillions of times (use ./torlab-large sshkeys)<br/>
(e)	Make sure that the clocks of the RPI are set correctly. If the RPI are connected to the Internet, this will happen automatically. Otherwise run `./torlab-large synch`<br/>
(f)	Make sure that privoxy is configured correctly and running on the clients (g, n and u)<br/>
(g)	To start the Tor network on the laptop type (the delay is needed to allow the tor network to stabilise):<br/>
```sh
./torlab-large start
```
wait 300<br/>
```sh
./torlab-large test
```

(h)	Bitcoind and the block chain explorer must be started manually on RPI s. On the laptop do:<br/>
`ssh bitcoin@s`  (password Sl0wd0wn) <br/>
```sh
cd ~/bitcoin-testnet-box
./bitcoin.sh start
```
This takes a minute or so… Then do: <br/>
```sh
cd ~/bitcoin-abe
python -m Abe.abe --config abe-rpc.conf
```
(i)	Assuming that the displays are connected to the RPI clients g, n, and u, login as pi to each of the clients, run startx, open four terminal windows, in each window ssh as pi to an appropriate RPI, and once logged in run the arm command (no arguments needed).<br/>
A screenshot of 2 RPIs terminal running `arm`. <br/>
![Alt text](rpi_arm.JPG?raw=true "Title")

(j)	Instruct your laptop to use 192.168.1.136:8118 as a web proxy, and go to the splash page on node g at http://192.168.1.136/.<br/>
(k)	Use your web browser to visit the Wallet (login as affhous, password jolt) and check that you have a balance of about 1 BTC.<br/>
(l)	Visit the Explorer and check that there are at least 3500 blocks available from the block chain explorer.<br/>
(m)	Visit the Forum and check that you can log in as affhous (password jolt)<br/>
(n)	Visit the Market and check that you can log in as buyer affhous (password jolt, passphrase matins) and as vendor StoneIsland (password flow, passphrase wearer) and make a test purchase.<br/>
(o)	To stop the Tor network from the laptop:<br/>
```sh
./torlab-large stop 
```
(p)	To shutdown the RPI from the laptop:<br/>
`./torlab-large shutdown` (see Power-down section below) <br/>
(q)	To set all transactions on the market to complete enter the following MySQL query:<br/>
```sh
update bw_orders set progress=7
```
(r)	To set all messages to viewed enter the following MySQL query:<br/>
```sh
update bw_messages set viewed="1"
```

## Power-down
To stop the tor routers **DO NOT SHUT DOWN UNLESS YOU HAVE STOPPED THE TOR NETWORK** <br/>
`./torlab-large stop`

To shut down the RPI. **DO NOT POWER OFF UNLESS YOU HAVE SHUT DOWN THE RPIs** <br/>
`./torlab-large shutdown`
