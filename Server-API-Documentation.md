# Movies endpoint
## Get Movies

### Path

`/api/movies`

### Method

`GET`

### Summary

Get movies

### Description

Get movies

### Operation ID

`getMovies`

### Parameters

| Name   | In    | Description                        | Required | Schema          |
|--------|-------|------------------------------------|----------|-----------------|
| search | query | Movie title to search              | true     | string          |
| limit  | query | Number of returned movie (max 250) | false    | integer (0-250) |
| offset | query | Offset to return movies            | false    | integer (0-250) |

### Responses

#### 200

Description: Search results matching criteria

Content:

- application/json:
    - schema: [SearchResults](#/components/schemas/SearchResults)

#### 400

Description: Bad request

Content:

- application/json:
    - schema: [InvalidRequest](#/components/schemas/InvalidRequest)

## Get Movie by ID

### Path

`/api/movies/{id}`

### Method

`GET`

### Summary

Get movie by id

### Description

Get movie by id

### Operation ID

`getMoviesId`

### Parameters

| Name | In   | Description       | Required | Schema |
|------|------|-------------------|----------|--------|
| id   | path | Movie id to fetch | true     | string |

### Responses

#### 200

Description: Movie details

Content:

- application/json:
    - schema: [Movie](#/components/schemas/Movie)

#### 400

Description: Bad request

Content:

- application/json:
    - schema: [InvalidRequest](#/components/schemas/InvalidRequest)

#### 404

Description: Movie not found

Content:

- application/json: {}

## Get Top Movies

### Path

`/api/movies/top`

### Method

`GET`

### Summary

Get top movies

### Description

Get top movies

### Operation ID

`getTopMovies`

### Parameters

| Name   | In    | Description                         | Required | Schema          |
|--------|-------|-------------------------------------|----------|-----------------|
| limit  | query | Number of returned movies (max 250) | false    | integer (0-250) |
| offset | query | Offset to return movies             | false    | integer (min 0) |

### Responses

#### 200

Description: Get top movies

Content:

- application/json:
    - schema: array of [TopMovie](#/components/schemas/TopMovie)

#### 400

Description: Bad request

Content:

- application/json:
    - schema: [InvalidRequest](#/components/schemas/InvalidRequest)

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

# Groups API endpoint

## Endpoint: `/api/groups`

### GET `/api/groups`

List all groups belonging to the user

#### Security

- bearerAuth: [ ]

#### Responses

| Code | Description    | Schema                                                |
|------|----------------|-------------------------------------------------------|
| 200  | All groups     | Array of [Group](#/components/schemas/Group)          |
| 400  | Bad request    | [InvalidRequest](#/components/schemas/InvalidRequest) |
| 401  | Not Authorized | [NotAuthorized](#/components/schemas/NotAuthorized)   |

### POST `/api/groups`

Create group

#### Security

- bearerAuth: [ ]

#### Request Body

- Description: Group to create
- Required: true
- Content: `application/json`
    - Schema: [NewGroup](#/components/schemas/NewGroup)

#### Responses

| Code | Description    | Schema                                                  |
|------|----------------|---------------------------------------------------------|
| 200  | Group created  | [NewGroupCreated](#/components/schemas/NewGroupCreated) |
| 400  | Bad request    | [InvalidRequest](#/components/schemas/InvalidRequest)   |
| 401  | Not Authorized | [NotAuthorized](#/components/schemas/NotAuthorized)     |

## Endpoint: `/api/groups/{id}`

### GET `/api/groups/{id}`

Get group by ID

#### Security

- bearerAuth: [ ]

#### Path Parameters

- `id`: Group ID (integer, required)

#### Responses

| Code | Description     | Schema                                                |
|------|-----------------|-------------------------------------------------------|
| 200  | OK              | [Group](#/components/schemas/Group)                   |
| 400  | Bad request     | [InvalidRequest](#/components/schemas/InvalidRequest) |
| 401  | Not Authorized  | [NotAuthorized](#/components/schemas/NotAuthorized)   |
| 403  | Forbidden       | [Forbidden](#/components/schemas/Forbidden)           |
| 404  | Group not found | {}                                                    |

### PUT `/api/groups/{id}`

Edit group

#### Security

- bearerAuth: [ ]

#### Path Parameters

- `id`: Group ID (integer, required)

#### Request Body

- Description: Group details to edit
- Required: true
- Content: `application/json`
    - Schema: [NewGroup](#/components/schemas/NewGroup)

#### Responses

| Code | Description     | Schema                                                  |
|------|-----------------|---------------------------------------------------------|
| 200  | Group edited    | [NewGroupCreated](#/components/schemas/NewGroupCreated) |
| 400  | Bad request     | [InvalidRequest](#/components/schemas/InvalidRequest)   |
| 401  | Not Authorized  | [NotAuthorized](#/components/schemas/NotAuthorized)     |
| 403  | Forbidden       | [Forbidden](#/components/schemas/Forbidden)             |
| 404  | Group not found | {}                                                      |

### DELETE `/api/groups/{id}`

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

