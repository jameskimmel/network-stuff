# What CG-NAT is, how to detect it, why it is bad, what can you do about it

## How to test if you suffer from CG-NAT
For these tests you have to be in your home network. Also make sure that you are not using a VPN or something like iCloud private realy on macOS or iOS.
### Based on your IP number
If your IP is between 100.64.0.0 - 100.127.255.255 you have CG-NAT.  
Beware, just because your IP is not in that range, does not mean that you don't have CG-NAT!  
To check what your IP is, you can use sites like https://www.whatismyip.com or https://whatismyipaddress.com or https://www.wieistmeineip.de and look for your IPv4.  

### Based on how many hops
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
When using a CG-NAT IPv4 you share the same IP with other people. If an attacker or hacker gets blocked, you could also get blocked, because you have the same IPv4 as the attacker. 

## What is NAT?
To better understand what CG-NAT is, it helps to first understand what NAT is. 


## What is CG-NAT?

## What can you do about it?

### Ask your ISP

### Get a VPN

### Use IPv6 
