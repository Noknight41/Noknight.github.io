---
title: Make REST API more RESTful
subtitle: A Handbook for Building RESTful API
date: 2023-08-01T09:46:45+07:00
tags: ["Blog", "REST-API", "EN"]
---

In the world of web applications, REST APIs are bridges that connect different software and devices, allowing them to work together. In this beginner-friendly handbook, we'll explore the best practices for designing RESTful APIs, making it easier for you to create powerful and user-friendly applications.

<!--more-->

## What to expect ?

Before we get started, let's take in consideration a few things:

**Every project is unique and may require different approaches.** Sometimes, certain conventions might not be the best fit for your specific situation. As an engineer, it's essential to assess what works best for your project or discuss it with your team. Flexibility is key!

**Best practices are not rules or laws you must follow.** They are helpful tips and conventions that have proven to work well over time. Some of these practices have become standard in the industry, but you can always adapt them to fit your specific needs.

**The purpose of best practices is to guide you in creating better APIs.** They focus on improving user experience for both the consumers (people using the API) and the developers (people buidling and maintaining it). They also help enhance the security and performance of your API.

Now that we've got those things out of our way, let's get to the main part of our topic!

## Versioning

Like in other applications there will be improvements, new features, and stuff like that. 
As you improve your API over time, you might make changes that could affect existing users. So it's important to know about versioning our API as well. 

The big advantage of versioning is that we can work on new features or improvements on a new version while the clients are still using the current version and are not affected by breaking changes.
We also don't force the clients to use the new version straight away as they can use the current version and migrate when the new version is stable.

A good practice is to add the version name in a path segment of the API (commonly named v1, v2)

```
// API X version 1
"/api/v1/X"

// API X version 2
"/api/v2/X"
```

When developing and writing code, I recommend creating sub-folders with each version (v1, v2), moving files that exclusive to certain version into that sub-folder.

## Naming Resources in Plural

When implementing endpoints, I always like to start with the routes first, specifically how we can name our endpoints.
For example, when we make a POST API that create a new room, we could make the endpoint /api/v1/room to make clear that this API is used to add one room, right ?
Well, yes, there isn't anything wrong with this approach but it can cause confusion when we work with resources.

By the same logic, when we make a GET API that get every rooms in the building, the endpoint would become /api/v1/rooms and now we have 2 different endpoint that interact with the same resource.

```
GET "/api/v1/rooms";
POST "/api/v1/room";
```

Remember our API is used by other people and should be clear and consistent to avoid misunderstanding. This applies especially to naming your API.
Naming your resources in plural has the big advantage that it's crystal clear to other people, that this is a collection of different entities.

## Avoid Verbs in Endpoint

Let's take a look how our URL's would look like if we had used verbs versus we don't use verbs:

Without verbs:
```
GET "/api/v1/rooms" ;
GET "/api/v1/rooms/:roomId";
POST "/api/v1/rooms";
PATCH "/api/v1/rooms/:roomId";
DELETE "/api/v1/rooms/:roomId";
```

With verbs:
```
GET "/api/v1/getAllRooms";
GET "/api/v1/getRoomsById/:roomId";
POST "/api/v1/createRoom";
PATCH "/api/v1/updateRooms/:roomId"; 
DELETE "/api/v1/removeRoom/:roomId";
```

Do you see the difference ? There are no uniformed pattern between the endpoint of each behavior on a single resource. 
And while the naming convention making sense on a human speaking level,
imagine we are using or documenting a system having hundred of endpoint that interacts with many resource,
having a completely different URL for every behavior become confusing and unnecessarily complex pretty fast.

