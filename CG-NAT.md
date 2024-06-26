# What CG-NAT is, how to detect it, why it is bad, what can you do about it

## How to test if you suffer from CG-NAT
For these tests you have to be in your home network. Also make sure that you are not using a VPN or something like iCloud private realy on macOS or iOS.
### Based on your IP number
If your IP is between 100.64.0.0 - 100.127.255.255 you have CG-NAT.  
Beware, just because your IP is not in that range, does not mean that you don't have CG-NAT!  
To check what your IP is, you can use sites like https://www.whatismyip.com or https://whatismyipaddress.com or https://www.wieistmeineip.de and look for your IPv4.  

### Based on hops
You can also check if you have CG-NAT based on how many hops it takes to reach your IPv4. 
If it takes one hop, you don't have CG-NAT. 
If it takes two hops, you have CG-NAG. 

Use one of the pages above to find out what your IPv4 is.  

On Windows, start PowerShell and insert the command "tracert -4" followed by your IPv4.  
Linux or macOS can use "traceroute -I" instead.  
So if for example your IPv4 is 215.84.156.8 you issue the command:  
tracert -4 215.84.156.8

Sometimes your router also gets included in these hops. If your first hop is something like 192.168.X.X or 172.16.X.X, ignore that line and don't count it as a hop!

## Why should I care?
If you don't have a public IPv4 but a CG-NAT IPv4, you can't host any services. No VPN, no webserver nothing.  
Games will sometimes show something like "NAT strict".  
When using a CG-NAT IPv4 you share the same IP with other people. If an attacker gets blocked, you could also get blocked, because you have the same IPv4 as the attacker. 

## What is NAT?
To better understand what CG-NAT is, it helps to first understand what NAT is.  

This picture is an internet connection without CG-NAT.  
The public IPv4 is 215.84.156.8 and the local IP of your router probablly is 192.168.1.1.  
Your router has a built in DHCP server, that serves IPs to your PC, Playstation and NAS.  
In your example this is 192.168.1.2, 192.168.1.3, 192.168.1.4.  
But these IPs are only internal! If any of these devices connect to the internet, from the outside they all come with the IPv4 215.84.156.8,  
No matter if you play a game on PSN or go on reddit on your PC.  

![image](https://github.com/jameskimmel/network-stuff/assets/17176225/62b3c6d6-e402-48e3-8363-f4f65afc53bb)



Now imagine that you want to setup OpenVPN Server on your NAS (or any other service). OpenVPN uses by default port 1194.  
So you are now on the road with your moblie and want to establish a VPN connection. On your OpenVPN Client on your phone, you set the target to be your public IPv4.  
In this example that would be 215.84.156.8. Your connection goes from your phone to your router. 
Your router can not know that you want to connect to to your NAS on 192.168.1.4. Here is where NAT comes in. 
You create a NAT rule on your router, so that every traffic that arrives on port 1194 should be redirected to 192.168.1.4.  
That way your phone can establish a VPN connection to your NAS.

This has some technical limitation. The only way for your router to know where to route incoming traffic is by port. So you can't have two services on port 1194. 
If you have a VPN server on 192.168.1.2 and 192.168.1.4 and there is incoming traffic on port 1194, your router does not know where to route it. 

You also need to set a static IPv4 to your NAS so that the local IP never changes and that NAT rule all of a sudden routes to your Playstation, because your Playstation randomly got the IP 192.168.1.4. 

Some devices like the PlayStation will try to use your routers UPnP, to automatically create NAT rules for you. Then you also don't need fixed IPs. Since UPnP is a security concern, a lot of routers have it disabled by default. 

NAT can behave pretty wonky on consumer routers. 

NAT is a workaround for an old problem. In the beginning of the internet, you only had one single computer that was directly connected to your modem. There was no need for multiple IPs. When people started to have multiple devices in their homes, NAT was a hacky solution for that problem. 

BTW: That problem is solved with IPv6. Instead of getting only a single IPv4 from your ISP for all your devices, you get IPv6 prefix. That contains at least millions of IPs. 
That way every device can get its own public IPv6. 


## What is CG-NAT?
Instead of you only getting one real and public IPv4, you share a IPv4 with many other customers.  
Your ISP basically set up another router and you and many other customers are behind that. 
You get the IPv4 100.64.34.34 and your neighbor 100.64.34.33, but for the internet perspective you both have the IP 215.84.156.8. 
That is why you have two hops to 215.84.156.8 and not one.  

With CG-NAT VPN Server is impossible. Imagine your phone trying to connect to 215.84.156.8 on port 1194. The router of your ISP has no idea where to route that. To you? Or your neighbor? Unless your ISP assigns a specific port just to you (I never heard of an ISP actually doing that) the traffic stops there. That is what is often called double NAT, since there is NAT on your ISPs routers and on your router before the traffic could potentially reach you. 

![cg-NAT](https://github.com/jameskimmel/network-stuff/assets/17176225/0611202f-f8da-4a07-b8e6-3debaf92e29a)

## What can you do about it?

### Ask your ISP
Ask your ISP about a real, public IPv4. Some ISPs call that a "NAS IP" or "gaming IP".  

### Get a VPN
Some VPNs offer portforwarding. A VPN has a negative performance impact and will not be for free. 

### Use IPv6 
IPv6 does not need NAT, since all devices can get their own IP. Problem is that if you setup your OpenVPN server on IPv6 but your phone carrier has to support IPv6 too. 
IPv6 also can offer better privacy and security by obsurity. Most devices use some kind of SLAAC with privacy extension to randomly generate and use a new IPv6 inside your prefix. 
It is also is less prone to port scanning, because there are to many IPs for attackers to randomly scan. 
