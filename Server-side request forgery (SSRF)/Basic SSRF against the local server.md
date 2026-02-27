Lab: Basic SSRF against the local server

Target URL: https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost
🎯 Vulnerability Overview

The application features a "Check Stock" function that fetches data from an internal system using a URL provided in a POST request. Because the server does not validate this URL, an attacker can modify it to make requests to the local loopback interface (127.0.0.1 or localhost), gaining access to administrative interfaces that are normally restricted to internal users.

    Vulnerability: Basic SSRF (Server-Side Request Forgery)

    Vulnerable Parameter: stockApi (POST request)

    Impact: Unauthorized access to the admin panel and the ability to delete users.

🛠️ Exploitation Steps
1. Identify the SSRF Entry Point

    Navigate to a product page and locate the "Check stock" button.

    Open Burp Suite and intercept the request when you click the button.

    Observe the POST /product/stock request. The body contains a parameter called stockApi with a URL-encoded value:
    stockApi=http%3A%2F%2Fstock.weliketoshop.net%3A8080%2Fproduct%2Fstock%2Fcheck%3FproductId%3D1%26storeId%3D1

2. Test for Localhost Access

    Send the request to Burp Repeater.

    Modify the stockApi parameter to point to the local server instead of the remote stock server:
    stockApi=http://localhost

    Send the request and observe the response. A 200 OK indicates that the server successfully queried itself.

    Use the Render tab in Burp or search the response HTML for "Admin". You will find a link to /admin.

3. Access the Admin Panel

    Update the payload to target the administrative interface:
    stockApi=http://localhost/admin

    In the response, you will now see the administrative dashboard, which is normally blocked for external users.

    Search the HTML for the string carlos. You will find a deletion URL:
    /admin/delete?username=carlos

4. Execute the Unauthorized Action

    Modify the stockApi parameter one last time to trigger the deletion:
    stockApi=http://localhost/admin/delete?username=carlos

    Send the request.

5. Lab Completion

The server executes the delete command internally. Since the request originates from localhost, the application assumes it is an authorized administrator. The user carlos is deleted, and the lab is marked as solved.
📝 Key Takeaways
The Trust Illusion

The developers likely assumed that the /admin path was safe because it was configured to only allow connections from the local machine. However, they failed to realize that the Stock Check functionality acts as a "proxy," allowing an external attacker to appear as a local user.
Why "localhost" works

In many web architectures, security is applied at the "edge" (the firewall or load balancer). Once a request is inside the network, internal services often trust each other without further authentication. SSRF bypasses the edge security entirely.
Remediation

    Whitelisting: The application should only allow the stockApi to connect to a specific list of approved hostnames/IPs.

    Input Validation: Never allow a user to define the protocol (http://) or the full URL. Only allow a storeId and build the URL on the backend.
