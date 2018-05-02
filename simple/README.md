#Very Simple GraphQL Example

This is very simple GraphQL example using Node.JS Apollo-Server. Refer to http://graphql.org/code/#javascript for full list of example libraries for JavaScript.

1) Install node.js for your operating system. E.g. in Mac in can be installed with brew as following:

```bash
  brew install node
```

2) Create a node package

```bash
  npm init
```

3) Set up apollo-server-express, graphql-tools and graphql for the endpoint

```bash
  npm install --save apollo-server-express graphql-tools graphql express body-parser
```
4) Create server.js

```bash
  touch server.js
```
5) Modify server.js as following:

  a) Add the require libraries as indicated

  ```bash
  const express = require('express');
  const bodyParser = require('body-parser');
  const { graphqlExpress, graphiqlExpress } = require('apollo-server-express');
  const { makeExecutableSchema } = require('graphql-tools');
  ```

  b) Define the GraphQL types. In this simple example, just an Order Object and a Query operation against it.

  ```bash
  const typeDefs = `
    type Order {
      id: String!,
      price: Int
    }
    type Query {
      orders: [Order]
    }
  `;
  ```
  c) Create some dummy data (as in this simple example we're not connecting to a data source)

  ```bash
  const orders = [
    {
      id: "ORDER001",
      price: 22,
    },
    {
      id: "ORDER002",
      price: 100,
    },
  ];
  ```

  d) Now that we have types defined with sample data, a "resolver" must be created. A resolver implements the operations, in this case, a Query.

  ```bash
  const resolvers = {
    Query: { orders: () => orders },
  };
  ```

  e) Now that the types and resolver are in place, the GrahQL schema can be created by making use of the "makeExecutableSchema" function part of "graphql-tools"

  ```bash
  const schema = makeExecutableSchema({
    typeDefs,
    resolvers,
  });
  ```

  d) Lastly we initiate the express server and define the different URI's the GraphQL service will resolve to

  ```bash
  // Initialize the app
  const app = express();

  // The GraphQL endpoint
  app.use('/graphql', bodyParser.json(), graphqlExpress({ schema }));

  // GraphiQL, a visual editor for queries
  app.use('/graphiql', graphiqlExpress({ endpointURL: '/graphql' }));

  // Start the server
  app.listen(3000, () => {
    console.log('Go to http://localhost:3000/graphiql to run queries!');
  });
  ```

5) Start the GraphQL service

  ```bash
  npm start
  ```
  or just

  ```bash
  node server.js
  ```