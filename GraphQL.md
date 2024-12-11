### GraphQl  
- Is a querylanguage :  common misconception is - query langauge or graphQL is something related to database, but query langauge is programming language use to get data may be from database, API etc.
- GraphQL between front end and backend, and SQL query language is between backend and database.

- In GraphQL there are 2 types of requests there is query and mutation
![image](https://github.com/user-attachments/assets/5609d8c5-daf9-4582-9fda-7055fdd0a5a0)

Here we see getting data from API is query and once we get it everything we do with data like put, delete, post is mutating with data.  
Difference betweent GQL and REST  
- we have one single Endpoint from where the request goes through /graphQl
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
