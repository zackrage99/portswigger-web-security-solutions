# Lab: Blind OS command injection with time delays

**Target URL:** `https://portswigger.net/web-security/os-command-injection/lab-blind-time-delays`

---

## 🎯 Vulnerability Overview
The application's "Submit Feedback" feature is vulnerable to **Blind OS Command Injection**. While the application does not return the output of the command in its HTTP response, it processes user-supplied data in a way that allows the execution of arbitrary operating system commands. We can verify this vulnerability by triggering a time delay.

* **Vulnerability:** Blind OS Command Injection (Time-based)
* **Vulnerable Parameter:** `email` (POST request)
* **Impact:** Full system compromise and execution of unauthorized commands on the server.

---

## 🛠️ Exploitation Steps

### 1. Identify the Entry Point
Navigate to the website and locate the **"Submit feedback"** page. This form requires a name, email, subject, and message. Since these fields are processed by the backend, they serve as potential injection points.

### 2. Intercept the Request
1. Fill out the feedback form with dummy data.
2. Open **Burp Suite** and ensure Intercept is active.
3. Submit the form and capture the `POST /submit-feedback` request.
4. Send the request to **Burp Repeater**.

### 3. Test for Blind Command Injection
In a blind scenario, `ls` or `whoami` won't show results in the response. We must use a command that causes a visible side effect, such as a **time delay**.

1. **Initial Attempts:** Testing with standard separators like `;` or `&&` may not trigger a noticeable change if the syntax of the underlying shell command is strict.
2. **Injecting the Time Delay:** Use the `||` (OR) operator followed by the `sleep` command. This ensures that even if the first part of the command fails, the `sleep` command will execute.
3. **Payload:** Modify the `email` parameter (or any other field) to include the delay:
   ```bash
   email=foobar123@gmail.com||sleep+10||

(Note: Ensure spaces are URL-encoded as + or %20).
4. Confirm the Vulnerability

    Send the modified request in Burp Repeater.

    Observe the "Response time" in the bottom right corner of the Burp Repeater tab.

    If the response takes approximately 10 seconds to return, the server has successfully executed the sleep 10 command.
![Uploaded Image](https://github.com/zackrage99/portswigger-web-security-solutions/blob/main/images/OS%20command%20injection/lab2/injecting%20payload.png)

5. Lab Completion

Once the 10-second delay is confirmed, the lab is marked as solved, as you have proven the ability to execute arbitrary OS commands on the server.

📝 Key Takeaways

    Blind Vulnerabilities: Just because you can't see the output doesn't mean the server isn't vulnerable. Always test for "out-of-band" or "time-based" effects.

    Separators Matter: If ; doesn't work, try |, ||, &, or &&. Different backend shell environments (sh, bash, cmd.exe) handle these differently.

    Encoding: Always remember to URL-encode special characters (like &, |, and spaces) when manually modifying requests in Burp Suite to ensure the server parses the command correctly.
