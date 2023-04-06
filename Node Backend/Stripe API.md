# Can you demonstrate how you would create an API endpoint using Node.js and the Stripe API, and use it to charge a user's credit card for a specific amount?

```typescript
const stripe = require('stripe')('sk_test_...'); // Replace with your own Stripe secret key

const chargeHandler = async (req, res) => {
  const { amount, token } = req.body;
  const charge = await stripe.charges.create({
    amount,
    currency: 'usd',
    source: token.id,
    description: 'Charge for test@example.com', // Replace with your own description
  });
  res.json(charge);
};

app.post('/charge', chargeHandler);
```

In this implementation, we first import the Stripe Node.js library and initialize it with our own secret key. We then define a request handler for the **`/charge`** endpoint that extracts the **`amount`** and **`token`** (a Stripe token object containing the user's credit card details) from the request body.

We then use the **`stripe.charges.create()`** method to charge the user's credit card for the specified amount. This method takes several parameters, including the **`amount`**, **`currency`**, **`source`** (the Stripe token ID), and **`description`** (a string that describes the charge). The method returns a Promise that resolves to a **`charge`** object containing details about the charge, such as the amount, currency, status, and more.

Finally, we return the **`charge`** object as a JSON response using **`res.json()`**.

To test this endpoint, you can make a POST request to the **`/charge`** endpoint with the following JSON payload in the request body:

```json
{
  "amount": 2000,
  "token": {
    "id": "tok_visa",
    "card": {
      "brand": "visa",
      "country": "US",
      "exp_month": 12,
      "exp_year": 2022,
      "last4": "4242"
    }
  }
}
```

This payload specifies an amount of $20.00 (in cents) and a test credit card with the last 4 digits "4242" and an expiration date of 12/2022. When the endpoint is called, Stripe will charge the specified amount to the test credit card and return a JSON response containing details about the charge.