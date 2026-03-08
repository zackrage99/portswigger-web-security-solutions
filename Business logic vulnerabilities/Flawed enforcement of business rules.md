قال Gemini

My apologies. I see exactly where I missed the mark—literally. I was putting the marks at the end of the lines instead of wrapping the text properly. Let's fix this and ensure the formatting is exactly how you want it for your GitHub documentation.
## Lab Walkthrough: Multi-Step Coupon Reliance ##
## 🎯 Objective ##

Exploit a business logic flaw in the coupon application process to purchase the Lightweight "l33t" Leather Jacket by repeatedly applying different discount codes to reduce the price.
## 🛠️ Tools Used ##

    Browser (Integrated with PortSwigger Labs)

    Burp Suite (Proxy & Repeater)

## 📖 Step-by-Step Guide ##
## 1. Setup and Authentication ##

    Launch the lab and open Burp Suite.

    Log in with the provided credentials:

        Username: wiener

        Password: peter

    Note the banner on the home page: New customers use code at checkout: NEWCUST5.

## 2. Initial Analysis and Testing ##

    Go to the Lightweight "l33t" Leather Jacket page and click Add to cart.

    Navigate to the Cart and apply the coupon NEWCUST5.

    Observe that a small discount is applied.

    Testing Logic: Attempting to change the quantity to a negative value (e.g., -1) in the POST /cart request does not work here.

## 3. Discovering Additional Coupons ##

    Return to the Home page and scroll to the bottom.

    Locate the Sign up to our newsletter! section.

    Enter an email address to register.

    You will receive a new coupon code: SIGNUP30.

## 4. Intercept and Repeat Coupon Application ##

    Go back to your Cart.

    Apply the SIGNUP30 coupon.

    In Burp Suite, go to Proxy > HTTP history and find the POST /cart/coupon request.

    Right-click the request and select Send to Repeater.

    In Repeater, notice the request body contains the CSRF token and the coupon code:

        csrf=[token]&coupon=SIGNUP30

    Change the coupon code to the previous one:

        csrf=[token]&coupon=NEWCUST5

    Click Send. You should receive a 302 Found response.

## 5. Manipulate the Total Price ##

    Switch back and forth in Repeater, alternating between SIGNUP30 and NEWCUST5.

    Each time you apply a different code, the system accepts it and adds an additional discount layer to the total.

    Refresh your Cart in the browser periodically to check the total price.

    Repeat this process until the price of the jacket drops below your remaining store credit.

## 6. Solve the Lab ##

    Once the price is sufficiently low, click Place Order.

    The order will process, and the lab banner will update to: Congratulations, you solved the lab!

## 💡 Key Takeaway ##

Logic flaws often occur when developers assume a specific sequence of events. In this case, the application failed to check if multiple coupons should be mutually exclusive. Always ensure that state transitions are validated against the current session state on the server.
