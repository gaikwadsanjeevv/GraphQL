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
```installation Commands
npm init
npm install apollo-server graphql
npm install nodemon
```
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
--> make a folder schema and make file type-def.js and resolvers.js into it  
The codes for the files are as follows:  
```package.json
{
  "name": "1graphqldemo",
  "version": "1.0.0",
  "description": "Demonstration of GraphQL",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "nodeman index.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "apollo-server": "^3.13.0",
    "graphql": "^16.9.0",
    "nodemon": "^3.1.7"
  }
}

```
index.js
```index.js
const { ApolloServer } = require("apollo-server");
const { typeDefs } = require("./schema/type-defs");
const { resolvers } = require("./schema/resolvers");

const server = new ApolloServer({ typeDefs, resolvers });

// Start the server and print the URL where it's running
server.listen().then(({ url }) => {
    console.log(`YOUR API IS RUNNING AT: ${url} :)`);
});
//its starting the server to listen and then  printing the msg- u can also put url where api is running.
```
resolvers.js
```resolvers.js

const {UserList} = require("../FakeData");
const resolvers = { //this object will contain all the resolvers functions that will exists inside oru API, all functions making calls to data base, what returing to frontend etc

    Query: {
        users() {

                
            return UserList;
        },
    },
};

module.exports = {resolvers};
```
type-defs.js
```type-defs.js
const { gql } = require("apollo-server"); // Importing the gql template literal

const typeDefs = gql`
type User {
    id: ID!
    name: String!
    username: String!
    age: Int!
    nationality: String!
}

type Query { # Every schema starts with one specific type called Query which will hold all queries we want to make inside our API.
    users: [User!]!
}
`;

module.exports = { typeDefs };

```
Data.js
```data.js
 const UserList = [

    
        { id: 1, name: "Alice Johnson", username: "alicej", age: 25, nationality: "American" },
        { id: 2, name: "Bob Smith", username: "bobsmith", age: 30, nationality: "Canadian" },
        { id: 3, name: "Charlie Brown", username: "charlieb", age: 27, nationality: "British" },
        { id: 4, name: "Diana Prince", username: "dianap", age: 29, nationality: "Australian" },
        { id: 5, name: "Evan Thomas", username: "evant", age: 35, nationality: "German" },
        { id: 6, name: "Fiona Davis", username: "fionad", age: 24, nationality: "Irish" },
        { id: 7, name: "George Miller", username: "georgem", age: 33, nationality: "American" },
        { id: 8, name: "Hannah Lee", username: "hannahl", age: 28, nationality: "South Korean" },
        { id: 9, name: "Ian Carter", username: "ianc", age: 31, nationality: "New Zealander" },
        { id: 10, name: "Jessica Wong", username: "jessicaw", age: 22, nationality: "Chinese" },
        { id: 11, name: "Kevin White", username: "kevinw", age: 26, nationality: "South African" },
        { id: 12, name: "Laura Green", username: "laurag", age: 32, nationality: "French" },
        { id: 13, name: "Michael Brown", username: "michaelb", age: 37, nationality: "Indian" },
        { id: 14, name: "Nina Patel", username: "ninap", age: 25, nationality: "Indian" },
        { id: 15, name: "Oscar Diaz", username: "oscard", age: 34, nationality: "Mexican" },
        { id: 16, name: "Paula Baker", username: "paulab", age: 29, nationality: "Canadian" },
        { id: 17, name: "Quincy Adams", username: "quincya", age: 40, nationality: "American" },
        { id: 18, name: "Rachel Kim", username: "rachelk", age: 23, nationality: "South Korean" },
        { id: 19, name: "Samuel Johnson", username: "samuelj", age: 36, nationality: "Australian" },
        { id: 20, name: "Tina Hernandez", username: "tinah", age: 27, nationality: "Spanish" },
        { id: 21, name: "Uma Gupta", username: "umag", age: 21, nationality: "Indian" },
        { id: 22, name: "Victor Cruz", username: "victorc", age: 39, nationality: "Brazilian" },
        { id: 23, name: "Wendy Nelson", username: "wendyn", age: 26, nationality: "British" },
        { id: 24, name: "Xander Black", username: "xanderb", age: 28, nationality: "American" },
        { id: 25, name: "Yasmin Ali", username: "yasmina", age: 34, nationality: "Egyptian" },
        { id: 26, name: "Zachary King", username: "zacharyk", age: 30, nationality: "Canadian" },
        { id: 27, name: "Aaron Carter", username: "aaronc", age: 23, nationality: "New Zealander" },
        { id: 28, name: "Betty Liu", username: "bettyl", age: 35, nationality: "Taiwanese" },
        { id: 29, name: "Carlos Silva", username: "carloss", age: 31, nationality: "Portuguese" },
        { id: 30, name: "Daisy Cooper", username: "daisyc", age: 24, nationality: "American" }
];

module.exports = {UserList};
```
> Now run the project:  node index.js you get an local host and following graphQL page:
![image](https://github.com/user-attachments/assets/eeeddde9-4f39-4adf-8400-bad1b75fd9f9)

Fllowing is the query request and response:  
![image](https://github.com/user-attachments/assets/9b20eb04-39d2-40b5-ad62-796d33d808dc)  


