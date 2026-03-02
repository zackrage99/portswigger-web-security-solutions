# Lab: Basic SSRF against another back-end system

Target URL: https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-backend-system

🎯 ### Vulnerability Overview

In this scenario, the application’s "Check Stock" feature communicates with a back-end system that sits on a different IP address within the internal network. Since the stockApi parameter is not restricted, we can use the main web server as a proxy to scan the internal 192.168.0.X range and discover other internal services.

    Vulnerability: Basic SSRF against another back-end system

    Vulnerable Parameter: stockApi (POST request)

    Impact: Internal network scanning, discovery of hidden services, and unauthorized administrative actions.

🛠️ ### Exploitation Steps

1. ### Identify the Entry Point

    Browse the website: Similar to the previous lab, the entry point is the "Check stock" functionality.

    Intercept the Request: Capture the POST request in Burp Suite and send it to Repeater.

    Analyze the body: Observe the stockApi parameter:
    stockApi=http%3A%2F%2F192.168.0.1%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D5%26storeId%3D2
    The use of an internal IP (192.168.0.1) suggests the web server is talking to a different back-end machine.

2. ### Analyze the Payload

    URL Decode: Press Ctrl+Shift+U to decode the value.

    Test the Base URL: Change the payload to stockApi=http://192.168.0.1:8080/.

    Observe Response: You will likely receive an HTTP/2 400 Bad Request.

    Test for Other Hosts: Changing the last octet manually (e.g., 192.168.0.2) might return a 500 Internal Server Error, indicating that specific IP is not responding or doesn't exist.

3. ### Automate the Internal Network Scan

Since we don't know which IP in the 192.168.0.0/24 range hosts the admin panel, we must automate the search:

    Send to Intruder: Right-click the request and send it to Burp Intruder.

    Set Payload Position: Highlight the last octet of the IP address (e.g., the 1 in 192.168.0.1) and click "Add §".
    stockApi=http://192.168.0.§1§:8080/admin

    Configure Payloads: * Payload type: Numbers

        From: 1

        To: 255

        Step: 1

    Start Attack: Click "Start attack".

4. ### Access the Admin Panel

    Filter Results: Most requests will return 400 or 500 errors. Look for a request that returns a 200 OK or a 404 Not Found (which indicates the server exists but the path is slightly different).

    Find the IP: In this lab, you will discover that IP 192.168.0.201 (or similar) returns a successful response for the /admin path.

    Verify in Repeater: Send stockApi=http://192.168.0.201:8080/admin to Repeater. Search the response for "carlos" to find the deletion link:
    /admin/delete?username=carlos

5. ### Execute and Solve

    Final Injection: Combine the discovered internal IP with the deletion command:
    stockApi=http://192.168.0.201:8080/admin/delete?username=carlos

    Confirm Deletion: Send the request. The user carlos is deleted on the back-end system.

📝 ### Key Takeaways

### Internal Network Discovery

This lab shows that SSRF isn't just for attacking the local machine (localhost). It can be used to map out the entire internal network (Internal Port Scanning) that is not accessible from the public internet.
### The Trust Illusion

Back-end systems often have weaker security than front-facing ones. Developers frequently assume that if a request comes from an internal IP (like the web server), it is safe and authenticated, which is why we didn't need a password to access the admin panel.
