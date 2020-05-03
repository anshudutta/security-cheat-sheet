## BurpSuite 
Web Traffic intercept tool
- Download and install BurpSuite
- Set up forward proxy in browser
  In firefox Preferences --> General --> Network Settings --> Settings
  Manual proxy configuration --> HTTP Proxy --> 127.0.0.1:8080
- Add certificates
  Download Certificate from BurpSuite.
  In firefox, Preferences --> Privacy and Security --> Certificates --> View Certificates --> Import BurpSuite Certificate
- Start BurpSuite, Proxy --> Intercept --> ON, Check Proxy --> Options
