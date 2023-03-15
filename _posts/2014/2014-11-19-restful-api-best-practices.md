---
title: "RESTful API Best Practices"
date: "2014-11-19"
categories: 
  - "development"
tags: 
  - "api"
  - "design"
  - "development"
---

The past few days I have read a number of articles outlining what they feel are best practices when designing RESTful APIs. I have put together some of the most common points that I feel directly impact good API design. This post is a work in progress, and will evolve as I dive deeper into the world of good API design.

### Nouns, not Verbs

A good API starts with its structure. The consumer of most APIs tend to be developers, so consistency and a nice interface goes a long way. The first step in good API design is having a good resource naming scheme. Too often people intermingle verbs with their resource names. A good API should be noun-based, and the verbs should be represented by HTTP methods.

**Bad**: GET /api/getProducts **Good**: GET /api/products

### API Versions

API versioning can be a point of contention. There are good and bad uses, and there are also appropriate and inappropriate time to make use of versioning. I feel that a well-designed API should not require a version, and that it is beneficial to maintain a consistent API through versions. That said, there are many cases where versioning is necessary such as major architectural changes, or paying customers who may not be able to keep their applications in lock step with your own. If you are going to make use of versioning in your API, do so in the URL and not in the header. All versions should be explorable, making headers a non-ideal option. Minor revisions can be expressed using special headers, and can also be expressed by adding new fields while leaving existing fields unchanged.

**URL**: /api/v1/products/1

### Plural or Singular

I have no strong feelings on plural versus singular naming, however the communal wisdom dictates that all resource locators should be plural in name. Regardless of what format you choose, be consistent throughout the application.

**Bad**: /api/products/1, /api/item/2 **Good**: /api/products/1, /api/items/2

### Nested Resources

Nested resources should only be created when there is a clear relationship between the two resources. It can be used to package convenience methods such as commonly made queries, and it can be used for filtering or sub-selection. The import concept to note is that nested routing should be refining a resource always.

**Bad**: /api/song/1/album/2 **Good**: /api/album2/song/1

### GET/HEAD/POST/PUT/DELETE

There is not much to say here - use the HTTP verbs whenever possible! They make your code easy to read and API easy to consume. There are some caveats which must be considered, such as proxies which prevent requests other than GET or POST, but for these, make use of a convention of setting a POST header which your software can decode and route appropriately.

| Action | Method |
| --- | --- |
| Retrieve an existing resource | GET /api/products/1 |
| Update an existing resource | PUT /api/products/1 |
| Create a new resource | POST /api/products/1 |
| Delete an existing resource | DELETE /api/products/1 |

### Status Codes

Whenever dealing with resources, always make sure you return an appropriate status code that follows standards. Most individuals are used to returning HTTP 200 OK or HTTP 400 BAD REQUEST  for their applications, but for not much additional cost you can provide a lot more semantic meaning in your application.

201 CREATED - Whenever a resource has been created. Include a URL to the new resource. 202 ACCEPTED - Let the user know their resource has been accepted for processing. 204 NO CONTENT - Let the user know the request succeeded, but the server has nothing to say (i.e. responding to a DELETE request).

### Error Codes

Continuing the discussion about status codes: we also need to tell the user when things didn't go so well. It is particularly important that your format your response consistently, correctly, and provide as much detail as possible. Often times, this is the only way that developers will be able to interact with your system and get meaningful information back.

400 BAD REQUEST - Malformed request payload such as invalid XML or JSON. 410 GONE - When a resource endpoint has been deprecated. 415 UNSUPPORTED MEDIA TYPE - When a content type was either incorrectly provided or the server cannot handle that content type. 422 UNPROCESSABLE ENTITY - When the request fails server-side validation.

### Payloads

Payload type doesn't matter in a real sense. Almost every framework and language will support the most common options: JSON and XML. I personally recommend choosing JSON as XML is unnecessarily verbose and has well known parsing problems. If you are going to support multiple types, I recommend going the Rails route and appending the type to the URL such as .xml or .json. This allows for browser explorability and simple visual inspection of the expected payload types.

### Security

In this day and age, it is unacceptable to not be using SSL. The overhead cost of SSL is trivial, and the security it can provide is invaluable. Certificates are cheap, and allow for the secure transmission of tokens, authentication, and of course your data.
