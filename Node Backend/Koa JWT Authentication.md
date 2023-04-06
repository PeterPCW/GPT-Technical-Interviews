# How would you use Koa.js middleware to implement an authentication system that requires users to provide a JWT token in the headers of their HTTP requests?

In this example, we first define a secret key (**`mysecretkey`**) for signing JWT tokens, and a user database (**`users`**) that stores the user credentials. We then define a route for handling user login requests using the **`app.use()`** method, which checks if the HTTP request method is **`POST`** and the path is **`/login`**. If it is, we lookup the user with the given username and password from the database, and generate a JWT token with the user ID as the payload using the **`jwt.sign()`** method. We then return the JWT token as a response.

We then define a middleware function (**`authMiddleware`**) for verifying JWT tokens using the **`koa-jwt`** package, which checks if the token is valid and contains a valid signature using the secret key. We mount this middleware function using the **`app.use()`** method, which applies it to any subsequent routes.

Finally, we define a protected route that requires authentication using the **`authMiddleware`** middleware function and the **`app.use()`** method. This route checks if the HTTP request method is **`GET`** and the path is **`/protected`**. If the user is authenticated, we return a response indicating that they are authenticated.

With this code, a client can make a POST request to **`/login`** with their username and password in the request body, and receive a JWT token in return. They can then include this token in the **`Authorization`** header of subsequent GET requests to **`/protected`** to authenticate themselves and access the protected resource.

```typescript
const Koa = require('koa');
const koaJwt = require('koa-jwt');
const jwt = require('jsonwebtoken');

const app = new Koa();

// Define a secret key for signing JWT tokens
const secret = 'mysecretkey';

// Define a user database or data source
const users = [
  { id: 1, username: 'user1', password: 'password1' },
  { id: 2, username: 'user2', password: 'password2' },
];

// Define a route for handling user login requests
app.use(async (ctx, next) => {
  if (ctx.request.method === 'POST' && ctx.request.path === '/login') {
    const { username, password } = ctx.request.body;

    // TODO: Lookup the user with the given username and password from the database or other data source
    const user = users.find(u => u.username === username && u.password === password);

    if (user) {
      // Generate a JWT token with the user ID as the payload and sign it with the secret key
      const token = jwt.sign({ id: user.id }, secret);

      // Return the JWT token as a response
      ctx.body = { token };
    } else {
      // Return a 401 Unauthorized error if the login credentials are invalid
      ctx.throw(401, 'Invalid login credentials');
    }
  } else {
    await next();
  }
});

// Define a middleware function for verifying JWT tokens
const authMiddleware = koaJwt({ secret });

// Define a protected route that requires authentication
app.use(authMiddleware);
app.use(async (ctx, next) => {
  if (ctx.request.method === 'GET' && ctx.request.path === '/protected') {
    // Return a response indicating that the user is authenticated
    ctx.body = { message: 'You are authenticated' };
  } else {
    await next();
  }
});

// Start the server
app.listen(3000, () => {
  console.log('Server started on port 3000');
});
```