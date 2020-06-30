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
In Reflected XSS, the malicious script comes from the current HTTP request.  
### Stored XSS  
This is also Known as Persistant XSS.
In Stored XSS, the malicious script comes from the Untrusted sources.  
### DOM-based XSS  
In DOM-based XSS, the malicious script is present in Client side itself.

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

