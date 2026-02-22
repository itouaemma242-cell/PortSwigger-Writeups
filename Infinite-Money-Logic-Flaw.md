**Author:** Itoua Emmanuel (Student ID: S559970)
Level: Practitioner
**Category:** Business Logic Vulnerabilities
# **1. Lab Overview**
This lab contains a logic flaw in its purchasing workflow. The goal is to exploit this flaw to accumulate enough store credit to purchase the "Lightweight l33t leather jacket" which costs $1337.
# **2. Vulnerability Analysis**

During the reconnaissance phase, I discovered that a 30% discount coupon (SIGNUP30) could be applied to all items in the store, including $10 gift cards.

- **Gift Card Price:** $10.00
- **Price after Discount:** $7.00
- **Redemption Value:** $10.00
- **Net Profit per Transaction:** $3.00

This creates an "infinite money" loop where the user can repeatedly buy and redeem gift cards to increase their account balance.

# **3. Exploitation Strategy**

To solve this lab efficiently, I used **Burp Suite Professional/Community** to automate the process through the following steps:

**Step 1: Macro Recording**

I recorded a macro to automate the following sequence of requests:

1. POST /cart - Add a $10 gift card to the cart.

2. POST /cart/coupon - Apply the SIGNUP30 coupon code.

3. POST /cart/checkout - Complete the purchase.

4. GET /cart/order-confirmation - Extract the gift card code from the response.

5. POST /gift-card - Redeem the code to add $10 to the balance.

**Step 2: Session Handling Rules**

I configured a session handling rule to run the macro for every request sent by the Intruder. I used **custom parameter extraction** to ensure the gift card code from the confirmation page was correctly passed to the redemption request.

**Step 3: Intruder Execution**

I launched an Intruder attack using **Null Payloads** (approximately 450 iterations).

- **Resource Pool:** Limited to **1 concurrent request** to prevent race conditions or logic errors during the macro execution.

# **4. Final Result**

After the automation finished, my account balance exceeded $1337. I then purchased the jacket and successfully solved the lab.

# **5. Remediation**

The system should prevent the application of promotional discounts to the purchase of store credit or gift cards to avoid value inflation.
