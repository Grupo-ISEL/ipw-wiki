# Requirements
To run the application and tests, you will need to have the following installed on your machine:

1. [Node.JS](https://nodejs.org/en/)
2. [NPM](https://www.npmjs.com/get-npm)
3. [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html)

# Running the Application

1. Install package dependencies if not already installed
```bash
npm install
```
2. Optionally set the `ELASTIC_URL` environment variable. If this is not set the default value `http://localhost:9200` will be used.
```bash
export ELASTIC_URL='http://localhost:9200'
```
3. Optionally set the `IMDB_API_KEY` environment variable. If this is not set, a default API key will be used (it's not guaranteed the default key works, so a new key is recommended.
```bash
export IMDB_API_KEY='http://localhost:9200'
```
4. Run the application 
```bash
npm start
```
or
```bash
node cmdb-server.mjs
```

## Running Unit and Integration Tests

1. Install package dependencies if not already installed
```bash
npm install
```
2. Run the tests with `npm test`
```bash
npm test
```

### Unit Tests
The Unit tests will run with mock user and group data provided by in-memory implementation in `cmdb-data-mem.mjs`.
The `movies` mock data is provided by the simulated IMDB-API in `imdb-mock-data.mjs`.
This module is used as a `fetch` replacement in the `imdb-movies-data.mjs` module.
This is achieved by dependency injection when initializing the data layer.
Unit tests target the services layer specifically. 

### Integration Tests
The Integration tests will run with real user and group data provided by the Elasticsearch implementation in `cmdb-data-elasticsearch.mjs`.
These will simulate an end-to-end request-response cycle between a client and the server.
All API endpoints available in the application are tested.


# Seeding the App with Data

A seed script is provided with the app that populates ElasticSearch with `users` and `groups` data. The script is designed to work if no conflicting data exist in ElasticSearch. Removing the `users` and `groups` indices before running the script is preferred.
The movie IDs must be valid as per the IMDB API module. This is important when using a simulated IMDB API that might not have all the movies on the seed data.
Alternatively, one can use the real IMDB API data and provide a valid API key.

Examples for both scenarios are provided below:

Using simulated IMDB API data
```
import {MAX_LIMIT} from "./services/cmdb-services-constants.mjs"
import moviesDataInit from "./data/imdb-movies-data.mjs"
import mockFetch from "./data/imdb-mock-data.mjs"

const moviesData = moviesDataInit(mockFetch, "k_1234abcd", MAX_LIMIT)
```

Using live IMDB API data
```
import {MAX_LIMIT} from "./services/cmdb-services-constants.mjs"

import moviesDataInit from "./data/imdb-movies-data.mjs"
import fetch from "node-fetch"

const moviesData = moviesDataInit(mockFetch, 'k_0v6pmbzj', MAX_LIMIT)
```

Once the user has set up the IMDB API module, they can follow these instructions to use the seed app:

1. Download or clone the seed app files to the project root in your local machine and open a command prompt or terminal window.

2. Run the following command: 

```
npm install
```

This will install any dependencies required.

4. Run the following command to start the seed script: 

```
node seed.mjs <filename> <elasticUrl>
```

Replace `<filename>` with the JSON file name containing the data to be seeded and `<elasticUrl>` with the URL of the Elasticsearch instance to be used for storing the data. 

For example:

```
node seed.mjs seed.json http://localhost:9200
```

5. The seed script will read the JSON data from the specified file and create users, groups, and movies in the Elasticsearch instance. The app will output messages indicating the progress of the seeding process, such as "Created user" and "Added movie to group".

6. Once the seed app has finished running, you can verify that the data has been seeded by querying the Elasticsearch instance or using the Web App API or website.
