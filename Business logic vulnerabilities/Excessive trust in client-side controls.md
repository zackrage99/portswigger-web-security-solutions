## Lab Walkthrough: Excessive Trust in Client-Side Controls

### 🎯 Objective

Exploit a business logic flaw to purchase a **Lightweight "l33t" Leather Jacket** (normally priced at $1,337.00) for a significantly lower price by manipulating the client-side request.

## 🛠️ Tools Used

* **Browser** (Integrated with PortSwigger Labs)
* **Burp Suite Professional/Community Edition** (Proxy & Repeater)
## 📖 Step-by-Step Guide
### 1. Setup and Authentication

* Launch the lab and open **Burp Suite**.
* Navigate to the **My Account** page.
* Log in using the provided credentials:
* **Username:** wiener
* **Password:** peter
### 2. Identify the Target

* Browse the home page and locate the **Lightweight "l33t" Leather Jacket**.
* Click **View details** to open the product page.
* 
### 3. Intercept the Purchase Request

* In Burp Suite, ensure **Intercept is ON** (Proxy tab).
* On the product page, set the quantity (e.g., 3) and click **Add to cart**.
* Switch to Burp Suite to view the intercepted **POST /cart** request.
### 4. Manipulate the Price

* Look at the request body. You will notice a **price** parameter:
> productId=1&redir=PRODUCT&quantity=3&price=133700
* Right-click the request and select **Send to Repeater**.
* In the Repeater tab, modify the **price** value to a lower amount, such as **1**:
> productId=1&redir=PRODUCT&quantity=3&price=1
* Click **Send**.
### 5. Verify the Exploit

* You should receive a **302 Found** response.
* In your browser, navigate to your **Cart**.
* Notice that the price has been successfully manipulated. The jacket is now listed at a fraction of its original cost (e.g., **$0.01** or **$0.03** total).
### 6. Solve the Lab

* Click the **Place Order** button.
* The order should process successfully, and the lab banner will update to **Congratulations, you solved the lab!**
* 
### 💡 Key Takeaway

**Never trust user-supplied data for critical business logic.** Prices should be stored and validated on the server side (e.g., in a database linked to a Product ID) rather than being passed as a modifiable parameter from the client.
