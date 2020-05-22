## Enumeration
### nmap - Port Scanning
Check what ports are open

### Enum4Linux - SMB/Samba
Enum4linux is a tool for enumerating information from Windows and Samba systems. It attempts to offer similar functionality to enum.exe formerly available from www.bindview.com.

## Vulnerability scanning

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

### Nikito

### Metasploit

## Brute Force authentication

### Hydra

## Priviledge escalation
### Linpeas 
- https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite
### LinEnum
- https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh
### gtfobins
- https://gtfobins.github.io/
