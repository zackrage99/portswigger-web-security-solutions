Lab: Blind OS command injection with output redirection

Target URL: https://portswigger.net/web-security/os-command-injection/lab-output-redirection
🎯 Vulnerability Overview

The application's "Submit Feedback" feature is vulnerable to Blind OS Command Injection. While the application does not return command output directly in the HTTP response, we can redirect that output to a file within the web server's writable directory and then retrieve it.

    Vulnerability: Blind OS Command Injection (Output Redirection)

    Vulnerable Parameter: email (POST request)

    Writable Directory: /var/www/images/

    Impact: Execution of arbitrary commands and unauthorized data exfiltration.

🛠️ Exploitation Steps
1. Identify the Entry Point and Writable Folder

Navigate to the "Submit feedback" page. From the lab description, we know the server has a writable folder located at /var/www/images. This is crucial because it allows us to save the results of our injected commands into a file we can later access via the browser.
2. Intercept and Modify the Request

    Fill out the feedback form with dummy data.

    Intercept the POST /feedback/submit request using Burp Suite.

    Send the request to Repeater.

    Experiment with command separators. While ; or & might be filtered, the || (OR) operator is often effective.

3. Inject the Command and Redirect Output

To solve the lab, we need to run whoami and save the result to a file in the images directory.

    Payload: Modify the email parameter to inject the command.
    Bash

    email=example@gmail.com||whoami+>+/var/www/images/output.txt||

    (Note: Ensure spaces are URL-encoded as + and the > character is handled correctly.)

    Send the request. You should receive a 200 OK response. Even though you don't see "peter" or "root" yet, the file output.txt has now been created on the server.

![Uploaded Image](https://github.com/zackrage99/portswigger-web-security-solutions/blob/main/images/OS%20command%20injection/lab3/injecting%20payload.png)

4. Retrieve the Command Output

Now, we need to read the file we just created. The website displays product images by calling a script: GET /image?filename=65.jpg. We can exploit this to read our text file.

    Find an image on the home page, right-click, and select Inspect (or check your Burp HTTP history).

    Modify the filename parameter to point to your new file:
    GET /image?filename=output.txt

    The server will treat your text file as an "image" and return the raw text content in the HTTP response.

5. Lab Completion

Once you view the response for output.txt, you will see the username of the application user (e.g., peter). The lab will be marked as solved.

📝 Key Takeaways

    Writable Directories: In blind injection scenarios, finding a writable web directory (like /var/www/images/ or /var/www/html/static/) is a common way to "exfiltrate" data.

    Parameter Manipulation: The same vulnerability that allows a site to display an image (?filename=) can often be used to read any file the web server has access to.

    URL Encoding: Always use Ctrl+U in Burp to URL-encode your payloads to ensure special characters like > and || aren't misinterpreted by the web server.
