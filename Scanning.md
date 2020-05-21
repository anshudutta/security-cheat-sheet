# Penetration Testing

## Vulnerability Indentification

### Port Scanning
#### nmap
```bash
# -sV - service version
# -O - OS information
$ nmap -sV -O -n <ip address>
# Scan IP range
$ nmap 10.73.31.1-100
# Scan subnet
$ nmap 10.73.31.1/24
# Decoy
$ nmap -D RND:10 10.73.31.145
```
#### Nmap Scripting Engine
```
$ ls /user/share/nmap/scripts -lah
# OR
$ brew info nmap
$ ls -lah /path/to/install/share.nmap/scripts
$ nmap --script=<script_name> <ip address>
OR Run all script against open ports on traget
$ nmap -sC 192.168.1.1
```
#### Zombie Scan
TCP 3-way handshake
```
# Host A --> syn (seq =100) --> Host B
# Host B --> syn,ack (seq=300, ack=101) --> Host A
# HOST A --> ack (seq=101,ack=301) --> Host B
```
Approach
```
# Every sent packet has a unique ipid and incremented by one
# Attacker wants to know ipid of zombie host
# Attacker --> SYN-ACK ---> Zombie
# Zombie --> RST (ipid) --> Attacker
# Attacker --> SYN (sourceip: Zombie) --> Target

# If attack port is open
# Target --> SYN-ACK --> Zombie
# Zobie --> RST (ipid+1) --> Target
# Attacker --> SYN-ACK --> Zombie
# Zombie --> RST (ipid+2) --> Attacker
# Attacker checks ipid = ipid+2 and knows traget has open port

# If attack port is closed
# Target --> RST --> Zombie
# Attacker --> SYN-ACK --> Zombie
# Zombie --> RST (ipid+1) --> attacker
```
Locate a system which has ipidseq incremental (the zombie)
```
$ nmap --script=ipidseq 192.168.1.0-255
# Host script results:
# |_ipidseq: Incremental!
# Zombie --> 192.168.199.132, Target --> 192.168.199.130
$ nmap -sI 192.168.199.132 -Pn 192.168.199.130
```
#### Stealth Scan
```
# Attack --> SYN --> Target
# Target --> SYN/ACK --> Attack (port open)
# Attack --> RST --> Target
# OR
# Attack --> SYN --> Target
# Target --> RST --> Attack (port closed)
$ nmap -sS <ip_address>
```

### Directory scanning
```bash
$ gobuster dir -u http://ip_address:open_port -w /path/to/list
```
### Banner Grab

### Spidering

