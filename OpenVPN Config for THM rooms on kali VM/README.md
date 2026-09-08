# OpenVPN Config for THM rooms on kali VM

Step 1 be done on both kali or host/normal system.

**DO IT ON KALI**

## Step 1
- Login to your dashboard 
- Click on the profile pic in the top right corner of the screen 

![step-1](/OpenVPN%20Config%20for%20THM%20rooms%20on%20kali%20VM/image/THM-vpn-conn-1.png) 

- A drop down menu will appear click on manage account 
- Click on "VM and vpn setting"

![step-2](/OpenVPN%20Config%20for%20THM%20rooms%20on%20kali%20VM/image/THM-vpn-conn-2.png) 

- Scroll down till you see the vpn-configuration page 
- We want OpenVPN 

![step-3](/OpenVPN%20Config%20for%20THM%20rooms%20on%20kali%20VM/image/THM-vpn-conn-3.png)  

- So click on configuration files
- Select "normal room"
- Download the configuration file

![step-4](/OpenVPN%20Config%20for%20THM%20rooms%20on%20kali%20VM/image/THM-vpn-conn-4.png)  

#### IF  YOU DID THIS ON YOUR HOST SYSTEM FIND COPY IT TO KALi USING THE **VIRTUAL BOX DRAG AND DROP FEATURE** if that fails repeat **Step 1**  in the kail VM
## Step 2
**Now in Kali Linux**

- Now open a terminal 
```
 cd Downloads
```
- Assuming it was downloaded there (mine was, find yours)
```
 sudo apt update 
```
```
 sudo apt install OpenVPN
```
```
sudo OpenVPN <the-config-file you just downloaded>
```

wait for it to  finish running 
pay attention to the last few line when it is done running  look for "initialization sequence complete"

then run 

```
ip a
```

- look for the interface **tun** that is the vpn ip your kali has been given
- To confirm return back to the manage account page and click on the refresh button next to openvpn 
