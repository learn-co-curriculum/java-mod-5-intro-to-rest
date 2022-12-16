# Introduction to REST

## Learning Goals

- Build RESTful APIs that are easy to navigate and use in applications.

***

## Key Vocab

- **Representational State Transfer (REST)**: a convention for developing
  applications that use HTTP in a consistent, human-readable, machine-readable
  way.
- **Application Programming Interface (API)**: a software application that
  allows two or more software applications to communicate with one another.
  Can be standalone or incorporated into a larger product.
- **Resource**: A resource is an object with a type, associated data,
  relationships to other resources, and a set of methods that operate on it.
  Resources are addressable by URLs and HTTP methods can perform operations on resources.
- **HTTP Request Method**: assets of HTTP requests that tell the server which
  actions the client is attempting to perform on the located resource.
- **`GET`**: the most common HTTP request method. Signifies that the client is
  attempting to view the located resource.
- **`POST`**: the second most common HTTP request method. Signifies that the
  client is attempting to submit data to create a new resource.
- **`PATCH`**: an HTTP request method that signifies that the client is attempting
  to update a resource with new information.
- **`PUT`**: an HTTP request method that signifies that the client is attempting
  to update a resource with new information contained in a complete record.
- **`DELETE`**: an HTTP request method that signifies that the client is
  attempting to delete a resource.

***

## Introduction

In 2000, Roy Fielding was frustrated by the haphazard ways in which web
applications were using HTTP. Specifically, he was frustrated with how URLs
and their corresponding HTTP verbs were used differently for every single
application. So, in his [Ph.D. dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/fielding_dissertation.pdf),
he came up with **REST**n (**RE**presentational **S**tate **Transfer**) as a
standardized way for web applications to structure their URLs.

Fielding also noticed the rise in web applications communicating with each
other ([What is an API?][api]). He hoped that inter-application
communication would get much easier if there was a standard way of forming URLs
to access resources.

If you have been building applications for a while, there is a good chance that
you have already worked with RESTful APIs. If your application posts to Twitter, pulls in a feed
of images from Instagram, or calls a list of locations from Google Maps, you are
using a RESTful API to communicate between applications.

<details>
  <summary>
    <em>What is REST?</em>
  </summary>

<h3>REST is an architectural design pattern, not a framework or code in
itself.</h3>
  <p>Many other web frameworks utilize RESTful design principles in some form or
     another. By using RESTful principles, Java apps are able to have a clear
     and standardized naming structure for routes and actions.</p>
</details>
<br/>

***

## Example REST Workflow

For a real world case study, let us pretend that you have a newsletter
application. The following is a high-level view of how such an app might work:

1. You fill out the form on the 'New Newsletter' page and click submit.
2. Data concerning you as the author, your newsletter content, and any
   additional information such as multimedia items is sent to the application
   server.
3. The server interprets the information, recognizes that the request is for a
   new newsletter, generates the new record in the database, and performs
   background tasks (updating the newsletter counter, sending notification
   emails, etc).
4. Next, the server sends a response back to the client. This does not
   necessarily mean that the newsletter was posted. The response could be that
   there was an error posting, or something along those lines. However, in this
   example we will say that the post went through properly, so the server sends
   a 201 response code and tells the browser which page to go to and render.
5. Lastly, the browser receives the server information and shows you a message
   saying that your newsletter was successfully posted.

***

## RESTful Conventions

Let's take a look at how this would work for our newsletter application. RESTful
routes fall into four familiar categories: Create, Read, Update, and Delete.
These will roughly translate to five routes:

| Category             | Action                                 |
|----------------------|----------------------------------------|
| Create               | Create the new newsletter instance.    |
| Retrieve All (Index) | Display a list of all newsletters.     |
| Retrieve One         | Display an individual newsletter.      |
| Update               | Update the newsletter instance.        |
| Delete               | Delete an existing newsletter instance |

***

## HTTP Request Methods and RESTful Routes

To implement each of the above actions, we combine an HTTP request method (or
_HTTP verb_) such as `GET` or `POST` with a route. A Java servlet would
need to map implement each method/route combination by overriding
the appropriate  method (i.e. `doGet()`, `doPost()`, etc). The
table below shows the HTTP request method and route we could use for this
RESTful newsletter app:

| HTTP Request Method  | Route               | Description                   |
|----------------------|---------------------|-------------------------------|
| GET                  | `/newsletters`      | Show all newsletters.         |
| POST                 | `/newsletters`      | Create a new newsletter.      |
| GET                  | `/newsletters/{id}` | Show a specific newsletter.   |
| PATCH or PUT         | `/newsletters/{id}` | Update a specific newsletter. |
| DELETE               | `/newsletters/{id}` | Delete a specific newsletter. |

Note that even though we have five separate actions, we only have two routes
defined in our application: `/newsletters` and `/newsletters/{id}`.
A Java servlet will use the `@WebServlet` annotation to capture both routes as shown:

```java
@WebServlet(urlPatterns="/newsletters/*")
```

While a Java servlet needs to do some explicit coding to get the path parameter `id`
from the route, we will see in subsequent lessons how Spring Boot's `@PathVariable` annotation
makes this very simple and effective.

<details>
  <summary>
    <em>How do RESTful routes benefit us as developers?</em>
  </summary>

<h3>RESTful routes have a clear mapping between the URL resource and the
corresponding backend actions.</h3>
</details>
<br/>

### Review of HTTP Request Methods

So what do `GET`, `POST`, et al. represent? These HTTP verbs give each HTTP
request unique behavior. Below is an explanation of each verb:

- **GET**: The GET method retrieves whatever information is identified by the
  Request URI. This means if you go to `/newsletters`, you will get all
  newsletter posts that the application has.

- **POST**: The POST method is used to send data enclosed in the request to the
  server. The server is expected to use this data to create some new resource.

- **PATCH/PUT**: The PATCH and PUT methods are both used to update existing
  resources. Sending either a `PATCH` or `PUT` request to `/newsletters/1` will
  update the post with an `id` of 1. `PUT` is used when we want to replace an
  entire resource. `PATCH` is used when we want to update a specific part of a
  resource. Check out this explanation of the [difference between PUT and PATCH][put-v-patch].

- **DELETE**: The DELETE method requests that the server delete the resource
  identified by the Request URI. This means… that it deletes the record. It's
  nice and explicit.

***

## Things to Keep in Mind throughout this Module

Below are a few keys to remember when thinking about REST:

- REST is an architectural design pattern, not a framework or code in itself.
  Many other web frameworks utilize RESTful design principles in some form or
  another. By using RESTful principles, Java apps are able to have a clear and
  standardized naming structure for routes and actions.

- RESTful routes have a clear mapping between the URL resource and the
  corresponding actions carried out by the backend.

- There are five RESTful route options we will commonly use as API developers.

<details>
  <summary>
    <em>What are the five RESTful route options we've discussed?</em>
  </summary>

<h3>Create</h3>
<h3>Retrieve All (Index)</h3>
<h3>Retrieve One</h3>
<h3>Update</h3>
<h3>Delete</h3>

</details>
<br/>

***

## Resources

- [What RESTful Actually Means](https://codewords.recurse.com/issues/five/what-restful-actually-means)
- [What is an API? - MuleSoft][api]
- [HTTP request methods - Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

[api]: https://www.mulesoft.com/resources/api/what-is-an-api
[put-v-patch]: https://www.geeksforgeeks.org/difference-between-put-and-patch-request/