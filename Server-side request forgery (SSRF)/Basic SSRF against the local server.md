### Lab: Basic SSRF against the local server

Target URL: https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost

### 🎯  Vulnerability Overview

The application features a "Check Stock" function that fetches data from an internal system using a URL provided in a POST request. Because the server does not validate this URL, an attacker can modify it to make requests to the local loopback interface (127.0.0.1 or localhost), gaining access to administrative interfaces that are normally restricted to external users.

    Vulnerability: Basic SSRF (Server-Side Request Forgery)

    Vulnerable Parameter: stockApi (POST request)

    Impact: Unauthorized access to the admin panel and the ability to delete users.

### 🛠️  Exploitation Steps

1. ### Identify the SSRF Entry Point

    Navigate to a product page and click the "Check stock" button.

    Intercept the request in Burp Suite and send the POST /product/stock request to Repeater.

    Observe the stockApi parameter:
    stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D1%26storeId%3D1

2. ### Test for Localhost Access

    In Repeater, change the stockApi value to target the local loopback:
    stockApi=http://localhost

    Send the request. A 200 OK response indicates the server is vulnerable and successfully queried its own local interface.

    Search the response for "Admin" to find the link to the administrative panel: /admin.

3. ### Access the Admin Panel

    Update the stockApi parameter to point to the admin path:
    stockApi=http://localhost/admin

    Send the request. The response will now render the internal Admin Panel, which is usually hidden from external traffic.

    Locate the deletion link for the user carlos:
    /admin/delete?username=carlos

4. ### Execute the Unauthorized Action

    Modify the stockApi parameter to perform the deletion:
    stockApi=http://localhost/admin/delete?username=carlos

    Send the request. The server will execute this request locally, bypassing any external authentication checks.

5. ### Lab Completion

The user carlos is successfully deleted. Refresh the page or check the lab status to confirm it is solved.

###  📝 Key Takeaways

### The Trust Illusion

Developers often assume that services running on localhost or internal IPs are safe and do not require authentication. SSRF exploits this misplaced trust by making the vulnerable server act as a proxy for the attacker.
### Bypassing Network Defenses

SSRF allows attackers to bypass firewalls, ACLs, and other network protections that block external access to internal administrative tools. Because the request originates from the server itself, it is treated as "trusted" traffic.
### Security Best Practices

    Whitelisting: Only allow the application to connect to a predefined list of trusted hostnames and IPs.

    Deny Local Access: Block requests to 127.0.0.1, localhost, and private IP ranges (e.g., 10.0.0.0/8, 192.168.0.0/16) unless strictly necessary.

    Input Validation: Do not allow users to submit full URLs; instead, require a specific ID that the server maps to a hardcoded internal URL.
