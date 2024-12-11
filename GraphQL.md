### GraphQl  
- Is a querylanguage :  common misconception is - query langauge or graphQL is something related to database, but query langauge is programming language use to get data may be from database, API etc.
- GraphQL between front end and backend, and SQL query language is between backend and database.

- In GraphQL there are 2 types of requests there is query and mutation
![image](https://github.com/user-attachments/assets/5609d8c5-daf9-4582-9fda-7055fdd0a5a0)

Here we see getting data from API is query and once we get it everything we do with data like put, delete, post is mutating with data.  
#### Difference betweent GQL and REST
Data Retrieval  
GraphQL:  
Clients specify exactly what data they need.  
Fetches data in a single request, reducing over-fetching or under-fetching issues.  
Example: Querying a user object with specific fields like name and email.  
REST:  
Data is retrieved via fixed endpoints, often returning a predefined structure.  
May lead to over-fetching (getting unnecessary data) or under-fetching (requiring multiple requests to gather needed data).  
- we have one single Endpoint from where the request goes through /graphQl

Endpoint Design  
GraphQL:  
Single endpoint (e.g., /graphql) to handle all queries and mutations.  
REST:  
Multiple endpoints (e.g., /users, /posts, /users/1/posts) for different resources.  
![image](https://github.com/user-attachments/assets/2ed8db55-6bf4-4aaa-8094-c1f48e209bed)  

#### We will be using Countries GraphQL API - which has the information about countries, continent and languages.  
https://github.com/trevorblades/countries  
and go to link :  https://countries.trevorblades.com/ to make the request to API  
Basic Datatpes  
Scalars in GraphQL are the simplest, most basic data types and serve as the foundation for creating more complex types. They are analogous to primitive types in other programming languages.  
```graphql


type User {
  id: ID!
  name: String!
  age: Int!
  height: Float!
isMarried:Boolean
  friend: [User!]
  videoPosted:[Video]
  
}
type Video {
  id:ID!
  title:String!
  description: String!
}
//here ! denotes is not nullable, so here isMarried can have null value.
}
```
Every GQL must have Schema - which defines all types of data that exist inside of the api. Every GraphQL Schema will have a root type called query. ANd this root type will have bunch of fields similar to below:   

type Query {
users: [User]
user: (id: ID): User

}
```graphql
//Request
query
{
  country(code:"US") {
    code
    name
    phone
    capital
    currency
  }
}
//output
{
  "data": {
    "country": {
      "code": "US",
      "name": "United States",
      "phone": "1",
      "capital": "Washington D.C.",
      "currency": "USD,USN,USS"
    }
  }
}
```
The curly braces {} denote the root selection set of the query.  
It represents the starting point for fetching data and specifies what fields to return.    
Every GraphQL operation (like a query or mutation) begins with {} to define the data structure you want in response.  

----------------
Now open VSC- and start a node js project.
> npm init
//add some relevant information and a package.json is created.
The package.json file is a core component of any Node.js project, including a GraphQL project. It serves multiple purposes, such as managing dependencies, scripts, and metadata for your application. Here's what package.json typically does in a GraphQL Node.js project:

Lists the libraries and tools required to build and run the GraphQL API.  
Essential packages for production, e.g.:  
```package.json
"dependencies": {
  "graphql": "^16.6.0",
  "express": "^4.18.0",
  "apollo-server": "^3.11.0"
}
```
Tools needed only during development, e.g.:  
```package.json
"devDependencies": {
  "nodemon": "^2.0.22",
  "eslint": "^8.49.0"
}
```
##### Script Management  
Defines scripts to simplify commands, such as starting the server or running development tools.  
Start the server: npm start  
Run the development server with Nodemon: npm run dev  
##### Project Metadata  
Contains descriptive information about your GraphQL project:  
name: The project name.  
version: The project version.  
description: A brief description. 
It ensures the project is reproducible across different environments.    
Manages essential GraphQL dependencies like graphql, apollo-server, or express.  
Simplifies development tasks through scripts.  
Acts as a manifest for your project, defining its purpose and structure.  


#### Now install apollo server library - help to create the API and graphQL package 
> npm install apollo-server graphql
//we need to install Nodemon which automatically restarts the Node.js server whenever it detects changes in your code files.
This is particularly useful during GraphQL API development, where you frequently update schemas, resolvers, or configurations.
> npm install nodemon

we had added in package.json  
```package.json
 "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "nodeman index.js"
  },
```
so when we do npm start it start the nodeman index.js  

--> Create index.js //entry point of our API  
and define the apollo server and start the server by server.listen and print the msg of start of API.  
```index.js
const {ApolloServer} = require("apollo-server");

const server = new ApolloServer({typeDefs, resolvers});

//every type you define, every query in graphQl willl exists inside of typeDefs, and all of the functions which resolves those types will be in resolvers.


server.listen().then(()=>{
    console.log(`YOUR API IS RUNNING AT: ${url} :)`);
})
//its starting the server to listen and then  printing the msg- u can also put url where api is running.
```
--> make a folder index.js and make file type-def.js and resolvers.js into it  

