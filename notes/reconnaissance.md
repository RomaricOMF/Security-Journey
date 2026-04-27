
## 1 - Traceroute Analysis — google.com

Date: 26/04/2026
Target: google.com
Resolved IP: 172.253.124.102

### Identified Zones

Local network: hops 1 to 4, latency ranging from 2.426 to 47.576 ms
ISP network: hops 6 to 10, latency ranging from 47.567 to 70.653 ms
Target network entry point: hop 13, IP 66.249.94.90

### Security Observations

Silent routers (***): hops 23 to 30 → meaning: these routers do not respond to ICMP packets
Main latency spike: between hops 14 and 15 → probable cause: transoceanic fiber link
Filtered ports/protocols: yes/no — how do we know? ICMP traffic appears to be blocked on some routers

## 2 - nmap analysis  — [localhost](http://localhost)
Date : 27/04/2026
tool : nmap 7.95
### Found ports
- Port 7070 : service Realserver , Accessible locally but not from the internet
- Port 9050 : service tor-sock, Tor as an out node, require a configuration to be used. By default it is just used to make the traffic anonymous. 
### Serive identification on port 7070
Command used :  sudo ss -tlnp | grep 7070
Result : The port is running Anydesk service
### Conclusion on the exposition to the internet
- Public IP  : 129.0.80.198
-  Port 7070 status from internet : filtered
- Reason : The internet provider is blocking the traffic on this port
- Real risk : yes/no, why ? No since the vulnerable port is not accessible from the internet.

## 3 - nmap -sV -O — OS and Service detection

### Differences between basic scan vs -sV scan
A basic scan allow to find open ports and services running on it. a -sV scan reveals the version of these services and the other information such as the if the devise is using a cipher to encrypt messages. 

### What does the ? means
It means that the result is not precise, that further verifications are required. 

### Why -O is imprecise on the localhost
This command is used to find the OS running on the devise. In our case, this has found Linux, but the version is vague (a range). This is because 
on the localhost, nmap use a loopback interface which is not a true network interface. Nmap does not have enough signal to be precise.  

### Implications for the deceptive system
A deceptive system must avoid the ? by well configuring the devise for a good lying process. 
Such a deceptive framework has to run a SSL protocol or not depending on the service running.
Manipulate the TTL and the TCP to mimic the targeted OS. 

## 4 - Scans nmap : stealthy vs noisy

### -sT (TCP Connect): noisy
- Handshake : complete Three-ways TCP handshake (SYN --> SYN ACK --> ACK --> data)
- leave trace in the network : yes
- Sudo required : no

### -sS (SYN Stealth)
- Handshake : incomplete Three-ways handshake (SYN --> SYN ACK --> RST) (with RST, nmap ask to ignore the connection)
- leave trace in the network : no
- Sudo required : yes
### -sU (UDP)
- Mechanism : you send a message, you received an answer or not. No handshake required. The port can be either closed or open. If it is closed, the ICMP answer port unreachable. If it is open, either an answer is returned or silence. UDP is low due to this silence. nmap has to wait a timeout for each silent port. 
- Why slow on distant targets: do not have a confirmation mechanism

### Implication for the deceptive system
- How to detect the type of scan used : you can detect the type of scan by analyzing if a complete connection has been established or not.A SYN Without ACK equals a stealth scan. 
- What is the scan type's reveals on the attacker: a stealth scan reveal that the attacker is experienced and a noisy one that the scan is either automatic or the attacker is a script kiddies. 
- How do nmap knows that a port is open but not filtered: A closed port just answer unreachable port, so having any other answer will let nmap knows that the port is open. Now he can't know whether such a port is filtered or not. he can just wait for the timeout or an answer. Usually, we see open|filtered, meaning that he is not really aware of the statut, but it is one among these two. 
