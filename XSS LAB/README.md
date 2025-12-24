# Cross-Site Scripting (XSS) Lab using DVWA

###   Objective

The objective of this lab was to **understand, reproduce, and analyze Cross-Site Scripting (XSS) vulnerabilities** using a deliberately vulnerable web application.
The lab focused on how **unsanitized user input** can be exploited to inject malicious scripts that execute in a victim’s browser.

All activities were conducted in a **controlled lab environment** for **educational and ethical purposes only**.


###   Lab Environment

* Attacker Machine: Cisco Cybersecurity Virtual Machine
* Target Application: Damn Vulnerable Web Application (DVWA)
* Operating System: Linux (Virtual Machine)
* Network Type: Host-only / Internal Network

---

###  Tools Used

* DVWA (Damn Vulnerable Web Application) – vulnerable web testing platform
* Cisco Cybersecurity VM – attacker environment
* Web Browser – interaction with DVWA
* Basic HTML & JavaScript – crafting XSS payloads


###  Lab Setup

1. Launched the Cisco Cybersecurity Virtual Machine.
2. Accessed DVWA via browser using its local IP address(10.6.6.13)
3. Logged into DVWA using default credentials:

   ```
   Username: admin
   Password: password
   ```
4. Set DVWA **Security Level to Low**.  ![img.png](Resources%2FXSS%2Fimg.png)

5. Navigated to the **XSS (Reflected)** and **XSS (Stored)** modules.

---

###  XSS Type 1: Reflected Cross-Site Scripting

### Description

Reflected XSS occurs when user-supplied input is immediately reflected back to the browser without validation or encoding.

### Steps Performed

1. Navigated to:

   ```
   DVWA → Vulnerabilities → XSS (Reflected)
   ```
2. Entered the following JavaScript payload:

   ```html
   <script>alert('You have been hacked")</script>
   ```
3. Submitted the form.

### Result

* A JavaScript alert box appeared in the browser displaying the text contained in my script.
* This confirmed the presence of a reflected XSS vulnerability as the application was permited to execute a script in a text box requiring a simple string as an input. This showed that the from did not implement any input sanitization and this therefore makes the application very susceptible to XSS attacks.

![xss1.jpg](..%2FScreenshots%2Fxss1.jpg)

4. After successfully investigating the behaviour of a web application with very low security I proceeded to increase the security level of DVWA to medium.

![img_1.png](Resources%2FXSS%2Fimg_1.png)

5. I then navigated back to the reflected cross site scripting and entered the same script i entered in the first instance again. This time however, the script did not execute but instead it returned the contents of our script tags.

![img_2.png](Resources%2FXSS%2Fimg_2.png)
6. Upon further investigation of the website source code it was revealed that the website now contains a filter that prevents the execution of any script tags.

![img_3.png](Resources%2FXSS%2Fimg_3.png)

7. However after some experimentation I discoverd that simply changing the case of some of the characters in the script tage enabled us to bypass the rudimentary filter that had been implemented in this security level. This experimentation resulted in the following command.

   ```html
   <sCrIpt>alert('You have been hacked")</sCrIpt>
   ```
   
![img_4.png](Resources%2FXSS%2Fimg_4.png)

8. After successfully bypassing the medium level of security i then further increased the security level to hard. And again when i attempted to rerun the previous script i was unsuccessful because the filter had now been further modified to not permit a script tag regardless of the case as seen below from the source code.

![img_5.png](Resources%2FXSS%2Fimg_5.png)

9. In the current environment it is now not possible to run a script directly using it's tags and hence this noe required the creative use of the other html tags in order to produce the same output we got in previous cases. Therefore I decided to make us of image tags to bypass this more complex filter and the code below was run successfuly producing the ouput depicted below.

   ```html
   <img src=x onerror=alert("You_are_hacked!")>
   ```
![img_6.png](Resources%2FXSS%2Fimg_6.png)

#### This Lab helped to better understand how easy it is for threat actors to exploit poorly secured web applications as this can lead to much more serious consequences that the harmless demonstrations displayed in this lab such as giving attackers access to the host computer .


---

## XSS Type 2: Stored Cross-Site Scripting

### Description

Stored XSS occurs when malicious input is **saved on the server** and executed every time a user accesses the affected page.

### Steps Performed

1. Navigated to:

   ```
   DVWA → Vulnerabilities → XSS (Stored)
   ```
2. Entered the following payload into the message field:

   ```html
   <script>alert('This is a hack')</script>
   ```
3. Submitted the comment.

### Result

* The payload was stored in the application.
* JavaScript executed automatically on every page reload.
* This confirmed a persistent XSS vulnerability.

![img_7.png](Resources%2FXSS%2Fimg_7.png)

####  Persistent Cross-Site Scripting (XSS) is particularly dangerous because malicious scripts are stored on the server and automatically executed whenever users access the affected page. This allows attackers to continuously compromise multiple users without further interaction, leading to credential theft, session hijacking, unauthorized actions, and long-term data exposure. In real-world enterprise and public systems, persistent XSS can remain undetected for extended periods, amplifying its impact and causing severe security, operational, and reputational damage if proper input sanitization and output encoding are not enforced.



###  Key Payloads Used

   ```html
   <script>alert('You have been hacked")</script>
   ```

   ```html
   <sCrIpt>alert('You have been hacked")</sCrIpt>
   ```

   ```html
   <img src=x onerror=alert("You_are_hacked!")>
   ```


These payloads simulate how attackers inject scripts to:

* Steal session cookies
* Redirect users
* Perform actions on behalf of victims


###  Findings and Observations

* The application **did not sanitize or validate user input**.
* JavaScript code executed directly in the browser.
* Stored XSS is more dangerous than reflected XSS because:

  * It persists across sessions
  * It affects multiple users
* Low DVWA security settings intentionally expose vulnerabilities for learning.


###  Security Implications

In real-world applications, XSS vulnerabilities can lead to:

* Session hijacking
* Credential theft
* Unauthorized actions
* Website defacement

This highlights the importance of secure coding and input handling.


### Mitigation Techniques

* Validate and sanitize all user input
* Encode output using HTML entities
* Implement Content Security Policy (CSP)
* Avoid inline JavaScript
* Perform regular security testing


###  Ethical Notice

This lab was conducted **strictly for educational purposes** in a controlled environment using intentionally vulnerable software.
No unauthorized systems or networks were targeted.


###  Conclusion

This lab strengthened my understanding of **client-side vulnerabilities**, specifically Cross-Site Scripting (XSS), and demonstrated how minor coding oversights can lead to serious security risks.
It reinforced the importance of secure development practices in modern web applications.


