# Lab: Web shell upload via Content-Type restriction bypass

**Target URL:** `https://portswigger.net/web-security/file-upload/lab-file-upload-content-type-restriction-bypass`

---

## 🎯 Vulnerability Overview
This application attempts to prevent malicious uploads by verifying the `Content-Type` header of the uploaded file. However, because this header is provided by the user (client-side), it can be easily spoofed using tools like Burp Suite to bypass the filter and achieve **Remote Code Execution (RCE)**.

* **Vulnerability:** Weak File Upload Validation (MIME-type bypass)
* **Vulnerable Parameter:** `avatar` (POST request)
* **Impact:** Arbitrary code execution / Retrieval of sensitive files (`/home/carlos/secret`)

---

## 🛠️ Exploitation Steps

### 1. Identify the Filter
1. Log in and attempt to upload a basic PHP shell (e.g., `exploit.php`).
2. Observe the error message: 
   `Sorry, file type application/octet-stream is not allowed. Only image/jpeg and image/png are allowed.`
3. This confirms the server is checking the **MIME type** provided in the request.
4. ![Uploaded Image](https://github.com/zackrage99/portswigger-web-securitysolutions/blob/main/images/File%20upload%20vulnerabilities/lab2/uploading%20a%20shell.png)

### 2. Capture a Valid Upload
1. Upload a legitimate image file (e.g., `test.jpg`).
2. Capture the `POST /my-account/avatar` request in **Burp Suite**.
3. Notice that for a successful upload, the header is set to: `Content-Type: image/jpeg`.

### 3. Bypass the Restriction
1. Send your original malicious `exploit.php` upload request to **Burp Repeater**.
2. Locate the `Content-Type` header associated with the file part of the request. It will likely say `application/octet-stream` .
3. **Manually change** this value to: `Content-Type: image/jpeg`.
4. Ensure your PHP payload is in the body:
   ```php
   <?php echo file_get_contents('/home/carlos/secret'); ?>
5. ![Uploaded Image](https://github.com/zackrage99/portswigger-web-security-solutions/blob/main/images/File%20upload%20vulnerabilities/lab2/editing%20content%20type.png)
