# Lab: OS command injection, simple case

**Target URL:** `https://portswigger.net/web-security/os-command-injection/lab-simple`

---

## 🎯 Vulnerability Overview
The application's stock check functionality is vulnerable to **OS Command Injection**. It takes user-supplied inputs (`productId` and `storeId`) and incorporates them into a shell command to query the database. Because the input is not sanitized, an attacker can use command separators to execute arbitrary system commands.

* **Vulnerability:** OS Command Injection
* **Vulnerable Parameter:** `storeId` (POST request)
* **Impact:** Full system compromise / Ability to execute commands as the web server user.

---

## 🛠️ Exploitation Steps

### 1. Identify the Entry Point
Navigate to a product page and locate the **"Check stock"** button. This feature sends a POST request to the server, making it our primary entry point for injection.

### 2. Intercept the Request
1. Open **Burp Suite** and ensure Interception is active.
2. Click the **Check stock** button on the page.
3. Capture the `POST /product/stock` request and send it to **Burp Repeater**.
4. Observe the request body: `productId=3&storeId=2`.

### 3. Test for Command Injection
We need to determine if we can break out of the original command using a separator (like `;`, `&`, or `|`).

1. **Initial Test:** Changing the parameter to a string (e.g., `storeId=ls`) may fail if the backend script expects an integer.
2. **Injecting a Separator:** Append a second command using a semicolon (`;`).
3. **Payload:** Modify the body to:
   ```bash
   productId=3&storeId=2;ls

    Observe Response: If the response contains file names (like .sh files) alongside the stock level, the injection is successful.

4. Solve the Lab

To complete the lab requirements, we must execute the whoami command to retrieve the current user's name.

    In Burp Repeater, modify the storeId parameter:
    Bash

    productId=3&storeId=2;whoami

    Send the request.

    The response will display the username (e.g., peter-XXXXX), confirming Remote Code Execution (RCE).

📝 Key Takeaways

    Untrusted Input in Shells: Never pass user-supplied data directly to system shell functions (like system() or exec()).

    Command Separators: Linux-based systems often use ;, &&, or | to chain commands. Identifying which one the server accepts is key to exploitation.

    Input Validation: Use strict whitelisting. If a field expects a "Store ID," the application should only permit numeric characters.
