**Quick Brief:**
- XSS is a web security vulnerability that allows attackers to inject malicious scripts into web pages viewed by other users
- Three Main Types:

    1.Reflected XSS

    2.Stored XSS
  
    3.DOM-based XSS


**What we're testing:** Any search or comment parameter or a sink

**Why it might be vulnerable:** 
1.Websites don't properly validate/sanitize user input
2.Malicious scripts execute in victims' browsers
3.Attacker can steal data, hijack sessions, or deface websites

**What xss exploits:** Browser automatically excutes java script code in user browser


**URL:** https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded
## Steps:
1. Open the lab
2. Go and view any post you find a comment section maybe we can inject a script in it
3. in the comment bar type `<script>alert('1')</script>`
4. When you return to the post every time you reload the page the pop up window with alert 1 will show this is stored xss
5. Lab solved

## Notes:
- The payload we used is just a simple java script code to alert us with number 1
- For various types of attacks we can use malicious payloads to steal cookies and data


## How to Fix (For Developers)
Primary Defenses:
1. Input validation (whitelist allowed characters)
2. Output encoding (HTML, JavaScript, URL encoding)
3. Content Security Policy (CSP) headers
4. HttpOnly cookies (prevent JavaScript cookie access)
5. Framework protections (React, Angular, Vue auto-escape by default)




