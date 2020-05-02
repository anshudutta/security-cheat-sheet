# security-cheat-sheet

## Pricinples
- Security is everyone's responsibility.
- Follow principle of least priviledge.
- Enforce Role Based Access Control with logging and audit trail.
- Encrypt everything

## Data Centres

## Employees
- Enforce least privilege
- Enforce strong password policy
- Enforce 2FA
- Discourage password sharing
- Enforce password rotation
- Enforce logging and audit trail
- Enforce use of passphrase in SSH keys
- Enforce rotation of ssh keys
- Revoke access tokens and ssh keys when employee leaves company
- Enforce disk encryption
- Enforce employees to save their individual credentils using a tool like LastPass or 1Password
- Make sure employees don't publish sensitive information while releasing open source projects

## Applications
### Source code
- Make sure passwords are not checked into source code repositories. 
  Run git hook on commit to make sure sensitive data is not committed. 
  Refer - https://github.com/awslabs/git-secrets
- Dependency scanning - Ensure package vulnerability scanning and automatic upgrade
### Managing Secerts
- Manage secrets in a centralized location. e.g - Hasicorp Valut
- Enforce rotation of master key
### Applications
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
#### Secret leakage
- Review code to ensure secrets aren't leaked via
  - Application logs
  - Error or diagnostic Reports
  - Build logs
#### OWASP
- Always test applications for OWASP top 10 vulnerabilities

## Infrastructure
#### DDOS protection

  
