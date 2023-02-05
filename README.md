# Publy - GraphQL example application

This repository contains my GraphQL example application _publy_ that is built with Spring for GraphQL and React. It demonstrates typical GraphQL use-cases.

## 1. Setup the workspace and run the application

![Screenshot of the example application](./publy-screenshot.png)

**Prerequisites:**

- Java 17
- Docker
- yarn](https://yarnpkg.com/) optional, only needed for the React/Apollo frontend

**Step 1: clone the repository**

- Please clone this repository with your Git client first.

**Step 2: Start the database (with Docker)**


- `docker-compose -f docker-compose.yml up -d`

**Step 3: Start userservice**

Attention. Port 17081 must not be occupied:

- `./gradlew publy-userservice:bootRun`

**Step 4: Start backend**

Attention. Port 17080 must not be occupied:

- `./gradlew publy-backend:bootRun`

You can now use `http://localhost:17080` to open the GraphiQL-Explorer
and execute queries and mutations.

**Step 5: Start frontend (optional)**

Attention. Port 3000 must not be occupied:

```bash

cd publy-frontend

yarn install

yarn start
```

You can now open the frontend via `http://localhost:3000` in your browser.

### Open workspace in the IDE

You can open the root directory in IDEA, then the project will be recognised as a Gradle project and automatically configured correctly by IDEA.

Then you can start the two server processes as Java applications:

- `nh.graphql.publy.userservice.UserserviceApplication`: The user service
- `nh.publy.backend.PublyApplication`: The (GraphQL-)backend

Important: In any case, please first start the database with `docker-compose` as described above!

## 2. Try the frontend

If you want to try out the example frontend, open it in your browser at `http://localhost:3000`.

As an anonymous visitor of the app, you are allowed to read articles and comments, as well as user ("member") information. GraphQL **Queries** are then executed in the background,
which you can inspect with the network console of your browser.

If you want to add a comment or leave a reaction (both implemented as **Mutations**),
you have to log in via the login page. There you will find usernames that you can use.

## 3. Execute queries in GraphiQL

You can use GraphiQL to execute GraphQL queries with the sample application.

Example 1:

```graphql
query {
    stories(first: 10) {
        edges {
            node {
                id
                title
                body
            }
        }
    }
}
```
### Authorization tokens (for mutations)

If you want to execute **mutations**, the backend expects a user token.
For testing, you can use one of the following tokens, which are always valid:

- User: `nils`
- Role: `ROLE_USER` (may do anything)

```JSON
{"Authorization": "Bearer eyJhbGciOiJSUzUxMiJ9. eyJzdWIiOiJ7XCJpZFwiOlwiVTFcIixcInVzZXJuYW1lXCI6XCJuaWxzXCIsXCJuYW1lXCI6XCJOaWxzIEhhcnRtYW5uXCIsXCJlbWFpbFwiOlwia29udGFrdEBuaWxzaGFydG1hbm4ubmV0XCIsXCJyb2xlc1wiOltcIlJPTEVfVVNFUlwiLFwiUk9MRV9FRElUT1JcIl0sXCJndWVzdFwiOmZhbHNlfSIsImlhdCI6MTY0MTM2OTg0NywiZXhwIjozMjMyOTg3OTg3fQ. lsLAqfAdMID_2QFL7OimWEHcunaPy18zWVYZUDiEJYVwR9PQG5qf8_gRrNAwkd9w33dwunJC3bswR1W0zMtTID9DyeaXGLws2AtmwF_ZecD6x5TVDEBcFJV1-WrRt2yRWoo-hNywGqDUqR49dJoICoQ-. aoP_7GOgYo5zYIYCRe2Xbn4DbX0xzGLyiRNzJzZhq8l6KE_Hb5Ern0hTZhTZXq4jmrCjf8wztuF37rsRJ-nZ9vozUaQU7vwhl93g1gvAMb3zJWBo_m9ujd3RKSZ5fjMuOVb-kVQI6NP9hLoEYO2mkcDSoHNIXBgJHr3TYNzeGtJ3Nt3bTXp-o_P-bSzXYQ"}
```

- User: `murphy`
- Role: `ROLE_GUEST` (may comment and respond, but may not create stories. Note: Stories can only be created via GraphiQL, not via the frontend).

```json
{"Authorization": "Bearer eyJhbGciOiJSUzUxMiJ9. eyJzdWIiOiJ7XCJpZFwiOlwiVTdcIixcInVzZXJuYW1lXCI6XCJtdXJwaHlcIixcIm5hbWVcIjpcIkV0aGVseW4gTXVycGh5XCIsXCJlbWFpbFwiOlwiZXRoZWx5bi5tdXJwaHlAZXhhbXBsZS5jb21cIixcInJvbGVzXCI6W1wiUk9MRV9VU0VSXCIsXCJST0xFX0dVRVNUXCJdLFwiZ3Vlc3RcIjp0cnVlfSIsImlhdCI6MTY0MTM2OTg0NywiZXhwIjozMjMyOTg3OTg3fQ. A1SHxkgGCfdo-v-kCGRSFuYngMW6438o1alkg4DAdwWBYuy1E7axYbpzGKghP5gR19b7qoc98Y9gY9-zekFxo35yrzDaEmWMYR0UmprYI27M_eh06OJzct2NJt9voldnUlPdCed8mn4vPs56IXHTxd6zGGwSA7JGYSIfswQh2w-. 3y1d3WCFR3ZPju0f9ZripR_4NQFOltm4NNbHC7CbcWgUtixJx-h5BiAeLfZcJDFoNqUq6obf8jkUzOX_2PEJaeRzxW6WTXd88EbSqjMns5PqmM5BosSJmyuZSjfGGbLaFBqPLrgMLNHHNFwGn6VBtarMvmHsU7zbGYmZXG31XAg"}
```

You can insert the complete Authorization-JSON-String in GraphiQL in the "**Request Headers**"-Tab.
The header will then be sent with every query/mutation.

![Set Request Header in GraphiQL](graphiql-request-header.png)

You can then for example query the current logged in user (represented by the token):

```graphql
query {
    me {
        id
        user {
            name
            email
        }
        
        bio
        skills
        currentlyLearning
    }
}
```

# Links

* GraphQL Homepage: https://graphql.org/
* IntelliJ Plug-in für GraphQL: https://plugins.jetbrains.com/plugin/8097-graphql
* GraphQL Extension für VS Code: https://marketplace.visualstudio.com/items?itemName=GraphQL.vscode-graphql
* GraphiQL: https://github.com/graphql/graphiql

# Contact

If you have questions or comments, please feel free to [contact me](https://nilshartmann.net) or open an issue in this repository. 
