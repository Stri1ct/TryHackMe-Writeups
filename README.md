# TryHackMe - Neighbour Write-up

Hey everyone! Here’s my quick walkthrough for the "Neighbour" room on TryHackMe. This room is a perfect example of why you should always check the source code and how Insecure Direct Object Reference (IDOR) vulnerabilities work.

## 1. Nmap Scan
I kicked things off with a standard Nmap scan to see what we're dealing with:
`nmap 10.114.143.83`

<img width="544" height="185" alt="بحث الايبي" src="https://github.com/user-attachments/assets/993f7239-72d7-4879-9c02-671050ef495b" />


As you can see, we have SSH (port 22) and HTTP (port 80) open. Since I didn't have any credentials for SSH yet, I headed straight to the web server on port 80.

## 2. Web Enumeration
Going to the target's IP address in the browser gave me a super simple login page. 

<img width="579" height="381" alt="تسجيل الدخول" src="https://github.com/user-attachments/assets/1b8b482b-9c2c-45bd-af34-4060cbf8cdfa" />


I tried a few basic bypasses, but decided to check the page source (`Ctrl+U`) first to see if the developer left anything behind.

## 3. Source Code & Information Disclosure
Bingo! Looking at the HTML source code, the developer left a massive hint in the comments. 

 <img width="961" height="62" alt="ملاحظات منسية" src="https://github.com/user-attachments/assets/63d6def5-28a6-4046-8166-fafba6bb91f0" />


The comment gave me valid guest credentials (`guest:guest`) and specifically mentioned that the "admin" account is off limits. Now I know exactly who my target is.

## 4. Exploiting IDOR & Getting the Flag
I logged in using the guest credentials. Once I was in, the URL looked like this:
`http://10.114.143.83/profile.php?user=guest`

Since the web application is pulling the profile based on the `user` parameter in the URL, I decided to test for an IDOR vulnerability. I simply changed `user=guest` to `user=admin` in the address bar and hit enter.

<img width="1918" height="297" alt="الفلاق مقفل" src="https://github.com/user-attachments/assets/341421da-7f8e-4f48-a164-1460d4004753" />


And that’s it! The application didn't verify if I actually had the authorization to view the admin profile. It just loaded it up and handed me the final flag.
