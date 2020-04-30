# security-cheat-sheet

## Pricinples
- Security is everyone's responsibility
- Follow principle of least priviledge
- Enfor logging and audit trail for access control

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
- Revoke access and ssh keys when employee leaves company
- Enforce disk encryption
- Make sure employees don't publish sensitive information while releasing open source projects

## Applications
- Make sure passwords are not checked into source code repositories. Run git hook on commit to make sure sensitive data is not   committed. Refer - https://github.com/awslabs/git-secrets
- Manage secrets in a centralized location. e.g - Hasicorp Valut
- Applications always granted minimum privilege
- No sharing secrets between applications, so that one one application is compromised, the blast radius is minimum
- Make applications requst secrets from the vault dynamically rather saving credentials in their database
- Make secerts that are granted to applications ephemeral. 
  For example, if a backend api requires credentials to access database -
  a. It shoudld be given credentials to query the vault
  b. The vault will generate an ephemeral username and password granting access to only the resources the application needs
  c. The crdentials being ephemeral will be invalidated after a certain amount of time
  d. If the credentials are compromised, an operator needs to invalidate all credentails granted to the application
  e. In this way, other applications and resources are not compromised

