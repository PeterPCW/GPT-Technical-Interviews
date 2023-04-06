# How would you use Node.js and the Redis database to implement a caching layer for a specific API endpoint that returns data that is expensive to compute, and configure the cache to automatically expire after a certain amount of time?

To use Node.js and the Redis database to implement a caching layer for a specific API endpoint, we can use the **`redis`** package to create a Redis client and the **`express`** package to define the API endpoint. Here's an example implementation:

```typescript
const express = require('express');
const redis = require('redis');
const { promisify } = require('util');

const app = express();

const redisClient = redis.createClient();

const getAsync = promisify(redisClient.get).bind(redisClient);
const setAsync = promisify(redisClient.set).bind(redisClient);
const expireAsync = promisify(redisClient.expire).bind(redisClient);

const CACHE_TTL = 60; // cache results for 1 minute

app.get('/api/expensive-data', async (req, res) => {
  const cacheKey = req.url;
  const cachedResult = await getAsync(cacheKey);
  if (cachedResult) {
    res.json(JSON.parse(cachedResult));
  } else {
    // compute expensive data
    const expensiveData = ...; // some expensive operation

    // cache result
    await setAsync(cacheKey, JSON.stringify(expensiveData));
    await expireAsync(cacheKey, CACHE_TTL);

    res.json(expensiveData);
  }
});

app.listen(3000, () => {
  console.log('Server listening on port 3000');
});
```

In this example, we create a Redis client using the **`redis`** package and define three utility functions using the **`util`** package to promisify Redis commands.

We then define an API endpoint for the **`GET /api/expensive-data`** route. When a client requests this endpoint, we first check the Redis cache for a cached result using the request URL as the cache key. If the result is cached, we return it immediately. Otherwise, we compute the expensive data, cache the result using the request URL as the cache key, and set the cache expiration time to **`CACHE_TTL`** seconds.

With this implementation, the expensive data is only computed when the cache is empty, and the result is cached for future requests. The cache also automatically expires after **`CACHE_TTL`** seconds, ensuring that stale data is not returned to clients.