# Nmap
## Host Dicovery
#### Host discovery using ARP
- To discover live hosts without port scanning:
```
nmap -sn Targets
```
- To perfom an ARP scan without port scanning(PR indicates you only want an ARP scan) :
```
nmap -PR -sn Targets
```

#### Host discovery using ICMP
- To use ICMP to discover live hosts, use:
```
nmap -PE Targets
```
- To use ICMP Timestamps(ICMP Type 13) to scan for live hosts:
```
nmap -PP Targets
```
- To use ICMP Address mask Queries(ICMP Type 17) to scan for live hosts:
```
nmap -PM Targets
```

#### Host discovery using TCP and UDP
###### TCP SYN Ping
- If you want Nmap to use TCP SYN ping, you can do so via the option **-PS** 
followed by the port number, range, list, or a combination of them.
```
nmap -PS Targets
```
```
nmap -PS21 Targets 
```
- Privileged users (root and sudoers) can send TCP SYN packets and don’t 
need to complete the TCP 3-way handshake even if the port is open.

#### TCP ACK Ping
- This sends a packet with an ACK flag set. To do so:
```
nmap -PA Targets
```
```
nmap -PA21 Targets
```

#### UDP Ping
- Sending a UDP packet to an open packet is not expected to lead to any reply.
- However, if we send a UDP packet to a closed UDP port, we expect to get an ICMP unreachable packet, indicating target system is up and reachable.
```
nmap -PU Targets
```
```
nmap -PU 53 Targets
```

#### Using Reverse DNS lookup
- The default behaviour is to use reverse-dns online hosts. Use **-n** flag to skip this step.
- Use the **-R** flag to query the dns server even for offline hosts.
- To use a specific dns server, use the **--dns-servers DNS_SERVER** flag.

## TCP and UDP Ports
1. Open - indicates service is listening on the port
2. Closed - indicates no service listening on that port even though its accessible.(reachable and not blocked by firewall)
3. Filtered - indicates nmap can't determine if port is open or closed since its not accessible.i.e. firewall blocks the requests or responses
4. Unfiltered - indicates that nmap can't determine if port is open or closed, although the port is accessible.(encountered when using an ACK scan -sA)
5. Open/Filtered - This means that nmap can't determine whether the port is open or filtered.
6. Closed/filtered - nmap can't determine is port is closed or filtered.

### TCP Flags
#### TCP Connect Scan
- TCP connect scan works by completing the TCP 3-way handshake, thus establishing the TCP port is open.
```
nmap -sT Targets
```
- A closed tcp port responds with an RST/ACK flag indicating it is not open.
- The **-r** option can also be added to scan the ports in consecutive order instead of random order.
#### TCP SYN Scan
- SYN scan does not need to complete the TCP 3-way handshake; instead, it tears down the connection once it receives a response from the server.
- Once SYN/ACK flag is received from server, connection is torn by sending an RST/ACK flag to the server thus establishing the TCP port is open.
- This decreases the chances of the scan being logged, as a connection wasn't established
```
nmap -sS Targets
```

### UDP Scans
- Doesn't require any handshake for connection establishment.
- If a UDP packet is sent to a closed port, an ICMP port unreachable error (type 3, code 3) is returned.
- You can select UDP scan using the **-sU** scan, moreover you can combine it with another TCP scan.
```
nmap -sU Targets
```

### Fine Tuning and perfomance
- To specify ports to scan, use the **-p** flag.e.g.
```
-p20,21,22                   //scans port 20, 21 and 22
```
```
-p20-40						//scans from port 20 to 40
```
```
-p-							//scans all ports
```
```
-F 							//scans for most common 100 ports
```
```
--top-ports 10 				//scans for ten most common ports
```
- To control the scan timing, use **T1-T5**, with **T1** being slowest(paranoid) and **T5** being fastest.
1. TO - paranoid
2. T1 - sneaky
3. T2 - polite
4. T3 - normal
5. T4 - aggressive
6. T5 - insane

- To control packet rate use **--min-rate <number>** or **--max-rate <number>**.e.g.
```
--min-rate 10 				//send min. of 10 packets per second
```
```
--max-rate 10 				//send max. of 10 packets per second
```
- To control probing parallelism, use **--min-parallelism <number>** or **--max-parallelism <number>**
- This specifies the number of such probes that can be run in parallel.

## Advance Port Scans
- 

