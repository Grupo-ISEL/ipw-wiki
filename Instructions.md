## Requirements
To run the application and tests, you will need to have the following installed on your machine:

1. [Node.JS](https://nodejs.org/en/)
2. [NPM](https://www.npmjs.com/get-npm)
3. [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html)

## Running the Application

1. Install package dependencies if not already installed
```bash
npm install
```
2. Run the application 
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
The movies mock data is provided by the simulated IMDB-API in `imdb-mock-data.mjs`.
This module is used as a `fetch` replacement in the `imdb-movies-data.mjs` module.
This is achieved by dependency injection when initializing the data layer.
Unit tests target the services layer specifically. 

### Integration Tests
The Integration tests will run with real user and group data provided by the Elasticsearch implementation in `cmdb-data-elasticsearch.mjs`.
These will simulate an end-to-end request-response cycle between a client and the server.
All API endpoints available in the application are tested.