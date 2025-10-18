# ActiveDirectory-IAM-HomeLab

## Overview: 
Installed a Windows Server 2019 VM with Active Directory, testing user/group permissions. Configured the VM to connect to the internet, configured DHCP. Ran a Powershell Script to create users in AD. 

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

## Purpose of Lab


<!-- Learning Active Directory can be beneficial for IT and Cybersecurity professionals, system administrators, and anyone interested in network management. It provides a solid foundation for understanding the fundamentals of user and group management, authentication and authorization, and security in a networked environment. Additionally, Active Directory is widely used in many organizations, making it a valuable skill to have in the IT industry. -->
