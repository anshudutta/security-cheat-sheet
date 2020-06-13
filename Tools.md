## Common Vulnerability & Exposure
- https://www.cvedetails.com/
- https://cve.mitre.org/

## Enumeration
### nmap - Port Scanning
Check what ports are open
```
# -sV      ---> service version
# -O       ---> OS information
# -p-      ---> scan every port
# -A       ---> aggressive
# --script ---> script
# -v       ---> verbose
# -T5      ---> interrval
# -Pn      ---> exclude ping
# -n       ---> don't resolve dns
# -oA      ---> Output

$ nmap -sV -O -n <ip address range>
```

### Enum4Linux - SMB/Samba
Enum4linux is a tool for enumerating information from Windows and Samba systems. It attempts to offer similar functionality to enum.exe formerly available from www.bindview.com.

### Directory scanning
```
$ gobuster dir -u http://ip_address:open_port -w /path/to/list
```
### Vulnerability scanning

#### Nikto
Nikto is an Open Source (GPL) web server scanner which performs comprehensive tests against web servers for multiple items, including over 6700 potentially dangerous files/programs, checks for outdated versions of over 1250 servers, and version specific problems on over 270 servers. It also checks for server configuration items such as the presence of multiple index files, HTTP server options, and will attempt to identify installed web servers and software. Scan items and plugins are frequently updated and can be automatically updated.

```
$ nikto -id bab:bubbles -host http://10.10.250.13:1234/some/path
```

## Exploits

### Brute Force authentication

#### Hydra
Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more.
```
-t 4                    Number of parallel connections per target

-l [user]               Points to the user who's account you're trying to compromise

-P [path to dictionary] Points to the file containing the list of possible passwords

-vV                     Sets verbose mode to very verbose, shows the login+pass combination for each attempt

[machine IP]            The IP address of the target machine

ftp / protocol          Sets the protocol

$ hydra -t 4 -l <user> -P /usr/share/wordlists/rockyou.txt -vV <ip> <protocol>

$ hydra -t 4 -l <user> -P /usr/share/wordlists/rockyou.txt -vV <ip> htp-get /path/
```
### Priviledge escalation
#### Reverse shell
https://redteamtutorials.com/2018/10/24/msfvenom-cheatsheet/

#### Linpeas 
- https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite
#### LinEnum
- https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh
#### gtfobins
- https://gtfobins.github.io/

### OWASP ZAP

### BurpSuite 
Web Traffic intercept tool
- Download and install BurpSuite
- Set up forward proxy in browser
  In firefox Preferences --> General --> Network Settings --> Settings
  Manual proxy configuration --> HTTP Proxy --> 127.0.0.1:8080
- Add certificates
  Download Certificate from BurpSuite.
  In firefox, Preferences --> Privacy and Security --> Certificates --> View Certificates --> Import BurpSuite Certificate
- Start BurpSuite, Proxy --> Intercept --> ON, Check Proxy --> Options

### Metasploit
```
# Initialize database
$ msfdb init
# Check help
$ msfconsole -h
# Start console
$ msfconsole -q
# Check database status
$ db_status
# Help
$ ?
```
Commonly used Modules
- Exploit     Holds all of the exploit code we will use
- Payload     Contains the various bits of shellcode we send to have executed following exploitation
- Encoder     Obfuscating payloads
- NOP         Buffer Overflow/ ROP attacks
- Auxillary   Scanning and verification machines are exploitable
- Post        After exploit pillage

Scan using nmap
```
$ db_nmap -sV BOX-IP
# list all hosts saved after scanning
$ hosts
# List all services
$ services
# List all scanned vulnerabilities
$ vuln
```
Module
```
# Search
$ search multi/handler
$ use 6

# use a module
$ use <modulename>
```
