# An Introduction to GraphQL for REST API Developers


## Why GraphQL?

Before jumping into GrapQl, let us analyze a scenario for REST API.

In REST APIs, we have resources. Each resource has an ID and methods to create, update, delete.
Consider a situation; we have three models, company, employee, and task.  A company has many employees, has many tasks, a task has many employees. And also, let's say each task has a supervisor who is an employee.

<br/>

We can assume that we have GET, POST, PUT, PATCH, DELETE supported endpoints for all the resources.

**Scenario 1:** Get company information for an ID. Call company endpoint with Method type GET and ID in parameters.

**Scenario 2:** Get company name for ID. Scenario 1 works fine, but we will still be getting all data, and we'll have to filter data at the client-side, which is not generally ideal even if data is not sensitive. One solution is to add an extra parameter in the GET function to tell what information we need, and our server can send only those data. i.e., { "only": ["name"]}

**Scenario 3:** Get Employee details, the task he is working on, and the task details. One option we can see here is to call GET for the ID, look for Task ID in the response make another request to get supervisor ID from task ID. Send a nested response from the API itself.

For the above three scenarios, there are many exciting approaches in REST itself. Still, as the size and complexity of the application grow, you will be finding yourself writing some dedicated specific purpose APIs.

## How GraphQL solves the problem.

GraphQL gives you an option to write query in the frontend. So the concept is requester of data will query the data they need to curate it, instead of a controller doing it for them.

It doesn't have an easy learning curve as REST API, but once you are comfortable with it and the project boilerplate is ready, it will be much easier to build upon.

## Basic Idea of implementing a GraphQL Backend.

In GraphQL, you'll have only one endpoint. Your frontend application will always request to that single endpoint with a GQL. With the GQL ( Graph Query Language), the backend will identify what data is needed and send it.

The server should be able to understand GQL; else, it will not understand the request. So we will have to add the capability to the server. To do this, we need to add a GraphQL Implementation to our backend. For NodeJs, [Apollo](https://www.apollographql.com/docs/apollo-server/) is an option; for Rails, there is a gem called [graphql](https://graphql-ruby.org/). Feel free to find the perfect graphql implementation for your backend; there are many.

### You'll need a minimum of three steps to have basic crud.

* Define Schema: This part is just like defining models. It will let your graphql engine know what data are available. It will feel repetitive as we already have models with similar code, but it will be worth it once it's done.

* Write Query Resolvers: In this step, you will how to get the data. It is basically like the get method of the controller but a bit complex to implement because you may want to generalize things to handle joins, and single data and array of data, etc.

* Write Mutation Resolvers: This is same as Query Resolvers, but instead, for fetching data, it will be for adding and editing data.

After these steps, you can test it using [GraphiQl](https://graphql-dotnet.github.io/docs/getting-started/graphiql/), just like PostMan for REST.

## Benifit of using GraphQL

This all feels like a lot of work compared to developing simple APIs. So there should be some benefits to adopt graphQl.

- You'll always fetch the data you need. You don't have to use an API that sends some extra data that you don't need.

- Option to batch requests from the front end. As all requests are directed to a single endpoint and contain queries in the same language, these can be clubbed in a single request, and GraphQL can handle multiple queries in one request.

- If you are developing SPA, using component-based framework batching will be helpful as it can fetch data for each component at once, so a user won't be waiting for half of the page to load. Many GraphQL Client libraries have this inbuilt, and some can also split if a query is too big for the first contentful paint.

- As the backends schema matures, the developer will find touching backend code less and less as most of the front data needs can be handled by GQL. Much dynamic UI can be built without the need for API to handle every minor thing.

There are some other features to look for, such as subscriptions, authorizations, validation, pagination, etc., which will be helpful in the development lifecycle.

I'll highly recommend getting an overview of the fundamentals from [https://graphql.org/learn/](https://graphql.org/learn/) and judge if it's a viable option for your current or next project.

<br/>


{{< admonition tip "Guest Post" >}}
Shoutout to my friend Vikas for finally giving me this writeup :), I haven't got a chance
to use GraphQL yet, but I see good enough reason to explore it soon. I hope you too are up for the same.
Also, Vikas has been writing blogs for some time, do check out his page [geekyhub](https://www.geekyhub.in/), there is some brilliant content in there.
{{< /admonition >}}

