# PROJECT NAME

Wireshark Malicious Traffic Analysis 

## Objective

Today I am going to install Wireshark on my Ubuntu machine. I have downloaded a pcap file from malware-traffic-analysis.net and I am going to analyze it looking for malicious activity and indicators of compromise. Please see last page for my Incident Report. 

### Skills Learned

- Netwrok Forensics
- Threat Hunting 

### Tools Used

- Wireshark
- Linux CLI
- Virus Total

## Steps

First I will need to add the stable by running the command sudo add-apt-repository ppa:wireshark-dev/stable

![image](https://github.com/user-attachments/assets/4372449e-4d28-41bc-b3db-79793f8adf31)

Next I update the repository by running command 
sudo apt-get update

![image](https://github.com/user-attachments/assets/6699af75-eeb2-46b8-8927-eebc563a46a4)

Now I’m ready to install Wireshark by running command sudo apt-get install wireshark

![image](https://github.com/user-attachments/assets/aab42aa6-8329-4841-a938-a8ac40872297)

I choose the option to allow non super user to be able to capture packets. It takes about a minute and Wireshark should be installed. I run the command sudo wireshark and perfect, its launched. 

I have already downloaded a pcap with malicious activity. A File containing alerts has also been provided.  

First thing I did was go down the alerts and look up each IP address on VirusTotal.com
IP address 79[.]124[.]78[.]197 was flagged as malicious and is associated with malware.

![image](https://github.com/user-attachments/assets/2f86bbed-21be-4b40-afe4-863a987c9d13)

This alert below is an IOC (Indicator of compromise). We can see that the victim could be infected with malware.

![image](https://github.com/user-attachments/assets/dd9e2035-5701-4965-b98a-23075ad17442)

I run the display filter ip.src== 172.17.0.99 && ip.dst== 79.124.78.197
330 packets are displayed now. To narrow it down I need to look at the UTC time of when these packets came in. I add a column that displays UTC time and look at packets that came in at 17:35 UTC time. 

So here I can see plenty of communication between 172[.]17[.]0[.]99 and malicious IP 79[.]124[.]78[.]197. I can see the victim’s MAC address is 18:3d:a2:b6:8d:c4. I need to find out the Host Name.

I use display filter dhcp so I can view the dhcp request. 

![image](https://github.com/user-attachments/assets/f1c64cc0-2cd1-4d27-bae6-dd4bce1217d7)

We can see here the Host Name is DESKTOP-RNV09AT. I make note of this for my report. 

Looking at the communication between victim and malicious IP we see the URLs that are generating the alerts.

![image](https://github.com/user-attachments/assets/ff8740e5-1e7a-4fe7-b413-5ed50daf8e01)

POST /foots.php
GET /index.php?id=&subid=qIOuKk7U
POST index.php

I now need to find out the victim’s first and last name using the display filter ldap.AttributeDescription && ip.addr== 172.17.0.99.  We see that the victims name is Andrew Fletcher. 

![image](https://github.com/user-attachments/assets/a5f39c5b-4777-4067-8c8c-030de1715c32)

![image](https://github.com/user-attachments/assets/1b698484-7aa2-46a9-b5c3-69c5df45955d)

End of Lab
