


# Testing APIs Using Jest and Supertest

This guide covers how to set up, configure, write, and run tests for APIs using Jest and Supertest.

## Website Link
[Getting Started with Jest](https://jestjs.io/docs/getting-started)


## 1. Prerequisites

Ensure you have Node.js and npm installed. You can check by running:

```bash
node -v
npm -v
```

If you don't have Node.js installed, download it from [nodejs.org](https://nodejs.org/).

## 2. Setting Up Your Project

1. Create a new Node.js project or navigate to your existing project directory:

   ```bash
   mkdir api-testing
   cd api-testing
   npm init -y
   ```

2. Install the required dependencies:

   ```bash
   npm install jest supertest --save-dev
   ```

3. Install Express (or any other framework you're using to create APIs):

   ```bash
   npm install express
   ```

## 3. Configuring Jest

### Generate a basic configuration file
```
npm init jest@latest
``` 


1. Open `package.json` and add a `test` script for Jest:

   ```json
   {
     "scripts": {
       "test": "jest"
     }
   }
   ```

2. (Optional) If you're using Babel or ES6+ features, you might need to configure Babel. Install Babel dependencies:

   ```bash
   npm install @babel/preset-env @babel/core --save-dev
   ```

3. Create a `.babelrc` file if needed:

   ```json
   {
     "presets": ["@babel/preset-env"]
   }
   ```

4. Create a `jest.config.js` file (optional if you need custom configurations):

   ```js
   module.exports = {
     testEnvironment: 'node'
   };
   ```

## 4. Initializing Supertest and Express

1. Create a basic `app.js` file to define your Express application:

   ```js
   const express = require('express');
   const app = express();

   app.use(express.json());

   // Sample API route
   app.get('/api/test', (req, res) => {
     res.status(200).json({ message: 'API is working!' });
   });

   module.exports = app;
   ```

2. Create a `server.js` file to run your app:

   ```js
   const app = require('./app');
   const PORT = process.env.PORT || 3000;

   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

## 5. Writing Tests

1. Create a `__tests__` folder to hold your test files. Inside, create a test file `app.test.js`:

   ```bash
   mkdir __tests__
   touch __tests__/app.test.js
   ```

2. In `app.test.js`, write the following test using Jest and Supertest:

   ```js
   const request = require('supertest');
   const app = require('../app'); // Ensure the correct path to your app

   describe('API Testing', () => {
     it('should return a successful response from /api/test', async () => {
       const res = await request(app).get('/api/test');
       expect(res.statusCode).toEqual(200);
       expect(res.body).toHaveProperty('message', 'API is working!');
     });
   });
   ```

## 6. Running the Tests

1. Run your test using the following command:

   ```bash
   npm test
   ```

   You should see output similar to this:

   ```bash
   PASS  __tests__/app.test.js
     API Testing
       ✓ should return a successful response from /api/test (40 ms)

   Test Suites: 1 passed, 1 total
   Tests:       1 passed, 1 total
   ```

## 7. Advanced Testing

- **POST requests**: If you're testing POST endpoints, you can use `request(app).post('/api/route').send(payload)` to simulate a request.
  
  Example:

  ```js
  it('should handle POST requests', async () => {
    const payload = { name: 'Test' };
    const res = await request(app).post('/api/post-route').send(payload);
    expect(res.statusCode).toEqual(201);
    expect(res.body).toHaveProperty('name', 'Test');
  });
  ```

- **Headers & Auth**: You can also set headers or authorization tokens with `.set()`:

  ```js
  it('should return a 401 for unauthorized requests', async () => {
    const res = await request(app).get('/api/secure-route').set('Authorization', 'invalid-token');
    expect(res.statusCode).toEqual(401);
  });
  ```

## 8. Running Tests in Watch Mode (Optional)

You can run tests in watch mode for continuous testing as you make changes:

```bash
npm test -- --watch
```

## 9. Test Coverage (Optional)

To check test coverage, you can run:

```bash
npm test -- --coverage
```

## 10. Conclusion
```

This guide outlines the steps for setting up, configuring, writing, and running API tests using Jest and Supertest in a Node.js environment.