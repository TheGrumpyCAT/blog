---
title: "Introduction to xss"
date: 2020-06-30T19:51:49+05:30
draft: false
---

Cross-Site Scripting (XSS) is a vulnerability where an attacker can execute script in client side.Normally it lets an attacker to make user's action or steal user's data.  
In this post I am going to give some introduction to xss vulnerability , why it works and how to mitigate this vulnerability.  
Now, let's consider a simple website which has some content and comment section.  
&nbsp;

![Website](/introductiontoxss/website.png)  
&nbsp;

We have a text box where we can enter comments. Whatever comment we enter in the text box added to the comment section.  
What if we enter some html elements in the place of comment. For example, if some user enters  
```"<b> bold Comment</b>"  ```
in the place of comment. the comment appears as follows  
&nbsp;

![Bold Comment](/introductiontoxss/bold_comment.png)  
&nbsp;

The comment appears in bold format.This might not look like a big problem until someone inserts a comment like this.  
``` <img src=1 onerror="alert(1)"/>```
&nbsp;

![Alert Dialog](/introductiontoxss/alert.png)  
&nbsp;

The script got executed instead of this entire comment getting added as text.When an attacker can execute javascript on client side it becomes a very big problem.Like, someone can steal cookies which leads to account hijack.  
&nbsp;

*Cookies are small files stored in browser's directory which help in remembering login and keeping track of movement within the site*  

&nbsp;

Thus Cross-site scripting enables the attacker to perform javascript in the website .

### Why this works ?  
When you place some HTML element (which is intended to be text ) inside the website, the browser's parser starts parsing it as HTML and executes the onerror attribute when there is an error in image's source.  

### Types
There are three main type of XSS.  
1. Reflected XSS  
2. Stored XSS  
3. DOM-based XSS

### Reflected XSS  
In Reflected XSS, the malicious script comes from the current HTTP request.The attacker's payload is a part of request that is sent to the web server.The Web server includes the data in the immidiate response in an unsafe way.  
Example :  
Let's consider the user makes request to some webpage (say, `https://myinsecurewebsite.com?message=hello)` and the server responds with " &lt;p&gt; the message is  hello&lt;/P&gt;"  
If the application doesn't process the data then we can send  malicious content in message parameter.  
`https://myinsecurewebsite.com?message=<script> malicious content ... </script>`

### Stored XSS  
This is also Known as Persistant XSS.
In Stored XSS, the malicious script comes from the Untrusted sources.
this one is quite dangerous than other types of xss since it affects everyone who visits the website.  
The Example used in blog was an example of stored xss since the comment usually gets stored in the database and everyone who sees the comment section will be affected .    
### DOM-based XSS  
In DOM-based XSS, the malicious script is present in Client side itself.This is similar to reflected xss but the different is that the malicious content is injected in client side itself and no request is made in server.  
Example :  
`https://myinsecurewebsite.com?language=English`  
If the developer places the content of language parameter  inside the DOM without processing, then it might cause some truoble if user gives url like,  
`https://myinsecurewebsite.com?language=<script>alert(document.cookie)</script>`

### What problems can XSS cause ?  
* The attacker can basically impersonate as any user who become victim of this vulnerability in this website.  
* Read or perform action that the user can do.   
* Can Deface website by replacing the website's content with their own content.
* If a user who has a higher privilege is affected by XSS, then the attacker can affect other user's data.

### How to mitigate ?
We can mitigate XSS by combining the below procedures.
1. Filter Input data on arrival.We can check if the input lies within the expected values.
2. Encoding data on output.(Requires combination of HTML, CSS, JS and URL encoding ).
3. Implementing Content Security Policy. This can help in reducing the impact of XSS. 

