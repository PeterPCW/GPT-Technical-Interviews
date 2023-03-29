<details>
  <summary>Can you demonstrate how you would create a JWT-based authentication API endpoint using Node.js, and use it to restrict access to a protected resource?</summary>
      
  ```jsx
  const express = require('express');
  const jwt = require('jsonwebtoken');
  const bcrypt = require('bcrypt');
  
  const app = express();
  app.use(express.json());
  
  const users = [
    {
      id: 1,
      username: 'user1',
      passwordHash: '$2b$10$0Gd7xJtSLzPdB7.BuIvzgeVZ.GCbBETJ1Vg0kYvS8Wj.aOR4TYg9C', // password is "password1"
    },
  ];
  
  const secretKey = 'secret';
  
  const generateToken = (user) => {
    const token = jwt.sign({ id: user.id, username: user.username }, secretKey);
    return token;
  };
  
  const authenticate = async (username, password) => {
    const user = users.find((u) => u.username === username);
    if (!user) {
      return null;
    }
    const match = await bcrypt.compare(password, user.passwordHash);
    if (!match) {
      return null;
    }
    return user;
  };
  
  const requireAuth = (req, res, next) => {
    const authHeader = req.headers.authorization;
    if (!authHeader) {
      return res.status(401).json({ message: 'Unauthorized' });
    }
    const [type, token] = authHeader.split(' ');
    if (type !== 'Bearer' || !token) {
      return res.status(401).json({ message: 'Unauthorized' });
    }
    try {
      const payload = jwt.verify(token, secretKey);
      req.user = payload;
      next();
    } catch (err) {
      return res.status(401).json({ message: 'Unauthorized' });
    }
  };
  
  app.post('/auth', async (req, res) => {
    const { username, password } = req.body;
    const user = await authenticate(username, password);
    if (!user) {
      return res.status(401).json({ message: 'Invalid credentials' });
    }
    const token = generateToken(user);
    return res.json({ token });
  });
  
  app.get('/protected', requireAuth, (req, res) => {
    return res.json({ message: 'Protected resource' });
  });
  
  app.listen(3000, () => {
    console.log('Server started on port 3000');
  });
  
  ```
  
  In this implementation, we define an array of **`users`**, each with an ID, username, and a bcrypt-hashed password. We also define a **`secretKey`** that will be used to sign and verify JWT tokens.
  
  We define a **`generateToken`** function that takes a user object and returns a JWT token signed with the **`secretKey`**.
  
  We define an **`authenticate`** function that takes a username and password, looks up the corresponding user in the **`users`** array, and compares the provided password hash with the stored password hash using bcrypt. If the authentication is successful, the function returns the user object; otherwise, it returns null.
  
  We define a **`requireAuth`** middleware function that checks the **`Authorization`** header of incoming requests for a JWT token. If the token is missing or invalid, the middleware responds with a 401 Unauthorized error. If the token is valid, the middleware sets the **`user`** property on the **`req`** object and calls the **`next`** function to pass control to the next middleware or route handler.
  
  We define a **`/auth`** route that accepts a username and password in the request body, calls the **`authenticate`** function, generates a JWT token using the **`generateToken`** function, and returns the token in the response body.
  
  The **`/protected`** endpoint is a GET endpoint that requires a JWT token to be present in the **`Authorization`** header of the request. The server first checks if the token is present, and if not, sends back a 401 Unauthorized error. If the token is present, the server verifies its authenticity using the secret key, and sends back the protected data if the token is valid.
</details>