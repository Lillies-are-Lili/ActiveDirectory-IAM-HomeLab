# ActiveDirectory-IAM-HomeLab

## Overview: 
Installed a Windows Server 2019 VM with Active Directory, testing user/group permissions. Configured the VM to connect to the internet, configured DHCP and RAS/NAT. Ran a Powershell Script to create users in AD, and configured the DC to be its own DNS. 

This repository will be detailing my experience creating this project, what I did and how I did it, and what I learned. 
Big thanks to kindsonthegenius and Abdullahi Ali, I followed their articles when doing this lab. The links to their labs are:

kindsonthegenius: https://www.kindsonthegenius.com/how-to-setup-active-directory-domain-with-virtualbox-and-join-computers-part-1/

Abdullahi Ali: https://medium.com/@aali23/how-to-build-an-active-directory-iam-home-lab-using-virtualbox-60b79b94b300

## Creating the Virtual Machine
This is my settings I configured on the VM. 

<img width="492" height="505" alt="Screenshot 2025-10-18 at 12 10 40 PM" src="https://github.com/user-attachments/assets/5c33e207-9f8d-4f12-92c1-1869a28773bc" />

I set my first network adapter for the NAT network, in order to obtain the Host IP Address from my home router so I could have access to the internet. Then I set my second network adater to be an Internal Network Adapter, in order to communicate with other virtual machines. I enabled bidirectional drag and drop, and shared clipboard. Now my VM is ready. 


## Configuring the Virtual Machine
I started up the machine and installed Guest Additions to enhance usability.

I assigned my internal network to the IP address 172.16.0.1, with subnet mask /24. I won't be assigning a default gateway, since it'll be acting as it's own default gateway. Now, when I install Active Directory, it can automatically install DNS. I'll also have the server use itself as the DNS server, with the IP address 127.0.0.1, a loopback address. 

<img width="449" height="457" alt="Screenshot 2025-10-18 at 2 01 53 PM" src="https://github.com/user-attachments/assets/309d17a2-daec-4baa-af1b-267a2d202456" />



Then I added the Active Directory Domain Services role to this server, so that this VM can be set as a Domain Controller. A domain controller is a server that manages network security and allows users to authenticate and access network resources. 
<img width="838" height="596" alt="Screenshot 2025-10-18 at 12 32 22 PM" src="https://github.com/user-attachments/assets/5cf5ff19-512f-46e0-83b6-0c63ea930bd3" />

<img width="761" height="537" alt="Screenshot 2025-10-18 at 12 33 53 PM" src="https://github.com/user-attachments/assets/20aa7f20-5b09-4e3d-915f-408ef6e071fc" />
I promoted the VM to a domain controller, adding a new forest and set the domain name as LTN.local. Configured a password for the Directory Services Restoure mode, and let the server restart to finish the promotion. 
<img width="846" height="668" alt="Screenshot 2025-10-18 at 12 42 58 PM" src="https://github.com/user-attachments/assets/623cef6f-4233-48ae-b606-9a9816e1f1ac" />

## Creating a dedicated Domain Admin Account
I went into the Active Directory Users and Computers and created an _ADMIN Organizational Unit folder. I added a new user, specifying myself.<img width="516" height="492" alt="Screenshot 2025-10-18 at 2 29 45 PM" src="https://github.com/user-attachments/assets/66c3e5a7-9de5-4d6e-84ed-b7b6b41d130e" />

And then to give myself admin privileges, I added myself to be a member of domain admins. <img width="517" height="443" alt="Screenshot 2025-10-18 at 2 31 04 PM" src="https://github.com/user-attachments/assets/fd11aae7-4853-4a8a-8d5b-66c0429bbcb0" />
I've now officially created my first admin account. Just to test it worked, I logged out and logged back int with my new credentials.<img width="530" height="415" alt="Screenshot 2025-10-18 at 2 33 17 PM" src="https://github.com/user-attachments/assets/288ff419-d323-4955-9f83-400af9063027" />

<!-- username: a-LNgo password: IloveNicole122!-->

## Installing RAS/NAT
It's important to have this installed on our DC since RAS/NAT will allow our Windows Client to be in the private network, while also still having access to the internet via our DC.

I gave the server remote access, clicked to allow routing and then installed it. 
<img width="978" height="644" alt="Screenshot 2025-10-18 at 2 46 36 PM" src="https://github.com/user-attachments/assets/5aac4273-7974-4921-a265-3ccfc18a4b30" />

