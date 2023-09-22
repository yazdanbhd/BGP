# BGP Protocol Configuration Between Two Routers

## Overview

The goal of this task is to implement and configure BGP protocol between two routers and create a peering session. I have used two Cisco 2691 routers in GNS3 network simulator environment.

![image](https://github.com/yazdanbhd/BGP/assets/106450709/a9fca84b-a11a-43ad-81a0-f97991819b04)



## Steps

1. First step is adding two routers to the environment. And then we connect the routers with fastethernet cable. 

2. Next we enter the first router (AS1) console and configure it. To do so we use the following command in console:

```
configure terminal
```

3. Using below command we define interface fastethernet0/0.

```
interface fa0/0
```

4. As mentioned, router interfaces are shutdown by default. To activate fa0/0 interface we use the following command:

```
no shutdown 
```

5. With below command we assign IP address 192.168.20.1 with subnet mask 255.255.255.0 to the interface:

```
ip add 192.168.20.1 255.255.255.0
```

6. Now we exit fa0/0 interface config using `exit` command. And define the next interface.

7. Using below command we enter virtual interface config named loopback10.

```
interface loopback 10
```

8. As said router interfaces are shutdown by default. To activate loopback10 interface we use: 

```
no shutdown
```

9. We assign IP address 1.1.1.1 with subnet mask 255.255.255.0 to loopback10 interface using:

```
ip add 1.1.1.1 255.255.255.0
```

10. Again we exit from loopback10 interface config using `exit` and go back to router config.

11. Using below command we enter BGP protocol config for the first router (AS1) which specifies this router will participate in BGP session:

```
router bgp 1 
```

12. With this command we specify the neighbor router which is in the BGP session. Here the neighbor is router 2 (AS2). And we specify its address:

```
neighbor 192.168.20.2 remote-as 2
```

13. Using below command we give routing process information about network 1.1.1.0 Subnet mask 255.255.255.0. So it will announce this network as a local network in BGP session. Other participating routers should be aware of this network existence:

```
network 1.1.1.0 mask 255.255.255.0
```

14. Now we exit BGP config using `exit`. And save config changes using `wr` command.

![image](https://github.com/yazdanbhd/BGP/assets/106450709/bef6f417-7d12-4480-912c-46e9ac06a80a)



15. Now we enter second router (AS2) Terminal and configure it with below commands:

```
configure terminal
interface fa0/0
no shut 
ip add 192.168.20.2 255.255.255.0
exit
interface loopback 10 
no shut
ip add 2.2.2.2 255.255.255.0
exit
router bgp 2
neighbor 192.168.20.1 remote-as 1  
network 2.2.2.0 mask 255.255.255.0
exit
wr
```

![image](https://github.com/yazdanbhd/BGP/assets/106450709/17f6db3a-d095-44f3-98a0-64ac95c59ad0)



16. Now we verify config and check BGP session establishment between routers AS1 and AS2 using:

```
show ip route
show ip bgp
```

![image](https://github.com/yazdanbhd/BGP/assets/106450709/54c662c9-4578-4dd1-816b-6982043b2efa)

