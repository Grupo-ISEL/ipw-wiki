# Overview

The Chelas Movie Database (CMDB) web application is a [Node.JS](https://nodejs.org/en/) application with a [RESTful Web API](https://en.wikipedia.org/wiki/JSON) and a website built with [HTML](https://html.spec.whatwg.org/multipage/), [CSS](https://www.w3.org/Style/CSS/), [Handlebars](https://en.wikipedia.org/wiki/JSON), and [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript). The web app has three distinct layers: the web layer, the services layer, and the data layer using [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html) for data persistence and the [IMDB-API](https://imdb-api.com/) for movie information.

![image](https://user-images.githubusercontent.com/36137130/220464191-5b625d86-a07a-482d-a809-d9f717d235c1.png)

# Server Components

## Web Layer

The web layer comprises two components: the web API and the website. The web API uses the [Express framework](https://expressjs.com/) and uses the [JSON](https://en.wikipedia.org/wiki/JSON) format for responses. It exposes RESTful endpoints for managing users and groups and searching and retrieving movie data from the IMDB API. All API endpoints are available under the HTTP path prefix `/API` to provide a clear and meaningful separation between the API and Website routes.

The website is built using HTML, CSS, Handlebars, and JavaScript. It is designed to provide a user interface for the functionality provided by the web API. The website renders pages, handles user input, and communicates with the web API (when appropriate ) to fetch or update data. The Handlebars templating engine renders (server side) dynamic content on the website's pages.

## Services Layer

The services layer implements all the business logic for managing users, groups, and movies. It consists of several services that handle different aspects of the application's functionality, such as authentication, user management, group management, and movie management. Authentication is implemented using a bearer token, generated when a user is created, and is required to access specific API endpoints.

## Data Layer

The data layer is responsible for accessing the application's data storage. The application uses Elasticsearch as the data store for users and groups. The data access layer uses a custom module that interacts with the ElasticSearch REST API.

In addition to ElasticSearch, the application provides an alternative implementation of the data persistence layer, in memory, used only for testing purposes. The in-memory data store is used for automated testing and provides a simple, lightweight alternative to ElasticSearch.

# Client Components

The client components of the CMDB application are implemented using HTML, CSS, Handlebars, and JavaScript. The website provides a user interface that allows users to sign up for the website, manage their groups, search for movies, and access other functionality provided by the web API.

The client renders the HTML sent by the server. Some functionality on the website is implemented using HTML input forms, which use the HTTP POST method. However, certain functionality that requires the HTTP PUT or DELETE method is implemented on the client side by JavaScript code, which makes calls to the API using the appropriate HTTP method.
The client-side JavaScript handles these alternative user interactions with the website and communicates with the web API to fetch or update data. 

The website uses a traditional username/password combo to authenticate the user, sent to the server via an input form. Upon successful authentication, the server retrieves the user bearer token, which is stored in the user's session and is used to authenticate subsequent requests. The client-side JavaScript code uses the bearer token to authenticate API requests where appropriate.

The user session is also used to cache group and movie data to avoid unnecessary API calls, particularly to the IMDB API that imposes a daily request limit on the free tier access used by the web app.


# Conclusion

The CMDB web application has a well-defined structure that separates the application's concerns into distinct layers. This architecture allows the application to be easily maintained and updated as new functionality is added. By separating the client and server components, the application can provide a seamless user experience while providing a flexible and scalable backend.