Now I'm enabling routing and Remote access.
<img width="645" height="518" alt="Screenshot 2025-10-18 at 2 50 32 PM" src="https://github.com/user-attachments/assets/7edad009-3bbf-40f0-8c39-3182a44c99fc" />
And I've successfully installed RAS/NAT. My DC will act as the default gateway to my Windows Client. 

## Setting up a DHCP Server
It's important to setup a DHCP server, because it will give all of the devices connected through Active Directory an IP address, allowing them access to the internet. We won't have to manually configure our own static addresses to each individual device, saving ourselves a lot of time and effort. 

I went back into the Roles and Features Wizard, and added DHCP.<img width="536" height="510" alt="Screenshot 2025-10-18 at 2 55 26 PM" src="https://github.com/user-attachments/assets/7fff257a-899b-4ba4-b46f-05be3bcd1134" /> 
After installing, I went to the DHCP menu and added a new scope which allows me to distribute the IP addresses to devices. I named it after the IP range I configured on the Internal Network.

<img width="549" height="294" alt="Screenshot 2025-10-18 at 2 58 31 PM" src="https://github.com/user-attachments/assets/092ddc0e-1697-47cd-ae98-0ea35795db2e" />
I set the IP range from 172.16.0.100 to 172.16.0.200,
I left the lease duration the default 8 days. The lease duration is how long a device can have an IP address before they'll have to get a new IP address. Since it's a home lab, 8 days should be perfect. 

I added the IP address of my DC to be used as the default gateway for any devices connecting to it. 
<img width="667" height="514" alt="Screenshot 2025-10-18 at 3 01 58 PM" src="https://github.com/user-attachments/assets/7120301e-13fb-4897-a163-79ffd91e271d" />

my DNS was down, indicated by a downwards red arrow. I authorized and refreshed the parent server and now it is up. 
<img width="300" height="126" alt="Screenshot 2025-10-18 at 3 03 09 PM" src="https://github.com/user-attachments/assets/dbfbe370-10a6-420c-85b8-c04e37a35f71" />
DNS is now active and running. 

## Powershell Script to create Users in AD
I then grabbed a powershell script from Github user joshmadakor1, and added it to my DC. I used the command, Set-ExecutionPolicy unrestricted, since Powershell typically doesn't execute scripts due to security policy. I changed into the directory where the scripts and name.txt files were. I clicked play and the scripts ran to add all the users in the name.txt. <img width="660" height="541" alt="Screenshot 2025-10-18 at 3 19 15 PM" src="https://github.com/user-attachments/assets/99f98b83-ed50-43ff-94eb-6ece82320f21" />

I went back into the Active Directory Users and Computers and all users were automatically added. <img width="448" height="608" alt="Screenshot 2025-10-18 at 3 22 49 PM" src="https://github.com/user-attachments/assets/fd0c8b48-a7c8-402b-a115-cf376421a5cf" />

<!-- https://medium.com/@mando_elnino/setting-up-active-directory-in-virtualbox-part-2-windows-virtual-machine-9faf590f51b1

go back to article nad try and finish lab? -->
## Purpose of Lab
Active Directory is very important for those in IT or Cybersecurity. It provides centralized authentication and authorization for managing users, computers, groups, and other network resources. It's fundamental in understanding user and group management, and a vital skill to have. I wanted to learn more about Active Directory, in order to be a more skilled IT professional. 

## Lessons Learned
When first starting this lab, I used mulitple resources to figure out what is the best way to do this smoothly and efficiently. Because of that, since the resources did each of the labs differently, I ended up having to delete my VM and configure a new one 3 separate times. This slowed me down significantly, but after the second time I was able to configure the VM very fast and without instruction. While it was somewhat a bother to have to redo large chunks of the lab, I believe it reinforced important concepts for me, and I learned a lot each time I redid it.

I also learned the importance in documentation. When I messed up a few times prior, instead of really looking at the guides, I looked at my own notes to see what I was doing. Documentation is vital everywhere, especially in service desk. Whether training someone new, or re-learning yourself, documenting the steps to complete a task can save many hours of stress and avoid wasting time. 

Overall, I really enjoyed this lab and feel like I learned a lot. There's much more to Active Directory than I thought, and I feel like a stronger IT individual because of it. 