In REST, every URL should lead to a specific resource, and we use HTTP methods (or "verbs") to work with those resources.
So it's not helpful and doesn't make sense in the RESTful approach since HTTP verb itself already indicates the action.
Keep it simple and stick to using the right HTTP methods for actions on your resources:
- GET: To get information from the server (like reading a book).
- POST: To create new resources on the server (like adding a new entry to a book).
- PUT: To update existing resources on the server (like editing a book's content).
- DELETE: To remove resources from the server (like tearing out a page from a book).

## Speak JSON

When you use an API, you exchange information by sending data in your request with specific information or receiving data in the response.
There are various data formats we can use, but JSON is a widely recognized and standardized format. You can build your API with any backend framework,
and they most likely can handle JSON too. 

Because of its widespread acceptance and standardization, APIs should use JSON format for both accepting requests and sending responses.
This ensures compatibility and consistency across different systems.

## Handling Error

In theory, everything should be working perfectly without problem. However, reality is often dissapointing and a lot of errors can happen – either from a human or a technical perspective.
We should handle certain cases that might go wrong or throw an error. This will also keep our API as close as the use case requirement.

When something goes wrong (either from the hardware, the internal logic, the internet connection, consumer's flawed input, etc...) we send HTTP Error codes back.
some APIs return a generic 400 error code for failed requests without providing any helpful message about why the error occurred. 
This lack of information can make debugging the issue a real challenge and cause unnecessary pain for developers trying to fix the problem.

That's why it's always a good practice to return proper HTTP error codes for different cases. This helps the consumer or the engineer who built the API to identify the problem more easily.
Since there are so many code to name all of them, I will link the [Standard HTTP response code Document](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) right here.

Normally, we will be tempted to also return a quick error message specify which part caused the error, however,
it is not very wise to always to do this since you might provide information about your system that you should really hide. 

## Logical Nesting, When ?

When you're designing your API, there might be cases where you have resources that are associated with others.
It's a good practice to group them together into one endpoint and nest them properly.

Let's imagine there are 3 entities in our problem: **Rooms** for people to stay in, **Customers** stay in room and **Bills** that Customers pay for Rooms. Now, the frontend needs an endpoint that responds with all Bills for a specific Room in order to display it in the UI.

The Rooms, the Customers, and the Bills are stored in different places in the database. Since the URL itself doesn't necessarily have to mirror the database structure,
the URI for that endpoint can be `/api/v1/rooms/:roomId/bills`

```
GET "/api/v1/rooms/:roomId/bills"
```

Theoretically you can nest it how deep you want, but the rule of thumb here is to avoid requiring resource URIs more than 3 level deep or `collection/item/collection`.
If you find yourself needing to nest deeper than that, it's better to split the endpoint into two smaller ones. This approach not only makes the API easier to use but also improves its documentation and maintainability.

For example, if the frontend need to find the Customer who pay for a bill in the Rooms, of course we could implement the following URI:
```
GET "/api/v1/rooms/:roomId/bills/:billId/cutsomers"
```

The endpoint now becomes less manageable the more nesting we add to it, so we should split this endpoint into the `/api/v1/rooms/:roomId/bills` and `/api/v1/customers/:customerId` with assumption that the first endpoint also return the customerId in the Bill data.

Which also bring up the non-nested approach. Obviously, the frontend can solve 2 previuos problem by using endpoint `/api/v1/bills?roomId=x` and `/api/v1/customers/:customerId` if the frontend have the specific ID to call, which shows that non-nested design seems to allow a more flexible and simpler endpoint schema. However, the former approach can tricky for documentation if there are too many query parameters and the latter can be hard to access since we typically don't want to public entities ID on the URL.

Both design strategy are valid in their own right and require careful consideration before implementation, which is why it is important to understand the trade-offs of each strategy.

## Filtering, Sorting & Pagination

Improving the performance of our API is crucial, and one way to achieve this is by incorporating filtering, sorting, and pagination, which are essential elements on my checklist.

Filtering is especially useful as it allows us to extract specific data from a large collection. For instance, it enables a feature where we can obtain all rooms located on Floor 1.

```
GET "/api/v1/rooms?floor=1"
```

Pagination is another mechanism to split our whole collection of rooms into multiple pages where each page only consists of 20 rooms, for example. This technique helps us to make sure that we don't send more than 20 rooms at the same time with our response to the client. 
For instance, it enables a feature where we can obtain the second page of the Rooms collection (assume each page has the length 20 rooms).

```
GET "/api/v1/rooms?pageId=2"
```

Sorting in a collection can be challenging, especially when it becomes too complicated for the frontend to handle. To overcome this challenge effectively, it's better to handle the sorting process in our API and send the sorted data directly to the client.

```
GET "/api/v1/room?sort=-name"
```

## Caching 

Caching can significantly improve API performance by reducing server load and minimizing response times.
When the data is an often requested resource or/and querying that data from the database is a heavy lift and may take multiple seconds. 
You can store this type of data inside your cache and serve it from there instead of going to the database every time to query the data.

Here are a few important things to consider when using a cache:

* Always ensure that the data stored in the cache is up-to-date to avoid serving stale and outdated information.

* When the cache is being filled during the processing of the first request and subsequent requests come in, we need a mechanism to decide whether to delay those requests and serve data from the cache or provide data directly from the database like the first request.

* Using a distributed cache like Redis adds another moving part to your infrastructure, so consider if it's really necessary and if it makes sense for your specific use case.

My preferred approach is to start simple and clean when building APIs. I usually don't include a cache right away unless there are  use-cases for it. Instead, I observe the API's performance over time, and if the need arises, I can implement caching later on. 
This approach allows me to keep the initial development straightforward and add optimizations as needed.

## Security

We've discussed best practices to enhance the usability and performance of our API. However, security is also a key factor for API design.
Even if you create an excellent API, it can become useless and even risky if it's vulnerable to attacks while running on a server.

The absolute must have is to use SSL/TLS because it's a standard nowadays on the internet. It's even more important for API's where private data is send between the client and our API.

If you've got resources that should only be available to authenticated users, you should protect them with an authentication check from, for instance, a middleware for specific routes and check first if the request is authenticated before it accesses a resource.

For more in-depth information and additional best practices on API security, I recommend reading [this article](https://restfulapi.net/security-essentials/). It can provide valuable insights to ensure your API remains safe and robust.

## Documentation

> "An API is just as good as it's documentation"

Though I'm not sure of the original author, this statement holds a great deal of truth. The faster the API consumers can understand the documentation, the faster they can use the API. If an API lacks proper documentation, it becomes challenging to use correctly, rendering it practically useless.

Although writing documentation is not the favorite for developers, it is an essential aspect, particularly for APIs. The documentation helps make developers' lives a lot easier, too. Like many fields in Computer Science, there's also some sort of standard for documenting API's called [OpenAPI Specificatio](https://swagger.io/specification/), which are widely adopted.

## Conclusion

Creating a user-friendly RESTful API requires careful planning to make sure it works well with modern applications. 
By following these helpful tips, you can design an API that runs efficiently, can handle more users, and is easy for developers to use. As technology changes, make sure to keep up with the latest ideas and add them to your API to keep it useful and up-to-date.

Happy coding !