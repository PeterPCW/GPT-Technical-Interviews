# Can you demonstrate how you would use the Express.js Router to create a RESTful API endpoint that accepts a parameter and returns a JSON response?

```typescript
const express = require('express');
const app = express();

// Create a new router instance
const router = express.Router();

// Define a route that accepts a parameter
router.get('/users/:id', (req, res) => {
  const userId = req.params.id;

  // TODO: Lookup the user with the given ID from a database or other data source
  const user = { id: userId, name: 'John Doe', email: 'johndoe@example.com' };

  // Return a JSON response with the user data
  res.json(user);
});

// Mount the router at a specific endpoint
app.use('/api', router);

// Start the server
app.listen(3000, () => {
  console.log('Server started on port 3000');
});

```

In this example, we first create a new instance of the Express.js Router using the **`express.Router()`** method. We then define a route for the router using the **`router.get()`** method, which accepts a URL path with a parameter placeholder (**`/users/:id`**) and a callback function that is executed when the route is matched. Inside the callback function, we retrieve the **`id`** parameter from the request using **`req.params.id`** and use it to lookup the corresponding user data from a database or other data source. Finally, we return a JSON response with the user data using the **`res.json()`** method.

We then mount the router at a specific endpoint (**`/api`**) using the **`app.use()`** method, which tells Express to use the router for any requests that match the specified endpoint. Finally, we start the server using the **`app.listen()`** method and log a message to the console to indicate that the server has started.

With this code, a client can make a GET request to **`/api/users/123`** (where **`123`** is the ID of the user they want to retrieve) and receive a JSON response with the user data in return.