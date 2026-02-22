# Lab: Web shell upload via obfuscated file extension

**Target URL:** `https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-obfuscated-file-extension`

---

## 🎯 Vulnerability Overview
The application's avatar upload feature attempts to restrict users to `JPG` and `PNG` files. However, it is vulnerable to **Null Byte Injection** ($%00$). This allows an attacker to bypass extension filters by appending a trusted extension after a null byte, tricking the validation logic while ensuring the underlying file system saves the file as a functional PHP script.

* **Vulnerability:** Web shell upload via Null Byte Injection
* **Vulnerable Parameter:** `filename` in `avatar` upload (POST request)
* **Impact:** Remote Code Execution (RCE) / Access to sensitive files (`/home/carlos/secret`)

---

## 🛠️ Exploitation Steps

### 1. Identify the Upload Restriction
Log in to the lab using `wiener:peter` and navigate to **My Account**. Try to upload a standard `exploit.php` file. 

The server rejects the request with: 
`Sorry, only JPG & PNG files are allowed` 

This indicates that while `Content-Type` might be checked, there is a strong filter on the file extension itself.

### 2. Intercept and Test Bypass Techniques
1. Capture the `POST /my-account/avatar` request in **Burp Suite**.
2. Send the request to **Burp Repeater**.
3. Attempt to bypass by changing the `Content-Type` to `image/jpeg`. If the server still returns an error, the filter is likely checking the filename extension.



### 3. Craft the Obfuscated Payload
In Burp Repeater, modify the `Content-Disposition` header to use a null byte:

1.  **Modify the Filename:** Change `filename="exploit.php"` to `filename="exploit.php%00.jpg"`.
2.  **Inject the Shell:** Replace the body content with the PHP payload:
    ```php
    <?php echo file_get_contents('/home/carlos/secret'); ?>
    ```
3.  **Send the Request:** You should receive a `200 OK` response.
4.  ![Secret Reaveal](https://github.com/zackrage99/portswigger-web-security-solutions/blob/main/images/File%20upload%20vulnerabilities/lab5/payload.png)

> **Why this works:** The validation logic sees the filename ends in `.jpg` and approves it. However, when the server's filesystem (often written in C-based languages) saves the file, it treats the null byte ($%00$ or $\0$) as the end of the string, saving the file literally as `exploit.php`.

### 4. Execute and Retrieve the Secret
1. Go back to your account page or check the Burp history to find the `GET` request for your uploaded avatar.
2. Note that the file is likely stored as `/files/avatars/exploit.php`.
3. View the response of this `GET` request. The server will execute the PHP code and return the secret key.

### 5. Lab Completion
Copy the secret string from the response and submit it to solve the lab.

---

## 📝 Key Takeaways
* **Null Byte Injection:** High-level languages (like PHP) may pass filenames to lower-level C functions. Since C uses null bytes to terminate strings, everything after the `%00` is discarded during the file-save process.
* **Validation Flaws:** Relying on simple string matches (like `endsWith(".jpg")`) is insecure if the underlying OS handles strings differently than the application code.
* **Security Mitigation:** To prevent this, servers should strip null bytes from user input, use a whitelist of allowed characters, and re-generate filenames randomly upon storage.

---
