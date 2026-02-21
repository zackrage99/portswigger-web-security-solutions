# Lab: Web shell upload via extension blacklist bypass

**Target URL:** `https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-extension-blacklist-bypass`

---

## 🎯 Vulnerability Overview
The application attempts to block malicious uploads by blacklisting specific file extensions like `.php`. However, this defense is often incomplete, as many servers are configured to execute other PHP-related extensions (e.g., `.php3`, `.php4`, `.phtml`) that may not be included in the blacklist.

* **Vulnerability:** Unrestricted File Upload (Blacklist Bypass)
* **Vulnerable Parameter:** `avatar` (POST request)
* **Impact:** Remote Code Execution (RCE) / Access to sensitive files (`/home/carlos/secret`)

---

## 🛠️ Exploitation Steps

### 1. Identify the Upload Functionality
Log in to the lab using `wiener:peter` and navigate to the **My Account** page. Locate the avatar upload field.

### 2. Discover the Extension Blacklist
1. Prepare a PHP shell: `<?php echo file_get_contents('/home/carlos/secret'); ?>`.
2. Attempt to upload the file as `exploit.php`.
3. Observe the error response: `Sorry, php files are not allowed`. This confirms the server is specifically looking for and blocking the `.php` extension.



### 3. Bypass the Blacklist
1. Capture the upload request in **Burp Suite** and send it to **Repeater**.
2. In the request body, locate the `filename` parameter.
3. Change `filename="exploit.php"` to an alternative PHP extension, such as `filename="exploit.php3"`.
4. Send the request. Notice the server returns a `200 OK`, indicating the `.php3` extension bypassed the blacklist.



### 4. Execute and Retrieve the Secret
1. Locate the GET request for your uploaded shell: `GET /files/avatars/exploit.php3`.
2. View the response. Because the server is configured to execute `.php3` files, your script runs and reveals the secret.



### 5. Lab Completion
Copy the secret string from the response and submit it to solve the lab.

---

## 📝 Key Takeaways
* **Blacklisting vs. Whitelisting:** Blacklists are inherently dangerous because they often forget to include all possible variations (e.g., `.php5`, `.phtml`, `.phar`). Whitelisting allowed extensions (like `.jpg` only) is much more secure.
* **Server Configuration:** Even if the web application tries to block certain files, the underlying server (Apache/Nginx) might still be configured to execute alternative extensions as scripts.
* **Payload Variation:** When testing file uploads, always try multiple extensions and character encodings to see what the filter might have missed.

---
*Walkthrough created for educational purposes.*
