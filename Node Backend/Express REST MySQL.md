# Can you demonstrate how you would create a RESTful API endpoint using Node.js and the Sequelize ORM, and use it to retrieve all records that meet a certain criteria from a MySQL database?

```typescript
const express = require('express');
const Sequelize = require('sequelize');

const sequelize = new Sequelize('mysql://username:password@localhost:3306/database');

const User = sequelize.define('user', {
  id: {
    type: Sequelize.INTEGER,
    primaryKey: true,
    autoIncrement: true,
  },
  name: {
    type: Sequelize.STRING,
    allowNull: false,
  },
  age: {
    type: Sequelize.INTEGER,
    allowNull: false,
  },
});

const app = express();

app.get('/users', async (req, res) => {
  try {
    const users = await User.findAll({
      where: {
        age: {
          [Sequelize.Op.gt]: 18, // Retrieve all users older than 18
        },
      },
    });
    res.json(users);
  } catch (err) {
    console.error(err);
    res.status(500).json({ error: 'Internal server error' });
  }
});

app.listen(3000, () => {
  console.log('Server started on port 3000');
});

```

In this implementation, we first create a Sequelize instance that connects to a MySQL database. We then define a **`User`** model that represents a user in the database and has **`id`**, **`name`**, and **`age`** fields.

We then create an Express app and define a route for the **`/users`** endpoint that retrieves all users from the database whose **`age`** is greater than 18. This is done by calling the **`findAll`** method on the **`User`** model with a **`where`** option that specifies the search criteria.

If the query is successful, the retrieved users are returned as a JSON response. If there is an error, a 500 status code and an error message are returned.

Finally, we start the server and listen for incoming requests on port 3000.
```