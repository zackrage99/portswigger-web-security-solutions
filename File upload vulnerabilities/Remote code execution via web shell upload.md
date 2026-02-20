# Lab: Remote code execution via web shell upload

**Target URL:** `https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-web-shell-upload`

---

## 🎯 Vulnerability Overview
The application's "Edit Profile" functionality allows users to upload an avatar image. However, it fails to properly validate the file extension or content type, allowing an attacker to upload a PHP script and achieve **Remote Code Execution (RCE)**.

* **Vulnerability:** Unrestricted File Upload
* **Vulnerable Parameter:** `avatar` (POST request)
* **Impact:** Full system compromise / Access to sensitive files (`/home/carlos/secret`)

---

## 🛠️ Exploitation Steps

### 1. Identify the Upload Functionality
Log in to the lab and navigate to the **My Account** page. Locate the avatar upload field. This is our primary attack vector.

### 2. Intercept the Upload Request
1. Upload a legitimate image file (e.g., `example.jpg`).
2. Capture the `POST /my-account/avatar` request using **Burp Suite**.
3. Send the request to **Burp Repeater**.
4. ![uploaded_image.png](.portswigger-web-security-solutions/images/uploaded_image.png)

### 3. Craft the Malicious Payload
In Burp Repeater, modify the captured request to inject the PHP shell:

1.  **Change the Filename:** Update `filename="53.jpg"` to `filename="exploit.php"`.
2.  **Modify the Content-Type:** While the server accepts `image/jpeg`, we are aiming to execute PHP code. (In this specific lab, the server doesn't validate this, but it's good practice to keep an eye on it).
3.  **Inject the Shell:** Replace the binary image data in the request body with the following PHP code:
    ```php
    <?php echo file_get_contents('/home/carlos/secret'); ?>
    ```

### 4. Execute and Retrieve the Secret
1. Send the modified `POST` request. You should receive a `200 OK` or a `302 Found` response, indicating the file was accepted.
2. Follow the redirect or go back to your account page.
3. Locate the `GET` request for the avatar (usually `/files/avatars/exploit.php`).
4. View the response of this `GET` request. The server will execute the PHP code and return the contents of the secret file in the response body.

### 5. Lab Completion
Copy the secret string from the response and submit it to solve the lab.

---

## 📝 Key Takeaways
* **Lack of Server-Side Validation:** The application blindly trusts the user-supplied file extension.
* **Execution Permissions:** The `/avatars/` directory has execution permissions enabled, allowing the PHP engine to process files within it.
* **Always Check for RCE:** Any file upload field should be tested for the ability to upload and execute scripts (PHP, ASPX, JSP, etc.) based on the back-end technology.

---

