## Defining GraphQL Schema

Before we switch our focus to MongoDB, let us setup the GraphQL server with some sample data.
I have created a file called sampleData.js in the server folder and added some sample data to it. It holds the data for the clients and projects.It contains arrays of client and project objects.

```javascript
const projects = [
  {
    id: "1",
    clientId: "1",
    name: "eCommerce Website",
    description:
      "Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aenean commodo ligula eget dolor. Aenean massa. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Donec quam felis, ultricies nec, pellentesque eu.",
    status: "In Progress",
  },
  ....
];

// Clients
const clients = [
  {
    id: "1",
    name: "Tony Stark",
    email: "ironman@gmail.com",
    phone: "343-567-4333",
  },
  ...
]
```

Now we have some sample data to work with. Let us create a schema for the GraphQL server. I have created a file called schema.js in the server folder and added the schema for the clients and projects.

The GraphQL schema for the project management app is defined in `server/schema/schema.js`. It outlines the structure of the data and the types of queries that can be executed against the GraphQL server.

### Types

We start the schema by defining the types that represent the data in the application. The schema defines two types: `ClientType` and `ProjectType`.

#### ClientType

Represents a client with the following fields:

- `id`: A unique identifier for the client. (GraphQLID)
- `name`: The name of the client. (GraphQLString)
- `email`: The email address of the client. (GraphQLString)
- `phone`: The phone number of the client. (GraphQLString)

#### ProjectType

Represents a project with the following fields:

- `id`: A unique identifier for the project. (GraphQLID)
- `clientId`: The identifier of the client associated with the project. (GraphQLID)
- `name`: The name of the project. (GraphQLString)
- `description`: A brief description of the project. (GraphQLString)
- `status`: The current status of the project. (GraphQLString)
- `client`: A reference to the client associated with the project. (ClientType)

### Root Query

The root query defines the entry points for the API:

- `clients`: Retrieves a list of all clients. Returns `[ClientType]`.
- `client`: Retrieves a single client by ID. Accepts `id` (GraphQLID) as an argument. Returns `ClientType`.
- `projects`: Retrieves a list of all projects. Returns `[ProjectType]`.
- `project`: Retrieves a single project by ID. Accepts `id` (GraphQLID) as an argument. Returns `ProjectType`.

## Starting the GraphQL Server

Now with our schema ready let us run our graphql endpoint. We will use express-graphql to create the graphql server.

```javascript
const express = require("express");
const { graphqlHTTP } = require("express-graphql");
require("dotenv").config();
const port = process.env.PORT || 6000;
const schema = require("../server/schema/schema.js");

const app = express();

app.use(
  "/graphql",
  graphqlHTTP({
    schema,
    graphiql: process.env.NODE_ENV === "development",
  })
);

app.listen(port, console.log(`App listening on port : ${port}`));
```

Now with all these changes let us restart our server and test the graphql endpoint by running the command `npm run dev`. To see the graphiql interface visit `http://localhost:5001/graphql`
