# [Project Name: Basic Routing]

## Overview
In this project we connected two local networks via a router and configuring and troubleshooting the router and switches and we also got familiar with layer 3 in TCP/IP and OSI reference model which is network layer. 


## 🎯 Learning Objectives
- Understanding routing process.
- What gateways are in networks and host configurations.
- How a switch deals with outrange packets.
- Get familiar with simulating tools like Packet Tracer.

## 🛠️ Technologies & Tools Used
- **Simulator:** Cisco Packet Tracer
- **Protocols:** IPv4, ARP, ICMP
- **Analysis Tools:** Packet Tracer Simulation Mode

## 📸 Visual Documentation

### 1. Topology Diagram
![Topology](Images/Network%20Topology.png)

*Caption: This diagram shows the overall network setup created in Packet Tracer.*

## Project Concepts
### Routers
Routers are devices that act like connectors. They connect different networks with variant net masks and ranges. Routers are likely what shape the internet by letting our packets go thorough different networks to reach their destinations. Plus layer one (Physical layer) and layer two (Data link layer) Routers can underhand and adjust layer 3 (network layer) packets and by looking at the netmask and their destination IP address , they forward a packet to one of their port. If they cannot find a sufficient port, the packet will be sent thorough default port which more likely is the internet cloud.

## Default gateway
Default gateway, is the gateway of our networks that packets with foreign destination IP address can go thorough. In most cases router is the default gateway because it can forward packets (diagrams) to different networks that it has its port connected to them.  

## 🚀 Implementation Steps
### 1.  **Topology Design:** Placed four PCs and two Cisco 2960 switches connected via a 2911 router in Packet Tracer




### 2.  **IP Configuration:** 
---
After implementation step, we assigned IP addresses to PCs. We talked about how to assign IP address in  [project 1](../Project1_Basic_Switchig/README.md).
Here are the PCs and associated IP addresses table.

|  PC |   IP address  |  switch  |
|-----|---------------|----------|
|  0  | 192.168.10.10 | switch 0 |
|  1  | 192.168.10.20 | switch 0 |
|  2  | 192.168.10.30 | switch 0 | 
|  3  | 192.168.20.10 | switch 1 | 
|  4  | 192.168.20.20 | switch 1 | 
|  5  | 192.168.20.30 | switch 1 | 

So now that we are done assigning IP addresses to our hosts, we need to assign an IP address to a router port which is basically our gateway. Since we have two local networks to connect to each other, we need to use two ports of the router to forward each individual's packets. 

Here is a summery of what port and IP we should set.

|        Port         |  IP address  | 
|---------------------|--------------|
| gigabitEthernet 0/1 | 192.168.10.1 |
| gigabitEthernet 0/2 | 192.168.20.1 | 

So let's start with the gigabitEthernet 0/1. We want this to be our gateway of network one with 192.168.10.0/24 as its range of IP address.

![router configuration 0/1](Images/Router%20config%20gig01.png)
As depicted, interface gigabitEthernet was chosen to config by command ```interface gigabitEthernet 0/1```. <br>
To assign IP address to the selected port we utilize ```ip``` Command to set the IP and netmask ->```ip address 192.168.10.1 255.255.255.0```. Then ```no shutdown``` command will turn on the port and you can see the message after.<br>
We have the same scenario for interface **gigabitEthernet 0/2** as the second network gateway.

![router configuration 0/2](Images/Router%20config%20gig02.png)




## Connection test
So now that every thing looks fine, let's see whether we have the connection or not. For the first time we check the PC0 to make sure that it has IP address then ping a PC in the foreign network like PC3 with IP **192.168.20.10**.

![Unsuccessful ping](Images/unsuccessful%20ping-PC0.png)

Ohhh man!!! we cannot ping it. What's wrong. Don't panic the key is here. Actually if you examine the default gateway in the picture above, you'll immediately notice that default gateway has not been set on our PCs. Let's fix it.<br>
To fix this issue we open the IP configuration section and check the default gateway field.

![default gateway misconfiguration](Images/defaultgateway%20misconfiguration-PC0.png)

Here it is. We need to fill this field by the IP address of our gateway that is our router port. So we set **192.168.10.1** for this PC and all the PCs that are in this network.

![default gateway configuration network 1](Images/defaul%20gateway%20confgureation.png)

Notice that this gateway is solely for this network. For our second network the default gateway IP address should be **192.168.20.1**.

![default gateway configuration network 2](Images/defualt%20gateway%20configuration-PC3.png)

Let's check the connection by the ``ping`` command again to see if the issue is fixed.

![successful ping pc0](Images/successful%20ping%20-PC0.png)

and we ping PC0 from PC3.

![successful ping pc3](Images/Successful%20ping-PC5.png)

Yesss. we did this man. Networks are now successfully connect and can talk to each other.

## 🔍 Key Findings & Results
- **Routing:** Understanding routing process. 
- **Default gateway:** Understanding  what default gateway is and how its misconfiguration can cause issus.
- **The arp process before routing:** Actually for the first time that you try to ping a foreign network, you get an icmp timeout error shown in the picture bellow.

![](Images/timeout%20-%20Copy.png)

This is because when for the first time we want to connect to another network, we should send our packets to the default gateway but we do not know what the MAC address of the router is and remember that switches only work with MAC addresses not IP addresses. to achieve that goal, first we need to find the MAC address by the ARP protocol and then we send the packets to it so that takes some time and is the reason why we see timeout message. For proof of concepts we can check the ARP table by ```ARP -a``` command before and after the ping process.

![before arp](Images/arp%20table%20before%20arp%20-PC4.png)

This is the ARP table before ping that is empty.

![After arp](Images/arp%20table%20after%20ping-PC4.png)

As the result of the ARP process, you can see that IP address of the router is added to the table.

## 🚧 Challenges & Solutions
- **Challenge:** Initially, Ping failed between the two devices due to default gateway misconfiguration. 
- **Troubleshooting:** checked the default gateway to see whether we have set on or not.
- **Solution:** Set the router IP address in the default gateway field.
- **Lesson Learned:** This challenge highlighted the critical importance of routing and default gateway.

## 🗂️ Project Files
- [Basic routing.pkt](./Basic%20routing.pkt.pkt)
- `README.md` (This file)
