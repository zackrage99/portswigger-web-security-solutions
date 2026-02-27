### Lab: Basic SSRF against the local server

Target URL: https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost

### 🎯  Vulnerability Overview

The application features a "Check Stock" function that fetches data from an internal system using a URL provided in a POST request. Because the server does not validate this URL, an attacker can modify it to make requests to the local loopback interface (127.0.0.1 or localhost), gaining access to administrative interfaces that are normally restricted to external users.

    Vulnerability: Basic SSRF (Server-Side Request Forgery)

    Vulnerable Parameter: stockApi (POST request)

    Impact: Unauthorized access to the admin panel and the ability to delete users.

###  🛠️ Exploitation Steps
1. ### Identify the Entry Point

To initiate an SSRF attack, you first need to find web functionality that connects to another server or uses a URL as an input.

    Observe the website: After inspecting the pages, the only place making such a connection is the "Check stock" functionality inside the "View details" page of a product.

    Intercept the Request: Open Burp Suite, click "Check stock", and capture the POST request. Send it to Repeater.

2. ### Analyze the Payload

Observe the body of the POST request:
stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D3%26storeId%3D1

    This parameter is using a URL to talk to an internal stock server.

    Try decoding the URL (Ctrl+Shift+U) and sending it. If an error occurs, it indicates the server expects a specific format.

    Testing the base URL: Changing the parameter to stockApi=http://stock.weliketoshop.net:8080 returns a 400 Bad Request, showing we can manipulate the path but need a valid endpoint.

3. ### Test for Localhost Access

Now, let's see if the website can be forced to talk to its own local server:

    Inject Localhost: Modify the parameter to stockApi=http://localhost.

    Verify Vulnerability: You should receive a 200 OK. This confirms the server is vulnerable to SSRF.

    Inspect Response: Press the "Render" tab in Burp Repeater. You will notice an "Admin panel" is now visible. Back in the Raw response, search for "admin" to find the link: /admin.

![Injecting Shell](https://github.com/zackrage99/portswigger-web-security-solutions/blob/main/images/Server-side%20request%20forgery%20(SSRF)/lab1/injecting%20payload.png)

4. ### Access the Admin Panel

    Open the Panel: Change the payload to stockApi=http://localhost/admin.

    Analyze Results: You get another 200 OK. This is a good sign—you are now viewing the internal administrative interface.

    Find the Target: Search the response for the user "carlos". You will find a link to delete him: /admin/delete?username=carlos.

5. ### Execute and Solve

    Final Injection: Combine the path to delete the user:
    stockApi=http://localhost/admin/delete?username=carlos

    Confirm Deletion: Send the request. The user carlos is deleted, and the lab is marked as solved.

### 📝  Key Takeaways
### Lack of Defenses

This lab demonstrates a server talking to a local interface with absolutely no defenses or whitelisting in place. It trusts any URL provided in the stockApi parameter.
### The Trust Illusion

We were able to act as an admin simply by using localhost. This happens because developers often assume that any request originating from the local machine is safe, authenticated, and belongs to an administrator, failing to account for request forgery.
