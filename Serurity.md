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
### Directory scanning
```bash
$ gobuster dir -u http://ip_address:open_port -w /path/to/list
```
### Banner Grab

### Spidering

## Exploitation
### Configuring BurpSuite - Web Traffic intercept tool
- Download and install BurpSuite
- Set up forward proxy in browser
  In firefox Preferences --> General --> Network Settings --> Settings
  Manual proxy configuration --> HTTP Proxy --> 127.0.0.1:8080
- Add certificates
  Download Certificate from BurpSuite.
  In firefox, Preferences --> Privacy and Security --> Certificates --> View Certificates --> Import BurpSuite Certificate
- Start BurpSuite, Proxy --> Intercept --> ON, Check Proxy --> Options

#### Brute fore 
##### Dictionary Attack
- Sniper: allows a single payload tried against every input field you select
- Battering Ram: tries a wordlist across all selected inputs at the same time
- Pitch Fork: Has 2 wordlists, matches first word with first/second/.. field in 2nd list
- Cluster Bomb: tries every combination of the the selected wordlists.

Example - DVWA. 
To launch a dictionary attack, 
- Open BurpSuite and load the Login page
- Right click --> Send to Intruder
- Clcik clear (Clear $) to clear highlighted field
- Select username and password and click (ADD $)
- GoTo Payloads, load payload from wordlist
- Start attack
##### Fuzzing
Fuzz testing or Fuzzing is a Black Box software testing technique, which basically consists in finding implementation bugs using malformed/semi-malformed data injection in an automated fashion.

Example - 
Consider a website that allows uploading files. To find out what kind of files are allowed, we can launch a fuzz attack using BurpSuite. 
To do this we intercept the traffic and then send a list of extensions in the payload

Click on "Payloads" ---> "Sniper" attack type.

Click the "Payloads" tab now, find the filename and "Add §" to the extension.
##### Privilege escalation
Once the attacker gains access to the target machine, he/she will try to escalate priviledge to root.

SUID (Set owner User ID upon execution) is a special type of file permissions given to a file. 
Normally in Linux/Unix when a program runs, it inherit’s access permissions from the logged in user. 
SUID is defined as giving temporary permissions to a user to run a program/file with the permissions of the file owner rather that the user who runs it. 

In simple words users will get file owner’s permissions as well as owner UID and GID when executing a file/program/command.

Example - passwd command.
This command is used to change user password and this edits, /etc/passwd and /etc/shadow (permissions only granted to root).
So passwd command is set with SUID to give root user permissions to normal user so that it can update /etc/shadow and other files.

To find all SUID files, execute (4000 is a special permission for SUID files)
```bash
$ find / -user root -perm -4000 -exec ls -ldb {} \;
```
If system is misconfigured, then you may find `systemctl` misconfigured with SUID priviledge
