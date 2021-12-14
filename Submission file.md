Networks Fundamentals II Homework: In a Network Far, Far Away!
Background
You are a Network Jedi working for the Resistance.

The Sith Empire recently carried out a DoS attack, taking out the Resistance's core network infrastructure, including its DNS servers.

This attack destroyed the Resistance's ability to communicate via email and retrieve other crucial information about each others' operations. The Empire has taken advantage of this compromised availability by ambushing numerous Resistance outposts, all vulnerable because they can no longer call for help.

Your task is a crucial one: Restore the Resistance's core DNS infrastructure and verify that traffic is routing as expected.

Good luck and may the Force be with you!

Files Required
Darkside pcap
Galaxy Network Map
Your Objectives:
Review each network issue in the missions below.

Document each DNS record type found.

Take note of the DNS records that can explain the reasons for the existing network issue.

Provide recommended fixes to save the Galaxy!

Submit your results and findings in a document.

Topics Covered in Your Assignments
DNS
NSLOOKUP
DNS record types
A, PTR, MX, NS, SOA, SRV, TXT
Wireless
WEP, WPA
Aircrack-ng
Wireshark Wireless analysis and decryption
Mission 1
Issue: Due to the DoS attack, the Empire took down the Resistance's DNS and primary email servers.

The Resistance's network team was able to build and deploy a new DNS server and mail server.

The new primary mail server is asltx.l.google.com and the secondary should be asltx.2.google.com.

The Resistance (starwars.com) is able to send emails but unable to receive any.

Your mission:

Determine and document the mail servers for starwars.com using NSLOOKUP.
nslookup -type=MX starwars.com

