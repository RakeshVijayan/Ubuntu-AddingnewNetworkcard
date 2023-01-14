# Adding additional network card in Ubuntu 22.04

Add the network adapter as normal we follow 

Power off the Guest Os -->  Settings --> Network ( enable the 2nd adapter ) 
Power on the Guest os and check new adapter is enabled or not with 
 command    <sub> ifconfig </sub>
if not appear the new adapter automatically, then check the new adapter is detected or not by 

<sub> lshw -C network </sub>

if you noticed that the new adapter is detected but in disable state, for the next is to enable the device with the following command 

<sub> ifconfig enp0s8 up </sub>

To ensure the network adapter is up use the ifconfig command again, on that moment we will realize that the new adapter does not have ip address yet. 

To get Ip address via dhcp lease use 

<sub> dhclient command </sub>

But you will face problem again if the system reboot, you couldn't find the addpter again, to make the adapter persistent  add the new adapter entry in the following file 

/etc/netplan/file.yaml   

# This is the network config written by 'ubiquity'
```
#First interface  enp0s3
network:
  ethernets:
    enp0s3:
      dhcp4: true
  version: 2
#First interface  enp0s8
network:
  ethernets:
    enp0s8:
#dhcp4: true
      addresses:
        -  192.168.56.3/24
#routes:
#- to: default
#via: 192.168.56.1
  version: 2

```
Save and close the file and do the following command to make it persistent with out reboot the system. 

> netplan apply  

#Restarting the network demon in New ubuntu OS 

> systemctl restart systemd-networkd
