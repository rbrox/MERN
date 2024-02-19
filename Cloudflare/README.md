## Cloudflare Workers <img src="../assets/cloudflare.svg" alt="Cloudflare Workers" style="max-height:20px;">

## Index

- [Introduction](#introduction)
- [Cloudflare Workers Functionality](#cloudflare-workers-functionality)
- [Getting Started with Cloudflare Workers](#getting-started-with-cloudflare-workers)

## Introduction

Cloudflare Workers is a serverless execution environment that allows developers to deploy JavaScript code at the edge of Cloudflare's network. It enables the execution of code closer to end-users, improving performance and reducing latency. Cloudflare Workers can be used for various use cases, including `API handling`, `static site hosting`, `security enhancements`, and more.

#### Link to [Documentation](https://developers.cloudflare.com/workers/)

## Cloudflare Workers Functionality

Cloudflare Workers offer a range of features and functionality for building serverless applications at the edge. Some of the key features include:

- **Edge Computing**: Cloudflare Workers enable developers to run JavaScript code at `Cloudflare's edge servers`, allowing for quick response times and efficient use of resources. This architecture brings compute power closer to users, reducing latency and improving performance.

- **Request Handling**: Workers can intercept and manipulate `HTTP requests and responses` at the edge. This capability enables developers to implement various `routing`, `content modification`, `security measures`, and other `request/response handling` logic.

- **Serverless Architecture**: Cloudflare Workers follow a `serverless` model, where developers can write and deploy code without managing the underlying infrastructure. This approach eliminates the need for provisioning or scaling servers, making it easier to develop and deploy applications.

- **Scalability and Performance**: With Cloudflare's global network of data centers, Workers can scale automatically to handle `millions of requests per second`. Additionally, the distributed nature of Workers ensures `high availability` and `reliability`, even during traffic spikes or infrastructure failures.

## Getting Started with Cloudflare Workers

To get started with Cloudflare Workers, follow these steps:

1. **Sign up for Cloudflare**: If you haven't already, sign up for a Cloudflare account at [cloudflare.com](https://www.cloudflare.com/).

2. **Install Wrangler CLI**: Wrangler is a command-line interface (CLI) tool for managing Cloudflare Workers projects. Install Wrangler by following the instructions in the [documentation](https://developers.cloudflare.com/workers/cli-wrangler/install-update).

3. **Create a New Worker**: Use Wrangler to create a new Cloudflare Worker project. Run `wrangler generate <project-name>` to generate a new project template.

4. **Write Your Worker Code**: Open the generated project in your preferred code editor and write your Cloudflare Worker code. You can implement logic to handle HTTP requests, manipulate responses, and more.

5. **Configure Your Worker**: Update the `wrangler.toml` file with your Cloudflare account information and project settings.

6. **Deploy Your Worker**: Use Wrangler to deploy your Cloudflare Worker to the edge network. Run `wrangler publish` to deploy your code.

7. **Verify Deployment**: Once deployed, verify that your Cloudflare Worker is functioning as expected by accessing its URL in a web browser or using tools like `curl` or `Postman`.

8. **Monitor and Debug**: Monitor the performance of your Cloudflare Worker using Cloudflare's analytics dashboard. Use logging and debugging techniques to troubleshoot any issues that arise.

By following these steps, you can quickly get started with Cloudflare Workers and leverage the power of serverless computing at the edge of the network. Happy coding!

#### Mini Project

1. Went to the terminal and ran the following cmd `npm create cloudflare -- [name-of-the-project]`.
2. This cmd creates a new project with the name provided and sets up the necessary files and configurations.
3. Understanding the created files and configurations:

   ```
    project-strucutre
    name/
    |__ wrangler.toml
    |__ src/
        |__ index.ts
   ```

   `wrangler.toml`: Cloudflare config file, contains the account id, zone id, and other configurations.
   `src/index.ts`: The main file where the worker code is written. quite similar to a node server file/Express server file.

4. Syntatical Changes:
   Does cloudflare workers initiate some new syntax/provide some new objects classes?

   - Yes routing seems to have downgardede a lot from the genral express routing.

   Express Routing:

   ```javascript
    app.get('/', (req, res) => {
       res.send('Hello World');
    });
    app.get('/login', (req, res) => {
       res.send('Login Page');
    });
    app.get('/signup', (req, res) => {
       res.send('Signup Page');
     });
   ...
   ```

   The stupid way to do it on cloudflare workers:

   ```javascript
   export default {
     async fetch(
       request: Request,
       env: Env,
       ctx: ExecutionContext
     ): Promise<Response> {
       console.log(request.body);
       console.log(request.headers);

       if (request.method === "GET") {
         return Response.json({
           message: "you sent a get request",
         });
       } else {
         return Response.json({
           message: "you did not send a get request",
         });
       }
     },
   };
   ```

   - The `fetch` function is the main function that handles all the requests.

5. Better Solution to ugly routing:

   - Use certain libraries like Express or Oak to handle the routing and other functionalities.
   - Sadly the cloudflare workers do not support the use of these libraries.
   - hence use others like `Hono` etc.

6. Hono is very intuitive once you get the hang of it. It is a very simple and easy to use library that can be used to handle the routing and other functionalities.

   - Hono stores all the req res stuff in a single obj (called `context` written as `c` genrally).

   ```Typescript
   import {Hono} from 'hono';

   const app = Hono();

   app.get('/:page', async (c) => {
     console.log(new Date().toLocaleTimeString());
     console.log(c.req.query('page'));
     console.log(c.req.param('page'));
     console.log(typeof c.req.query('page'));
     return c.text('Hello Hono!');
   });
   ```

   - Listing the various methods that can be used with the `context` obj.
     [Link to Docs](https://hono.dev/api/context)