![image](https://user-images.githubusercontent.com/93474690/146053734-12415712-3e8c-42ed-9b88-eede338f86f7.png)

Explain why the Resistance isn't receiving any emails.

The Resistance isn’t receiving any emails because their MX records are not set to the correct primary and secondary mail servers as provided. asltx.l.google.com and asltx.2.google.com are not part of the MX records.

Showing as below:
starwars.com mail exchanger = 5 alt1.aspx.l.google.com.
starwars.com mail exchanger = 5 alt2.aspmx.l.google.com.
starwars.com mail exchanger = 10 aspmx3.googlemail.com.
starwars.com mail exchanger = 1 aspmx.l.google.com.
starwars.com mail exchanger = 10 aspmx2.googlemail.com.

Document what a corrected DNS record should be.

Corrected DNS should show the following:
starwars.com mail exchanger = 1 asltx.l.google.com.
starwars.com mail exchanger = 5 asltx.2.google.com.

Mission 2
Issue: Now that you've addressed the mail servers, all emails are coming through. However, users are still reporting that they haven't received mail from the theforce.net alert bulletins.

Many of the alert bulletins are being blocked or going into spam folders.

This is probably due to the fact that theforce.net changed the IP address of their mail server to 45.23.176.21 while your network was down.

These alerts are critical to identify pending attacks from the Empire.

Your mission:

Determine and document the SPF for theforce.net using NSLOOKUP.

nslookup type=TXT theforce.net

![image](https://user-images.githubusercontent.com/93474690/146053885-3b13925e-ab9e-45d9-a34c-7ee790b745f0.png)

theforce.net text = "v=spf1 a mx mx:smtp.secureserver.net include:aspmx.googlemail.com ip4:104.156.250.80 ip4:45.63.15.159 ip4:45.63.4.215"

Explain why the Force's emails are going to spam.

The Force has not updated their DNS text record to reflect the required SPF pointer to their recently changed mail server “45.23.176.21”.

Document what a corrected DNS record should be.

Corrected DNS record should be: theforce.net text = "v=spf1 a mx mx:smtp.secureserver.net include:aspmx.googlemail.com include:45-23-176-21.lightspeed.rcsntx.sbcglobal.net ip4:104.156.250.80 ip4:45.63.15.159 ip4:45.63.4.215"

![image](https://user-images.githubusercontent.com/93474690/146053985-e6327378-5479-4ab6-b4af-c27ab7a522d1.png)

Mission 3
Issue: You have successfully resolved all email issues and the resistance can now receive alert bulletins. However, the Resistance is unable to easily read the details of alert bulletins online.

They are supposed to be automatically redirected from their sub page of resistance.theforce.net to theforce.net.

Your mission:

Document how a CNAME should look by viewing the CNAME of www.theforce.net using NSLOOKUP.

nslookup -type=CNAME www.theforce.net

![image](https://user-images.githubusercontent.com/93474690/146054101-cc65be3e-5c8c-4abe-861f-782913e73452.png)

www.theforce.net canonical name = theforce.net.

Explain why the sub page of resistance.theforce.net isn't redirecting to theforce.net.

The DNS CNAME record is missing a reference from resistance.theforce.net to theforce.net

Document what a corrected DNS record should be.

Corrected DNS record should be. www.theforce.net canonical name = theforce.net. resistance.theforce.net canonical name = www.theforce.net.  

Mission 4
Issue: During the attack, it was determined that the Empire also took down the primary DNS server of princessleia.site.

Fortunately, the DNS server for princessleia.site is backed up and functioning.

However, the Resistance was unable to access this important site during the attacks and now they need you to prevent this from happening again.

The Resistance's networking team provided you with a backup DNS server of: ns2.galaxybackup.com.

Your mission

Confirm the DNS records for princessleia.site.
nslookup -type=NS princessleia.site

![image](https://user-images.githubusercontent.com/93474690/146054225-fbc21a5b-d641-429a-85bb-ec78c4c408d9.png)

Current name servers: princessleia.site nameserver = ns26.domaincontrol.com. princessleia.site nameserver = ns25.domaincontrol.com.

Document how you would fix the DNS record to prevent this issue from happening again.
Need to add a reference to the backup DNS server: princessleia.site nameserver = ns25.domaincontrol.com. princessleia.site nameserver = ns2.galaxybackup.com.  

Mission 5
Issue: The network traffic from the planet of Batuu to the planet of Jedha is very slow.

You have been provided a network map with a list of planets connected between Batuu and Jedha.

It has been determined that the slowness is due to the Empire attacking Planet N.

Your Mission:

View the Galaxy Network Map and determine the OSPF shortest path from Batuu to Jedha.

The “OSPF” shortest path from “Batuu” to “Jedha”:
D C E F J I L Q T V Jedha
1 2 1 1 1 1 6 4 2 2 2 -----> 23 hops

Confirm your path doesn't include Planet N in its route.

It’s not going through Planet N as per the above path.

Document this shortest path so it can be used by the Resistance to develop a static route to improve the traffic.

Planet Batuu → D → C → E → F → J → I → L → Q → T → V → Planet Jedha  

Mission 6
Issue: Due to all these attacks, the Resistance is determined to seek revenge for the damage the Empire has caused.

You are tasked with gathering secret information from the Dark Side network servers that can be used to launch network attacks against the Empire.

You have captured some of the Dark Side's encrypted wireless internet traffic in the following pcap: Darkside.pcap.

Your Mission:

Figure out the Dark Side's secret wireless key by using Aircrack-ng.

Hint: This is a more challenging encrypted wireless traffic using WPA.
In order to decrypt, you will need to use a wordlist (-w) such as rockyou.txt.
aircrack-ng Darkside.pcap -w /usr/share/wordlists/rockyou.txt

![image](https://user-images.githubusercontent.com/93474690/146054441-e4ffc53b-29cc-4df7-bae3-15213f095d97.png)

KEY FOUND! [ dictionary ]  
Use the Dark Side's key to decrypt the wireless traffic in Wireshark.

Hint: The format for they key to decrypt wireless is <Wireless_key>:<SSID>.
Preferences > Protocols > IEEE 802.11 > Enable Decryption > Decryption Keys > Edit > wpa-pwd key=dictionary
  
  ![image](https://user-images.githubusercontent.com/93474690/146054568-947ae0d7-58f1-409b-9646-420f076f3471.png)
  
  ![image](https://user-images.githubusercontent.com/93474690/146054614-7de458ae-a513-4407-9bec-46b0606ea9f8.png)
  
  Once you have decrypted the traffic, figure out the following Dark Side information:

Host IP Addresses and MAC Addresses by looking at the decrypted ARP traffic.
ARP Protocol Specific Addresses:
--------- IP Addresses ------------ MAC Addresses
Target’s 172.16.0.101 is at Cisco-Li_e3:e4:01 (00:13:ce:55:98:ef)
Sender’s 172.16.0.1 is at IntelCor_55:98:ef (00:0f:66:e3:e4:01)

Additional IPs of interest:
IP Addresses ---- MAC Addresses
172.16.0.9 is at 00:14:bf:0f:03:30
68.9.16.30 is at 00:0f:66:e3:e4:01 same mac as \
68.9.16.25 is at 00:0f:66:e3:e4:01 same mac as - 172.16.0.1
10.1.1.50 is at 00:0f:66:e3:e4:01 same mac as /

Document these IP and MAC Addresses, as the resistance will use these IP addresses to launch a retaliatory attack.
--------- IP Addresses --- MAC Addresses
Target’s 172.16.0.101 is at Cisco-Li_e3:e4:01 (00:13:ce:55:98:ef)
Sender’s 172.16.0.1 is at IntelCor_55:98:ef (00:0f:66:e3:e4:01)

Watch gateway:
------------- IP Addresses ---- MAC Addresses
Destination: 172.16.0.9 is at 00:14:bf:0f:03:30
Location: http://172.16.0.9:5431/dyndev/uuid:0014-bf0f-0330000099dc\r\n

  ![image](https://user-images.githubusercontent.com/93474690/146054746-04d83838-dab0-416d-8c4c-a1b80678ecad.png)
  
  ![image](https://user-images.githubusercontent.com/93474690/146054798-f8af447a-ded8-42f9-a918-5b877aeeba5e.png)
  
  ![image](https://user-images.githubusercontent.com/93474690/146054848-bc59a0fb-b2ad-4600-902a-8ded17c69e30.png)
  
  Mission 7
As a thank you for saving the galaxy, the Resistance wants to send you a secret message!

Your Mission:

View the DNS record from Mission #4.

The Resistance provided you with a hidden message in the TXT record, with several steps to follow.

Follow the steps from the TXT record.

Note: A backup option is provided in the TXT record (as a website) in case the main telnet site is unavailable
Take a screen shot of the results.
  
  ![image](https://user-images.githubusercontent.com/93474690/146054971-fcee2143-6000-4467-b104-62a3e7cf6bf4.png)
  
  princessleia.site text = "Run the following in a command line: telnet towel.blinkenlights.nl or as a backup access in a browser: www.asciimation.co.nz/#"

telnet towel.blinkenlights.nl
  
  ![image](https://user-images.githubusercontent.com/93474690/146055100-05bf68db-0ded-4f24-8652-fa3fb47f9924.png)
  
  ![image](https://user-images.githubusercontent.com/93474690/146055151-a574c698-33d1-4654-bf8c-87012bab8e23.png)
