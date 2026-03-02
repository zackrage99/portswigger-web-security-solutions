### Lab: SSRF with blacklist-based input filter

Target URL: https://portswigger.net/web-security/ssrf/lab-ssrf-with-blacklist-filter

 ### 🎯 Vulnerability Overview

In this scenario, the application has implemented a security blacklist to prevent SSRF attacks. It blocks common keywords like 127.0.0.1 and admin. To bypass these controls, we must use alternative IP representations and obfuscation techniques like double URL encoding.

    Vulnerability: SSRF with blacklist-based input filter

    Vulnerable Parameter: stockApi (POST request)

    Impact: Unauthorized access to administrative functions by bypassing weak security filters.

### 🛠️  Exploitation Steps

1. ### Identify the Entry Point

    Browse the website: Observe the application. The only functionality that appears to interact with other servers is the "Check stock" feature.

    Intercept the Request: Click "Check stock" and capture the POST request in Burp Suite. Send it to Repeater.

    Analyze the body: Observe the stockApi parameter.
    stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D19%26storeId%3D1

2. ### Test for Filter Restrictions

    URL Decode: Press Ctrl+Shift+U to see the plain URL.

    Test Localhost: Try to replace the stock URL with http://localhost.

    Observe Block: You will receive a 400 Bad Request with the message: "External stock check blocked for security reasons". This confirms a blacklist is active.

3. ### Bypass the IP Blacklist

Applications often block 127.0.0.1 or localhost specifically. We can try alternative representations:

    Try 127.1: Modify the payload to stockApi=http://127.1.

    Success: This returns a 200 OK.

    Locate Admin: Search the response for "admin". You will find the link to the administrative interface: /admin.

![Injecting Shell](https://github.com/zackrage99/portswigger-web-security-solutions/blob/main/images/Server-side%20request%20forgery%20(SSRF)/lab3/admin%20page.png)

4. ### Bypass the Keyword Filter (Double Encoding)

    Test Admin Path: Try stockApi=http://127.1/admin.

    Observe Block: You get the same "blocked for security reasons" error. The word "admin" is blacklisted.

    Single Encoding: Try to encode the first letter: stockApi=http://127.1/%61dmin. This is also blocked.

    Double Encoding: The application likely decodes the input once before checking the blacklist. By double encoding the letter 'a' (%25%36%31), we bypass the filter.
    stockApi=http://127.1/%25%36%31dmin

    Access Granted: You receive a 200 OK and can now see the Admin Panel.

![Injecting Shell](https://github.com/zackrage99/portswigger-web-security-solutions/blob/main/images/Server-side%20request%20forgery%20(SSRF)/lab3/double%20encode.png)

6. ### Execute and Solve

    Find Deletion Link: Search the response for "carlos" to find the deletion URL: /admin/delete?username=carlos.

    Final Payload: Apply the double encoding to the admin portion of the delete path:
    stockApi=http://127.1/%25%36%31dmin/delete?username=carlos

    Confirm Deletion: Send the request. The user carlos is deleted, and the lab is marked as solved.

### 📝  Key Takeaways

### The Weakness of Blacklists

This lab demonstrates that blacklists are often incomplete or easily bypassed. If a developer only blocks 127.0.0.1 and admin, an attacker can use alternative IP formats (like 127.1, decimal, or octal) or URL encoding to hide the forbidden keywords.
### Double Encoding Attack

When an application decodes user input multiple times (or the web server decodes it before passing it to the application logic), double encoding can be used to "hide" malicious strings from security filters that only decode the input once.
### Security Best Practices

    Whitelisting over Blacklisting: Instead of blocking "bad" IPs, only allow "good" IPs.

    Consistent Encoding: Ensure that input is validated after all decoding processes have been completed.
