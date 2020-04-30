## Penetration Testing

### Port Scanning
```bash
$ nmap -sV -O -n <ip address>
```
### Directory scanning
```bash
$ gobuster dir -u http://ip_address:open_port -w /path/to/list
```
### Man-In-Middel Attack
- Download and install BurpSuite
- Set up forward proxy in browser
  In firefox Preferences --> General --> Network Settings --> Settings
  Manual proxy configuration --> HTTP Proxy --> 127.0.0.1:8080
- Add certificates
  Download Certificate from BurpSuite.
  In firefox, Preferences --> Privacy and Security --> Certificates --> View Certificates --> Import BurpSuite Certificate
- Start BurpSuite, Proxy --> Intercept --> ON, Check Proxy --> Options

#### Spidering


#### Dictionary Attack
- Sniper: allows a single payload tried against every input field you select
- Battering Ram: tries a wordlist across all selected inputs at the same time
- Pitch Fork: Has 2 wordlists, matches first word with first/second/.. field in 2nd list
- Cluster Bomb: tries every combination of the the selected wordlists.
