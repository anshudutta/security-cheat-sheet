# security-cheat-sheet

## Pricinples
- Security is everyone's responsibility.
- Follow principle of least priviledge.
- Enforce Role Based Access Control with logging and audit trail.
- Encrypt everything

## Employees
- Enforce least privilege
- Enforce strong password policy
- Enforce 2FA
- Discourage password sharing
- Enforce password rotation
- Enforce logging and audit trail
- Enforce use of passphrase in SSH keys
- Enforce rotation of ssh keys
- Revoke access tokens and ssh keys when employees leave company
- Enforce disk encryption
- Manage individual credentils using a tool like LastPass or 1Password
- Make sure employees don't inadvertently reveal sensitive information through 
  - social media 
  - open source projects
- Encrypt emails containing sensitive information

## Applications
### Source code
- Make sure passwords are not checked into source code repositories. 
  Run git hook on commit to make sure sensitive data is not committed. 
  Refer - https://github.com/awslabs/git-secrets
- Dependency scanning - Ensure package vulnerability scanning and automatic upgrade
- Add Security.md file which details security management and escalation
### Development
- Always test applications for OWASP top 10 vulnerabilities
- Enforce Secure Software developmnet Life Cycle
  - MS Security Development Lifecycle (MS SDL)
  - NIST 800-64
### Managing Secerts
- Manage secrets in a centralized location. e.g - Hasicorp Valut
- Enforce rotation of master key
### Accessing Secrets
- Applications always granted minimum privilege
- No sharing secrets between applications, so that one one application is compromised, the blast radius is minimum
- Make applications requst secrets from the vault dynamically rather saving credentials in their database
- Make secerts that are granted to applications ephemeral. 
  For example, if a backend api requires credentials to access database -
  - It shoudld be given credentials to query the vault
  - The vault will generate an ephemeral username and password granting access to only the resources the application needs
  - The crdentials being ephemeral will be invalidated after a certain amount of time
  - If the credentials are compromised, an operator needs to invalidate all credentails granted to the application
  - In this way, other applications and resources are not compromised
- If the aapplication needs to persist sensitive information, it needs to be encrypted. The encryption key can be queried       from the vault.
### Secret leakage
- Review code to ensure secrets aren't leaked via
  - Application logs
  - Error or diagnostic Reports
  - Build logs
### Containers
#### Docker
- Verify authenticity when downloading images - valid checksums, download over HTTPS, valid GPG keys
- Always use private registry to store your images unless you want them to be public
- Pin versions for images, DO NOT use `latest`
- Pin versions for packages installed on the images
- Never run as root, Use `USER` directive to specify a user when running containers
- Use `gosu` NOT `sudo`
- Use minimal base images e.g. alpine
- DO NOT run `ssh` inside your container
- If neeed you can run containers in `priviledged` mode
- Always specify capabilities by using the `--add-cap` flag to give specific capabilities to the docker container
- Always use an image linter to lint DOckerfile. e.g - https://github.com/hadolint/hadolint
- Always scan images for vulnerabilities before pushing into registry. eg - https://github.com/aquasecurity/trivy

### Kubernetes
#### Users Accounts
- Create user groups like - administrators, developers, etc
- Use K8's RBAC to implement fine grained control for user groups
#### Pods
- Run pods on least priviledged mode controlled by RBAC
- Use Pods `securityContext` to set `rusAsUser` where necessary for all containers inside the pod
- Use container's `securityContext` to limit prvililedges for that container
- Use `capabilities` to limit priviledges
#### Service
- Use ingress controllers to define ingress into the controllers, make sure only required number of ports are open for ingress traffic
- Make services `ClusterIp` by default
#### Network Policies
- Use network policies to defined how pods accept ingress and egress traffic.
  For example - ALLOW DB pod to accept traffic from Backend API only
#### Service Mesh
Use a service mesh to impletment ZERO TRUST NETWORK
- Mutual TLS
- Network Policy (implemented at edge of the service mesh that defines which pods and talk to what)
- Egress traffic Monitoring (Enforce strict policy on directing traffic leaving the cluster to be forwarded via a proxy)

## Infrastructure
### Data Centres
### Cloud
### Servers
- Disable root login
- Use sudo to run commands that require root priviledges. Avoid using `su - root` or `sudo` to gain root shell. 
  This is for better audit trail on who ran what command
- Disable ssh root logins
- Use one service account per service (do not share service accounts across services)
- Deny login, ssh from service accounts
- Periodically review and delete unused accounts
- Enforce 2FA with google auth provider or RSA key for login
- Use LDAP as login provider
- Protect against single user mode attack or bootloader side loading
- Encrypt disk
- Regularly update OS packages
### DDOS protection
### WAF

## Reporting and Escalation


  
