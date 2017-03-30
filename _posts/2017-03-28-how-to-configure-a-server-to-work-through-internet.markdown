---
layout: default
modal-id: 4
date: 2017-03-28
img: safe.png
alt: img-alt
project-date: 2017

---

This guide is tested on Ubuntu 16.04

#### Setup local ssh server
Install **ssh server** (sshd)
```
sudo apt-get install openssh-server
```

In order to get **sshd** to start on boot for a systemd system, you need to:
```
sudo systemctl enable ssh.socket
```

At this point, it is possible to access your server from other computer inside the same network with the server, with following command:
```
ssh <username>@<local_ip>
```

with `<local_ip>` is the IP address of the server, which can be obtained by:
```
ifconfig
```

then looking for **inet addr** of the active (Ethernet) device from response in the terminal, e.g. `192.168.x.x`.

Also from the above response, you should look for **HWaddr**, e.g. `xx:xx:xx:xx:xx:xx` and keep it for later use. It is actually your physical MAC address.

In order to connect by **ssh** to your server from outside of your network through internet, it is necessary to forward your port **22** to your router. 

#### Port Forwarding
Connect to your router using web browser, then look for the **port forwarding** setting. Normally, it is inside the *Advance* or equivalent tab.

Using the **IP address** or **MAC address** collected before in setup to allow port forwarding to your server. In the port part, remember to set it as **22** for **ssh** communication.

Now, browse to the website [http://whatismyip.org/](http://whatismyip.org/) to look for Internet IP address of your router. Using this, you now can **ssh** to your server by:
```
ssh <username>@<internet_router_ip>
```

However, the internet IP address of your router can change at any time, depending on the policy of your Internet ISP. So it is better to set up a Dynamic DNS service allowing you to link your home IP address to a memorable address.

#### Dynamic DNS
What do you need to set up the DDNS for your home network? Following is the list of them, not so long:
1. A DDNS host: 
2. A router with DDNS support: Please note that some routers only support some DDNS service providers, so please check it carefully.

or

1. A DDNS host:
2. A local update client: If your router doesn't support DDNS, you have to go for this option.

So how to configure DDNS?
1. Create a DDNS account at a chosen host. For example, at [Dynu](https://www.dynu.com/en-US/ControlPanel/CreateAccount), get into [Control Panel](https://www.dynu.com/en-US/ControlPanel) and choose the **DDNS services** then **Add** to create a new DDNS service.
2. Configure your router with the DDNS service you have just created.

or 

1. Create a DDNS account at a chosen host.
2. Configure a PC-based updater: If you choose Dynu as your DDNS host, you can use **ddclient** as the updater, which can install by:
```
sudo apt-get install ddclient
```

#### Reference  

(1) [How to forward port on your router](https://www.howtogeek.com/66214/how-to-forward-ports-on-your-router/)  
(2) [How to set Dynamic DNS for your home network](https://www.howtogeek.com/66438/how-to-easily-access-your-home-network-from-anywhere-with-ddns/)

