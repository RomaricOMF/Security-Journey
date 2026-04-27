
## Traceroute Analysis — google.com

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

## nmap analysis  — [localhost](http://localhost)
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

## nmap -sV -O — OS and Service detection

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
