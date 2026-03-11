# Lab Walkthrough: High-Level Logic Vulnerability

## Objective

Exploit a business logic flaw to purchase the **Lightweight "l33t" Leather Jacket** (priced at $1337.00) using only $100 store credit by manipulating the **quantity parameter** in the cart request.

---

## Tools Used

- Browser
- Burp Suite (Community or Professional Edition)

---

## Step-by-Step Guide

### 1. Setup and Authentication

Launch the lab and open **Burp Suite**.

Navigate to the **My Account** page and log in using the provided credentials.

Username: `wiener`  
Password: `peter`

---

### 2. Identify the Target Product

Browse the store homepage and locate the **Lightweight "l33t" Leather Jacket**.

Open the product page and observe the following:

- Jacket price: $1337.00
- Your store credit: $100

Since the jacket costs more than your available credit, we need to exploit a logic flaw to reduce the cart total.

---

### 3. Intercept the Add to Cart Request

Turn **Intercept ON** in Burp Suite.

Click **Add to cart** for the leather jacket.

The following request will be intercepted:


POST /cart


Request body:


productId=1&redir=PRODUCT&quantity=1


Right-click the request and select **Send to Repeater**.

---

### 4. Test Negative Quantity

Modify the quantity value to a negative number:


productId=1&redir=PRODUCT&quantity=-2


Send the request.

Now go back to the cart in the browser.

You will notice that the cart shows **-2 quantity**, which means the application accepts negative values. This is a strong indication of a logic vulnerability.

---

### 5. Reduce the Cart Total

Keep **one leather jacket** in the cart.

Now send another request adding a **different product with a negative quantity**.

Example:


productId=2&redir=PRODUCT&quantity=-4


Send the request.

Return to the cart and observe that the **total price decreases** because the negative item subtracts from the cart total.

This effectively reduces the price of the leather jacket.

---

### 6. Make the Jacket Affordable

Continue adding the second product with **larger negative quantities** until the total cart price drops below **$100**.

Example:


productId=2&redir=PRODUCT&quantity=-10


Adjust the value until the cart total becomes affordable with your store credit.


![payload](https://github.com/zackrage99/portswigger-web-security-solutions/blob/main/images/Business%20logic%20vulnerabilities/lab4/miniplating%20price.png)

---

### 7. Complete the Purchase

Once the cart total is below $100:

1. Go to your **Cart**
2. Click **Place Order**

The order will be processed successfully and the lab will be marked as solved.

---

## Key Takeaway

This vulnerability exists because the application fails to properly validate the **quantity parameter**.

To prevent this issue:

- Quantity values should never be allowed to be negative.
- Business logic validation must occur on the server side.
- Cart calculations should not rely on untrusted user input.

Failing to validate user input in payment workflows can lead to serious financial abuse vulnerabili
