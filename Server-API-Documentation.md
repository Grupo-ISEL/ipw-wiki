# Movies endpoint

| Path               | Method | Summary         | Description     | Responses                                                                                   |
|--------------------|--------|-----------------|-----------------|---------------------------------------------------------------------------------------------|
| `/api/movies`      | `GET`  | Get Movies      | Get movies      | 200 application/json: SearchResults, 400 application/json: InvalidRequest                   |
| `/api/movies/{id}` | `GET`  | Get Movie by ID | Get movie by id | 200 application/json: Movie, 400 application/json: InvalidRequest, 404 application/json: {} |
| `/api/movies/top`  | `GET`  | Get Top Movies  | Get top movies  | 200 application/json: array of TopMovie, 400 application/json: InvalidRequest               |
### Parameters

| Name   | In    | Description                        | Required | Schema          |
|--------|-------|------------------------------------|----------|-----------------|
| search | query | Movie title to search              | true     | string          |
| limit  | query | Number of returned movie (max 250) | false    | integer (0-250) |
| offset | query | Offset to return movies            | false    | integer (0-250) |


`getMoviesId`

Parameters

| Name | In   | Description       | Required | Schema |
|------|------|-------------------|----------|--------|
| id   | path | Movie id to fetch | true     | string |


`getTopMovies`

### Parameters

| Name   | In    | Description                         | Required | Schema          |
|--------|-------|-------------------------------------|----------|-----------------|
| limit  | query | Number of returned movies (max 250) | false    | integer (0-250) |
| offset | query | Offset to return movies             | false    | integer (min 0) |


# Users endpoint

### POST /api/users

Create new user

#### Operation ID: addUser

#### Summary

Adds a user to the system

#### Request Body

New User

- Required: true
- Content-Type: application/json
- Schema: `#/components/schemas/NewUser`

#### Responses

| Status Code | Description  | Content-Type     | Schema                                |
|-------------|--------------|------------------|---------------------------------------|
| 201         | User created | application/json | `#/components/schemas/NewUserCreated` |
| 400         | Bad request  | application/json | `#/components/schemas/InvalidRequest` |

# Groups endpoint

| Path               | Method | Security   | Summary         | Description                           | Responses                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|--------------------|--------|------------|-----------------|---------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `/api/groups`      | GET    | bearerAuth | List all groups | List all groups belonging to the user | - 200: All groups - Array of [Group](#/components/schemas/Group)                                         \n- 400: Bad request - [InvalidRequest](#/components/schemas/InvalidRequest)                          \n- 401: Not Authorized - [NotAuthorized](#/components/schemas/NotAuthorized)                                                                                                                                                                |
| `/api/groups`      | POST   | bearerAuth | Create group    | Create group                          | - 200: Group created - [NewGroupCreated](#/components/schemas/NewGroupCreated)                         \n- 400: Bad request - [InvalidRequest](#/components/schemas/InvalidRequest)                          \n- 401: Not Authorized - [NotAuthorized](#/components/schemas/NotAuthorized)                                                                                                                                                                  |
| `/api/groups/{id}` | GET    | bearerAuth | Get group by ID | Get group by ID                       | - 200: OK - [Group](#/components/schemas/Group)                                                         \n- 400: Bad request - [InvalidRequest](#/components/schemas/InvalidRequest)                          \n- 401: Not Authorized - [NotAuthorized](#/components/schemas/NotAuthorized)                          \n- 403: Forbidden - [Forbidden](#/components/schemas/Forbidden)                                         \n- 404: Group not found - {} |
| `/api/groups/{id}` | PUT    | bearerAuth | Edit group      | Edit group                            | - 200: Group edited - [NewGroupCreated](#/components/schemas/NewGroupCreated)                          \n- 400: Bad request - [InvalidRequest](#/components/schemas/InvalidRequest)                          \n- 401: Not Authorized - [NotAuthorized](#/components/schemas/NotAuthorized)                          \n- 403: Forbidden - [Forbidden](#/components/schemas/Forbidden)                                         \n- 404: Group not found - {}  |
| `/api/groups/{id}` | DELETE | bearerAuth | Delete group    | Delete group                          | - 200: Group deleted - [NewGroupCreated](#/components/schemas/NewGroupCreated)                         \n- 400: Bad request - [InvalidRequest](#/components/schemas/InvalidRequest)                          \n- 401: Not Authorized - [NotAuthorized](#/components/schemas/NotAuthorized)                          \n- 403: Forbidden - [Forbidden](#/components/schemas/Forbidden)                                         \n- 404: Group not found - {}  |


## Schemas

### Movie

A Movie object has the following properties:

| Property    | Type    | Description                                                                 | Example                                                           |
|-------------|---------|-----------------------------------------------------------------------------|-------------------------------------------------------------------|
| id          | string  | Movie ID                                                                    | "tt0111161"                                                       |
| title       | string  | Movie title                                                                 | "The Shawshank Redemption"                                        |
| year        | integer | Movie release year                                                          | 1994                                                              |
| imdbRating  | number  | Movie IMDB rating                                                           | 9.3                                                               |
| runtimeMins | integer | Movie runtime in minutes                                                    | 142                                                               |
| imageUrl    | string  | Movie image URL                                                             | "https://image.tmdb.org/t/p/w500/9O7gLzmreU0nGkIB6K3BsJbzvNv.jpg" |
| directors   | string  | Movie director(s)                                                           | "Frank Darabont"                                                  |
| writers     | string  | Movie writer(s)                                                             | "Stephen King"                                                    |
| actors      | array   | List of movie actors. The type of each actor is defined in the Actor schema |                                                                   |

### SearchResults

A SearchResults object has the following properties:

| Property    | Type   | Description                                                                                      | Example                                                           |
|-------------|--------|--------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| movies      | array  | List of movie search results. Each item in the array is an object with the following properties: |                                                                   |
| id          | string | Movie ID                                                                                         | "tt0111161"                                                       |
| title       | string | Movie title                                                                                      | "The Shawshank Redemption"                                        |
| description | string | Group description                                                                                | "My favorite movies"                                              |
| imageUrl    | string | Movie image URL                                                                                  | "https://image.tmdb.org/t/p/w500/9O7gLzmreU0nGkIB6K3BsJbzvNv.jpg" |

### TopMovie

A TopMovie object has the following properties:

| Property | Type    | Description     | Example                    |
|----------|---------|-----------------|----------------------------|
| id       | string  | Movie ID        | "tt0111161"                |
| title    | string  | Movie title     | "The Shawshank Redemption" |
| rank     | integer | Movie IMDB rank | 10                         |

### MovieAddedToGroup

A MovieAddedToGroup object has the following properties:

| Property | Type   | Description                               | Example                    |
|----------|--------|-------------------------------------------|----------------------------|
| status   | string | Status                                    | "New movie added to group" |
| group    | object | Group object, defined in the Group schema |                            |

### MovieRemovedFromGroup

A MovieRemovedFromGroup object has the following properties:

| Property | Type   | Description                               | Example                    |
|----------|--------|-------------------------------------------|----------------------------|
| status   | string | Status                                    | "Movie removed from group" |
| group    | object | Group object, defined in the Group schema |                            |

### Group

A Group object has the following properties:

| Property | Type   | Description | Example |
|----------|--------|-------------|---------|
| id       | string | Group ID    | "1"     |

