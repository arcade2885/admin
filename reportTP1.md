# TP1 Report - Installation and Discovery of Proxmox

Authors: Baptiste and Rayan  
Group: 13

## Network diagram and reference images

The network architecture used in the lab is shown below:

<img width="640" height="491" alt="image" src="https://github.com/user-attachments/assets/8e077ffa-29e8-4e20-88e4-59ac50eda751" />

## Steps of the lab

### 1 Connection to Proxmox

The first step was to connect to the Proxmox interface of the course infrastructure:

- URL: https://hyp-01.lab.ephec-ti.be
- username: stu13
- default password: Test1234!

we change the password right after login

### 2 Firewall installation

The firewall was started first. 

The firewall network configuration used:

- stuWan/net0 : 172.31.90.13/24
- gateway: 172.31.90.254
- DNS: 172.31.90.254

- stuMgm/net1 : 172.31.80.213/24

- stuVmNet net2 To OPT1

<img width="409" height="307" alt="IMG_20260928_152413" src="https://github.com/user-attachments/assets/26395d79-753d-424c-acc4-f8fe24a0f1dd" />

### 3 Pfsense VM's LAN config

I logged on the firewall using the jump server rdp

and logged on Pfsense using the default user/password

I made Vlan 130 using those setting : 
<img width="825" height="521" alt="Screenshot 2026-10-02 102243" src="https://github.com/user-attachments/assets/dd7ff7a8-2f73-4ea5-ae48-5160c6f08168" />

I then added the int OPT2:

<img width="747" height="222" alt="Screenshot 2026-10-02 102407" src="https://github.com/user-attachments/assets/be5dfea1-5c86-4b0b-b3c6-d57222d6c6a0" />

and gave it an IP :

<img width="1082" height="510" alt="Screenshot 2026-10-02 102543" src="https://github.com/user-attachments/assets/a5d5f8be-3ff1-4768-9b48-0b062074a7e0" />

created the allow any/any rule:

<img width="1148" height="232" alt="Screenshot 2026-10-02 102707" src="https://github.com/user-attachments/assets/bdfd35c4-6961-4f26-a9da-5882792e7993" />

and enabled DHCP on the interface :

<img width="1173" height="512" alt="Screenshot 2026-10-02 102742" src="https://github.com/user-attachments/assets/abe56b24-bed5-450b-ae16-e9517316952f" />

we tested the connectivity by pinging www.google.com :

<img width="1072" height="326" alt="image" src="https://github.com/user-attachments/assets/f4ba9eff-8f8c-4f53-a323-9005bb3896a2" />


### 4 Proxmox setup 

I started the hyps1301 vm

i noted the interfaces :
<img width="648" height="128" alt="Screenshot 2026-10-08 142216" src="https://github.com/user-attachments/assets/f82f80bf-a82d-4469-8b00-2f25c731bd57" />

i then installed proxmos following the gui instruction
chosing the 8giB disk :
<img width="285" height="42" alt="Screenshot 2026-10-08 142356" src="https://github.com/user-attachments/assets/8d0424fe-ff19-45f2-b311-120c1deb0cde" />

here are the networks setting used: 
<img width="351" height="187" alt="Screenshot 2026-10-08 142735" src="https://github.com/user-attachments/assets/7752675f-f915-425c-bb88-e23aeeacef2d" />

summary:

<img width="577" height="351" alt="Screenshot 2026-10-08 142752" src="https://github.com/user-attachments/assets/4c286673-95c3-4291-af64-6f608a71edd5" />
<img width="373" height="261" alt="Screenshot 2026-10-08 143236" src="https://github.com/user-attachments/assets/68bb6a77-51fc-4730-92bb-a4e1ead8f6b5" />

### 5 linux Bridge

I went and created a linux bridge in proxmox

<img width="745" height="342" alt="Screenshot 2026-10-08 152716" src="https://github.com/user-attachments/assets/3ebadc14-6d19-49bc-b6bb-61bf88799fde" />

and applied the changes
result :
<img width="872" height="263" alt="Screenshot 2026-10-08 152902" src="https://github.com/user-attachments/assets/306cd935-6570-44f2-aeb4-90c81c7afca9" />


### 6 vms

i first created a ZFS Volume in RAID 1


<img width="987" height="540" alt="Screenshot 2026-10-08 161242" src="https://github.com/user-attachments/assets/9ee8db75-63de-4580-a74b-28641cad09f7" />

then using the info in the pdf i tried dowloading the iso provided in the pdf 

<img width="745" height="236" alt="Screenshot 2026-10-08 161450" src="https://github.com/user-attachments/assets/7aae7636-32b8-45f3-bcbd-c5952ec7aa78" />
<img width="988" height="151" alt="Screenshot 2026-10-08 161840" src="https://github.com/user-attachments/assets/8da8a36b-c278-46c5-8538-1c80afd97c2c" />

but it failled because the link didn't exist

so i looked for the updated link

https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-13.7.0-amd64-netinst.iso

and tried again

<img width="737" height="227" alt="Screenshot 2026-10-08 161824" src="https://github.com/user-attachments/assets/45247855-7e45-41d2-b4e9-5887aaa31c61" />
<img width="1328" height="52" alt="Screenshot 2026-10-08 161830" src="https://github.com/user-attachments/assets/3478cb93-0266-4545-b1b5-652a69b0f2fc" />

this time it worked

validation of my proxmox hyp :

i made a temp vlan interface : 


<img width="1231" height="35" alt="Screenshot 2026-10-08 162524" src="https://github.com/user-attachments/assets/acc5587e-57e6-4852-84a5-535725cd3f69" />

pinged 10.13.0.254

<img width="487" height="191" alt="image" src="https://github.com/user-attachments/assets/bf58aec0-cf01-463a-b1b9-3c0056a739fb" />


after i disable the int by removing it's ip adress
<img width="1170" height="30" alt="image" src="https://github.com/user-attachments/assets/60f8e8bf-aafc-438f-a905-c1a7d2d8e82b" />

### vm deployment :

<img width="932" height="210" alt="image" src="https://github.com/user-attachments/assets/cf610363-04ba-4d8e-ae65-3d01df543912" />
<img width="747" height="245" alt="image" src="https://github.com/user-attachments/assets/a9580cc2-0e8b-470e-a221-f4941955476f" />
<img width="935" height="167" alt="image" src="https://github.com/user-attachments/assets/8f5f643a-52bf-4147-a800-b5dbeaf8d5f3" />
<img width="742" height="446" alt="image" src="https://github.com/user-attachments/assets/940aa557-5002-4fb8-a819-09b5a081afa7" />



