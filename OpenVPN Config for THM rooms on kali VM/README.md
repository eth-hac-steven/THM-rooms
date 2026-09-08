# OpenVPN Config for THM rooms on kali VM

Step 1 be done on both kali or host/normal system.

**DO IT ON KALI**

## Step 1
- Login to your dashboard 
- Click on the profile pic in the top right corner of the screen 
- A drop down menu will appear click on manage account 
- Click on "VM and vpn setting"
- Scroll down till you see the vpn-configuration page 
- We want OpenVPN 
- So click on configuration files
- Select "normal room"
- Download the configuration file 
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
